---
date: '2009-03-17 18:11:19'
layout: post
slug: twitter-http-rest-api-invocation-infrastruture-using-data-pipelines
status: publish
title: Twitter / HTTP / REST API Invocation Infrastruture using data pipelines
comments: true
wordpress_id: '558'
categories:
- Internet and Social Media
- programming
- python
- software
- web
tags:
- http
- rest
- twitter
---

This blog post is not about twitter API programming, though thats what it does deal with. It focuses on the intermediate level infrastructure (ie. higher than the HTTP/REST APIs exposed by other sites but lower than the class libraries that surround those) necessary to work with HTTP/REST based APIs being offered by various web sites. 

I have been working on scouring and analysing twitter data for which I have been having to work with continuously accessing twitter on a sustained basis for a number of days. I started refactoring my code recently, and this blog post details the results of that exercise. More specifically in the context of invoking HTTP APIs it deals with the following aspects (many of them which are specifically introduced due to twitter.




	
  * Ability to make HTTP Calls and deal with HTTP error conditions

	
  * Ability to deal with Connection and other failures and allow for auto retries

	
  * Ability to make HTTP Calls in bulk in batches

	
  * Ability to spawn HTTP Calls across multiple threads

	
  * Ability to respect and deal with API Rate Limitations



I have specifically focused on creating a pipelined design which might be an interesting design aspect for many in this situation, and have primarily relied on python based generators for the same though similar functionality could be built using Iterators in other languages as well.

**Accessing sites using HTTP Basic Authentication**

Twitter is accessed using Python urllib2. Since many API's often require using basic authentication, we shall set up the HTTP Opener to be able to work with such sites.
To be able to initialise the opener we define a method `get_opener()` which accepts the username, password, realm and host to be able to setup the opener for the site.


    
``` python
    def get_opener(username,password,realm,host):
    	"""
    	The default urllib2 opener is adequate for non authenticated http api requests.
    	However twitter API requires basic authentication in many situations. Hence
    	this method initialises the opener to use the appropriate user id / password
    	parameters
    	"""
    	headers = {}
    	basic_auth = base64.encodestring('%s:%s' % (username, password))[:-1]
    	headers['Authorization'] = 'Basic %s' % basic_auth
    	handler = urllib2.HTTPBasicAuthHandler()
    	handler.add_password(realm, host, username, password)
    	opener = urllib2.build_opener(handler)
    	opener.addheaders = headers.items()
    	return opener
```    



**Making a Basic HTTP call to fetch JSON data**

Twitter supports data both in XML and JSON formats. I tend to prefer json since it is much more compact on the network and is more efficient to parse and construct as well. As a result I have function which extracts the data from a URL as presented below. It takes the opener and the URL and returns a constructed data object. Note the class Data which has no predefined attributes but instead dynamically adds attributes to itself based on JSON attributes. Moreover it checks whether the return parameter is a single object or a sequence and acts correspondingly_The logic actually should work with nested dictionaries (dictionaries within dictionaries) as well, which is currently a pending exercise_


    
``` python
    class Data(object):
    	"""
    	This is a generic data object. It is a completely dynamic object meant to 
    	create itself from a dictionary or a set of nested dictionaries. All keys 
    	in the dictionary are converted to object attributes allowing for an easier 
    	and more intuitive access to the object attributes.
    	"""
    	def __repr__(self):
    		return "Generic Data Object:%s" % self.__dict__.__repr__()
    	def __str__(self):
    		return "Generic Data Object:%s" % self.__dict__.__str__()
    	
    	@classmethod
    	def load_from_json(self,json):
    		data = Data()
    		for key,val in json.items():
    			data.__dict__[key] = val
    		return data  #to allow chaining of calls
    
    def http_to_json(opener,url):
    	"""
    	This method invokes the remote URL which is expected to revert with a JSON datastream. It returns
    	an object with the json keys being attributes of the object along with the corresponding values.
    	"""
    	try:
    		response = opener.open(url)
    		stream = response.read()
    		# A JSON stream begins with a { or a [
    		if (stream[0] == '{') or (stream[0] == '[') :
    			jsonobj = simplejson.loads(stream)
    			if jsonobj :
    				# Some twitter API return an 'error' in case of error
    				if stream[0] == '[' :
    					data = [Data.load_from_json(obj) for obj in jsonobj]
    				else:
    					data = Data.load_from_json(jsonobj)
    	except urllib2.URLError, e:
    		print 'Received Error while fetching twitter record for url : %s = %s' % (url, e)
    		raise e
    	return data
```    



**Simple Client**

I think we have enough infrastructure methods at this stage to initiate a simple HTTP invocation to give us an idea of how these could be used. In the code below twitter_user_id and twitter_password will need to be changed to your user id and password. This shows you how a simple twitter call could be made.


    
``` python
    if __name__ == "__main__":
    	opener = get_opener('twitter_user_id','twitter_password','Twitter API','twitter.com')
    	data = http_to_json(opener,"http://twitter.com/account/rate_limit_status.json")
    	print data
```    



**Automatic Retries**

We do know that network communications are not perfect and occasionally we get errors which are not due to the data but due to the network or technical infrastucture. Errors that are unlikely to be repeated if the same function is retried again. Hence we work out a common retry function which will retry certain function invocation based on various retry policy parameters. Note that retry strategies cannot be used when the functions are not idempotent, especially if it is not possible to ascertain if a invocation has been received and processed by the server. However most retrieval calls are idempotent (except for the side effect of twiter api rate limits still getting impacted), so we should be able to use the retry logic in most cases. The retry method is as follows.


    
``` python
    def retry(max_tries, check_error_func, break_codes, continue_codes, retry_policy, func,args):
    	"""
    	This is a wrapper which runs a function in a retry loop . In this case it is assumed to be
    	a function thats dealing with HTTP. The arguments are as follows :
    	
    	max_tries : maximum number of tries before giving up
    	check_error_func : function to check if a repeatable occurred. (Repeatable errors should
    		not be retried. Plugin for custom logic.
    	break_codes : sequence of http error codes on which no retry should be attempted
    	continue_codes : sequence of http_error codes on which retry should be attempted
    	retry_policy : default policy in case of http code not matching either of the two sequences
    	func : function to be performed / executed
    	args : arguments to the function
    	"""
    	try_num = 1
    	success = False
    	data = None
    	e = None
    	while try_num <= max_tries :
    		data = None
    		e = None
    		try :
    			data = func(*args)
    		except Exception, e :
    			pass
    		if check_error_func(data,e) : break
    		if not check_retry(data,e,break_codes, continue_codes, retry_policy) : break
    		try_num += 1
    	if e : raise e 
    	else : return data
```    



There are still some missing gaps we need to fill in in this case. The check error function is domain specific (in this case twitter specific) which checks for an attribute called error in the data object as follows.


    
``` python
    def is_twitter_error(data,e):
    	"""
    	Method to check if the contained data is actually an error based on twitter return
    	value semantics
    	"""
    	if hasattr(data,'error') :
    		return True
    	return False
```    



We also know that http error code 404 means a not found, and retrying is unlikely to help that. However code 520 is often received when twitter is over capacity and 101 is received in case of network communication failures, good candidates for retrying the http calls. Finally for other error codes I preferred to have a default policy as retry though your choice could be different. Thus the final invocation along with the retry logic looks like follows 


    
``` python
    rate_status = retry(3, is_twitter_error,(404),(520,101),True,get_rate_status,(opener,))
```    



**Bulk Processing Twitter IDs**

It is often useful to do processing in bulk, especially when one does have bulk data to process. In such a situation the starting point is the list of data elements to be processed. We shall be attempting to retrieve twitter data in bulk for a large number of user ids. So the starting point is a generator which can return a list of user ids that we need to fetch. For sake of simplicity, I have written a generator which will generate twitter ids as a sequence of numbers, though you could replace it with a more suitable generator as appropriate


    
``` python
    def gen(min,max):
    	"""
    	A generator to generate twitter ids
    	"""
    	counter = min
    	while counter < max :
    		yield counter
    		counter += 1
```    



**Batching**

In case of bulk processing, it is often useful to break down the data in a set of batches. To be able to do that we need a generator which can generate a set of batches from an underlying generator of ids (defined above). Usually the batch size could've been fixed, but in case of twitter, API rate limits apply and hence sometimes the batch sizes need to be computed dynamically depending upon the remaining available API calls. Thus we first define a function which decides on the batch size based on the preferred batch size and remaining hits as per twitter. It also uses a keep_unused parameter to ensure that a certain number of API calls are left unused (ostensibly for usage by other programs)


    
``` python
    def twitter_batch_sizer(opener,max_size, keep_unused):
    	"""
    	Method to generate a batch size based on available rate limit, maximum batch size, and number
    	of API calls to keep as unused
    	"""
    	while True :
    		rate_status = retry(3, is_twitter_error,(404),(520,101),True,get_rate_status,(opener,))
    		if rate_status : 
    			size = rate_status.remaining_hits - keep_unused
    			if size > max_size : size = max_size
    			elif size < 0 : size = 0
    			yield size
    		else : yield 0
```    
    



Now we also need to create a generator which will generate batches using the underlying generator. However I did run into a peculiar problem. When the underlying generator as completed (StopIteration), the batch generator also needs to complete processing. This required setting up of a booleanfunction attribute, but since function attributes are not mutable, I had to define a BooleanWrapper class which internally contains a boolean attribute and has a false() method which sets the attribute value to false. The new class along with the Batch generator are shown below.The sizer and generator arguments are functions / generators that we have already defined earlier.


    
``` python
    class BooleanWrapper(object):
    	"""
    	This is just a dummy object to represent a boolean. It is wrapped in an object
    	since function attributes are not mutable from within a function body. However calling a
    	method on an object to change its internal attribute works.
    	"""
    	def __init__(self):
    		self.status = True
    	def false(self):
    		self.status = False
    			
    def batcher(sizer,gen):
    	"""
    	This object takes a sizer function which computes a size of a batch dynamically, and creates
    	a batch out of the generator function supplied by gen. Thus it effectively breaks down the 
    	data supplied by gen into individual potentially unequally sized batches as specified by the
    	sizer function
    	
    	To put it differently, it takes a generator and outputs a generator of generators, each 
    	second level generator being a batch
    	"""
    	loop = BooleanWrapper()
    	def lot(sizer,gen):
    		counter = 0
    		size = sizer.next()
    		if size == 0 :
    			loop.false()
    			raise StopIteration
    		while counter < size :
    			try :
    				x = gen.next()
    				yield x
    				counter += 1
    			except StopIteration, e:
    				loop.false()
    				raise StopIteration
    	while loop and loop.status:
    		try :
    			yield lot(sizer,gen)
    		except StopIteration, e :
    			raise StopIteration
```    
    



**Threading**

Now that we have batching support, we might like to be able to multithread the calls. In my situation, I was whitelisted by Twitter so had an API rate limit of 20,000 calls per hour. However I prefer to get the API calls done as soon as possible (say in the first 20 minutes of an hour), and use the remainder of the time to do additional processing. Being able to process 1000 API calls per minute requires support of either multiprocessing or multi threading. Since I was interested in only having one executable (from a manageability perspective) I decided to use the multi threading approach. Note that python has the Global Interpreter Lock (GIL) issue, but thats unlikely to play a role since threads easily yield the GIL to each other when they reach blocking IO points and most HTTP calls are indeed blocking IO. 

Having decided on threading, I decided to spawn a set of threads per batch, process the batch, shutdown the threads and continue processing. There are more efficient ways of maintaining a continuously running thread pool, but given the limitations of the rate limit APIs, even the implementation I chose is far more than adequate as far as efficiency goes. 

Anyway for any threading operations we either need a Thread subclass with a run method or a function which will be executed in a thread. Given the simplicity of this situation I chose to use a function to retrieve a particular user's information as follows. This function shall be invoked simultaneously by different threads for different user ids.


    
``` python
    def threadfunc(threadid, queue, process_func, func_args, global_dict):
    	"""
    	This method runs takes data item from a queue, invokes another function as specified by
    	process_func and its arguments func_args, updates the result in the dictionary global_dict
    	with the key being the data item. Since this function is to be intended to be run as a 
    	standalone thread, the first argument threadid, is a identifier for the thread purely for
    	debugging purposes
    	"""
    	print "Thread %s started " % threadid
    	while not queue.empty() : 
    		try :	  
    			val = queue.get_nowait()
    			args = [arg for arg in func_args]
    			args.append(val)
    			result = process_func(*args)
    			print "Thread %s processed %s with result %s" % (threadid,val,result)
    			global_dict[val] = result
    		except Exception, e:
    			print "Unexpected error in thread %s for value %s : %s " % (threadid, val, e)
    		queue.task_done()
    	print "Thread %s exited " % threadid
```    



Note that I added the print statements just so that you can follow the program when it runs. These are not a part of the final code. It is quite important here to be able to handle all exceptions, and not leave any exceptions unhandled. Unhandled exceptions result in the main program getting terminated, something that is probably not such a great thing to have for a continuously running program. However I haven't really included any error handling code here apart from a simple print statement.

If you noticed this function still doesn't contain our actual twitter userid fetch logic. Thats in another function which is passed as a parameter to the above function


    
``` python
    def resultfunc(opener,val):
    	"""
    	Method to generate user data along with the retry and twitter specific error testing
    	"""
    	user_data = retry(3,is_twitter_error,(404),(520,101),True,get_user_data,(opener,val))
    	return user_data
```    



Having written a function that can be executed in a thread which will retrieve user data, we now need a function which will take a batch, spawn a bunch of threads, feed the data to the threads through a queue, and allow the threaded function to process each element in the queue concurrently. This function is as follows.


    
``` python
    def batch_threader(size,gen, process_func, process_func_args):
    	"""
    	This is a generator which expects a two level generator (generator which returns a generator). 
    	The top level returns a set of batches, whereas the nested generator supplies a set of 
    	individual items in the batch. It iterates through all the batches, and for each batch spawns 
    	a thread pool of the requested size and executes the function process_func along with the
    	supplied arguments on each of the items concurrently in the thread pool. It finally returns a
    	dictionary with each item being the key and the result of the processing being the value.
    	"""  
    	batch_number = 0
    	for batch in gen :
    		batch_number += 1
    		print "Running Batch : %s with pool size %s" % (batch_number, size)
    		# Construct a list to figure out size of queue
    		items = []
    		for item in batch :
    			items.append(item)
    		
    		# Initialise queue	
    		queue = Queue.Queue(len(items))
    		
    		# Push Items into the queue	
    		for item in items :
    			queue.put(item)
    			
    		# Start threads
    		result_dict = {}
    		for i in range(size) :
    			t = threading.Thread(None,threadfunc,None, 
    					(i,queue, process_func, process_func_args, result_dict))
    			t.setDaemon(True)
    			t.start()
    		
    		# Wait for all threads to complete processing
    		queue.join()
    		yield result_dict
```    



**Putting it all together**
Finally here's the main body which actually assembles the data pipeline and triggers the full processing. Since the typical API rate limit is 100, I have set the keep_unused to 90 (so 10 API calls are likely to be used). I also set the preferred batch size as 6 and thread pool size as 3. This should result in two batches being fetched, the first of size 6 and the next of size 4 (assuming your API rate limit is at 100) through a threadpool of 3.


    
``` python
    if __name__ == "__main__":
    	opener = get_opener('twitter_user_id','twitter_password','Twitter API','twitter.com')
    	for results in batch_threader(3, batcher(twitter_batch_sizer(opener,6,90),gen(0,10)), resultfunc,(opener,)) :
    		print "====== Begin Batch ======="
    		for key, val in results.items() :
    			print "%s was processed with result %s" % (key,val)
    		print "====== End Batch ======="
```    




**In summary**

I would've actually preferred to blog at length about the various design characteristics, but since this post has become really long, I shall probably come back to it in the coming few days. However I would encourage you to note the brevity (about 175 lines of non commented source code) in which we have been able to achieve a lot, and a clear separation of the infrastructural and twitter specific code. Moreover its an example of really putting together a lot of diverse functionalities and capabliities by using building blocks which can be plugged together to work with each other (thanks in no small part to the function objects and generator support of python). The full listing is in the file [twitter-http-rest-api.py](http://blog.dhananjaynene.com/wp-content/uploads/2009/03/twitter-http-rest-api.py). 
