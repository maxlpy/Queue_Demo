# HW #3 Proxies, Queues, Cache Fluency (Option2).

## Building Infrastructure

### Setup

* Clone this repo, run `npm install`.
* Install redis and run on localhost:6379

### A simple web server

Use [express](http://expressjs.com/) to install a simple web server.

	var server = app.listen(5000, function () {
	
	  var host = server.address().address
	  var port = server.address().port
	
	  console.log('Example app listening at http://%s:%s', host, port)
	})

Express uses the concept of routes to use pattern matching against requests and sending them to specific functions.  You can simply write back a response body.

	app.get('/', function(req, res) {
	  res.send('Welcome to this Server.')
	})

#### My installed servers
	
	node Server_1.js(port on 5001)
	node Server_2.js(port on 5002)
	node proxy.js   (port on 5000)

### Redis

You will be using [redis](http://redis.io/) to build some simple infrastructure components, using the [node-redis client](https://github.com/mranney/node_redis).

	var redis = require('redis')
	var client = redis.createClient(6379, '127.0.0.1', {})

In general, you can run all the redis commands in the following manner: client.CMD(args). For example:

	client.set("key", "value");
	client.get("key", function(err,value){ console.log(value)});

### An expiring cache

Create two routes, `/get` and `/set`.

When `/set` is visited, set a new key, with the value:
> "this message will self-destruct in 10 seconds".

Use the expire command to make sure this key will expire in 10 seconds.

When `/get` is visited, fetch that key, and send value back to the client: `res.send(value)` 


### Recent visited sites

Please see this funciton on `http://localhost:5001/recent`. It will show you all your recent visited urls.


### Cat picture uploads: queue

Implement two routes, `/upload`, and `/meow`. The function is implement on port 5000, 5001, 5002.
 
Use curl to upload a picture using below command and then visit the page, you will see this picture.

	curl -F "image=@./img/hairypotter.jpg" localhost:5000/upload

In this process, `upload` store the images in a queue, `meow` display the most recent image to the client and *remove* the image from the queue.

### Proxy server

I have created a proxy sever on `http://localhost:5000`. When you visit this , you will see a changed url which is `http://localhost:5001/meow` or `http://localhost:5002/meow`. The proxy server will direct to two different urls automatically.
