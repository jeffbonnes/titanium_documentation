<summary>
This guide will show you how to interact with data from remote services.  By the end of this guide, you should
understand:

* How to interact with remote servers through HTTPClient
* How to work with retrieved data in an XML or JSON format
* How to download and upload files
* The steps necessary to work with SOAP web services
* Working with HTTP headers

</summary>

# Remote Data in Titanium

Your Titanium application can interact with remote servers over [HTTP](http://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol)
using the `HTTPClient` object provided through the `Titanium.Network` namespace.  HTTPClient's API mirrors that of the XMLHTTPRequest
object in the web browser, so if you have done any Ajax programming in the browser (outside of libraries like Dojo or jQuery,
which use XMLHTTPRequest), HTTPClient will be familiar to you.

# HTTPClient and RESTful resources

It is possible to use HTTPClient to interact with many popular types of web services, but the easiest form to work
with are REST-style web services.  Defining and explaining RESTful web services is beyond the scope of this guide, but
[you can learn more about REST here](http://en.wikipedia.org/wiki/Representational_State_Transfer).  For our purposes, it is
sufficient to understand that a 'resource' is some bit of data on the web, identified by a
[URI](http://en.wikipedia.org/wiki/Uniform_Resource_Identifier).  Most commonly, your mobile application will interact with
this data on the web using HTTP GET or POST requests (though the full range of HTTP verbs are supported by HTTPClient).

## GET requests

Making a GET (or any other type of) request to a resource on the web consists of three steps:

* Creating an HTTP Client
* Opening an HTTP connection to a specified resource
* Sending an HTTP request

Here is a simple example of this using Titanium's JavaScript API:

	var xhr = Ti.Network.createHTTPClient(); // returns an instance of HTTPClient
	xhr.open('GET','http://www.google.com');
	xhr.send();

Most of the time, simply sending the request is not useful to your application.  You are likely interested in the data the
server will respond with, which is available in the response body.  In order to access this data, you can specify callback
functions to be executed at specific points in the lifecycle of the request.
[There are many events you can assign callbacks for](http://developer.appcelerator.com/apidoc/mobile/latest/Titanium.Network.HTTPClient-object.html),
but the most common are `onload` and `onerror` - the former being called after a response from the resource has been successfully
received, and the latter being called if there is an error in trying to retrieve the desired resource.

	var xhr = Ti.Network.createHTTPClient(); // returns an instance of HTTPClient

	xhr.onload = function(e) {
		//this fires when your request returns successfully
		//this.responseText and this.responseXML
		//will be used often
	};

	xhr.onerror = function(e) {
		//this fires if Titanium/the native SDK cannot successfully retrieve a resource
	};

	xhr.open('GET','http://www.google.com');
	xhr.send();

The handling of network communication is handled asynchronously, since you would not want your application to hang while waiting on an
HTTP request to return.

## POST requests

Often you will need to send data to the server in the body of your request, as you would in a standard HTML form.  This is typically
accomplished via a POST (or PUT) request.  Titanium provides an easy way of sending along a POST body with a request, automatically
serializing JavaScript object graphs into form-encoded POST parameters:

	var xhr = Ti.Network.createHTTPClient();

	xhr.onload = function(e) {
		//handle response
	};

	xhr.open('POST','http://www.myblog.com/post.php');
	xhr.send({
		title:'My awesome blog',
		body:'Today I met Susy at the laundromat.  Best day EVAR!'
	});

You can also send arbitrary string data as the body of your post by passing a string to `send`:

	xhr.send('<some><xml><data></data></xml></some>');

# Working with data

In your lifecycle events, you will often need to examine data sent back from the server in your application.  The HTTPClient object
will have useful properties set on its self to allow you to consume the data returned from the server as necessary.

## Retrieving and parsing JSON

The best data transport format for use with JavaScript (and hence Titanium) is JavaScript Object Notation, or `JSON`.  JSON is a great
fit for JavaScript applications since it can very easily be serialized into and out of JavaScript Objects.  Moreover, since JSON is
such a terse transport format, it takes less less time and less bandwidth to transfer over the air, which can become important on low-speed
data networks.  For more on JSON, [check out the official website](http://json.org).

Titanium has built-in support for JSON serialization in the `JSON` namespace.

	var myObject = {
		foo:'bar',
		stuff:[1,2,3]
	};

	//deserialize...
	var myObjectString = JSON.stringify(myObject);
	// returns '{"foo":"bar","stuff":[1,2,3]}'

	//re-serialize...
	var myNewObject = JSON.parse(myObjectString);
	// myNewObject.stuff[1] === 2

If you have a server-side resource (web service) that has a JSON response format, you can very easily serialize that response
inside HTTPClient's `onload` function.  The data returned from your web service will be stored as a property of the HTTPClient
[object](http://developer.appcelerator.com/apidoc/mobile/latest/Titanium.Network.HTTPClient-object.html), so it is accessed and
parsed like so:

	xhr.onload = function(e) {
		var myData = JSON.parse(this.responseText);
	};

## Retrieving and parsing XML

As was stated earlier, JSON is the preferred format for data transport in a Titanium application, since it is easy to consume
inside your JavaScript application and it is a very compact format.  However, many existing applications maintain an XML-based
interface that you must work with in your client application.  Titanium provides facilities for consuming XML by providing an
[XML DOM Level 2 implementation](http://www.w3.org/TR/DOM-Level-2-Core/), and automatically serializing XML responses into one
of our [XML Document objects](http://developer.appcelerator.com/apidoc/mobile/latest/Titanium.XML.DOMDocument-object.html).  All
XML-related functionality in Titanium is contained in the `Titanium.XML` namespace.

Inside your handler function, if your response has an XML `Content-Type` header, Titanium will automatically serialize the response
text into XML for your use:

	xhr.onload = function(e) {
		var doc = this.responseXML.documentElement;
		//this is the XML document object

		//Use the DOM API to parse the document
		var elements = doc.getElementsByTagName("someTag");
	};

For more examples of parsing XML documents, [check out the XML DOM examples in the Kitchen Sink](https://github.com/appcelerator/KitchenSink/blob/master/KitchenSink/Resources/examples/xml_dom.js).

# Working with files

A common need in a mobile application is to upload a file (like an image) to a remote server.  You might also need download a file
and store it locally on the device.  Luckily, both are easily accomplished with Titanium.

## File upload

Assuming you have a server-side service which accepts file uploads, you should find upload fairly straightforward.  Titanium handles the
setting of headers and marshaling POST parameters for you, so you simply need to pass a Titanium [blob](http://en.wikipedia.org/wiki/Blob_(computing))
object to `send()`.  A blob is returned by many different Titanium APIs, including `Titanium.Filesystem.File.read`.  Below, you will find an example
of how you might select a photo from the device photo gallery, and upload that blob to a web service:

	Titanium.Media.openPhotoGallery({
		success:function(event) {
			var blob = event.media;
			var xhr = Titanium.Network.createHTTPClient();
			xhr.onload = function(e) {
				Ti.UI.createAlertDialog({
					title:'Success',
					message:'status code ' + this.status
				}).show();
			};
			xhr.open('POST','https://myserver.com/api/uploadAndPost.do');
			xhr.send({
				theImage:blob,
				username:'foo',
				password:'bar'
			});
		}
	});

## File download

Occasionally, you will also need to download and store a file from a remote server.  This is as simple as specifying a filesystem path
where you would like HTTPClient to store the file.  Below is an example of how to fetch a file from a server and store it
in the `applicationDataDirectory`, the primary location for any read/write Filesystem IO in a Titanium application:

	var c = Titanium.Network.createHTTPClient();
	c.onload = function() {
		Ti.API.info('PDF downloaded to applicationDataDirectory/test.pdf');
	};
	c.setTimeout(10000);
	c.open('GET','http://www.appcelerator.com/assets/The_iPad_App_Wave.pdf');
	c.file = Titanium.Filesystem.getFile(Titanium.Filesystem.applicationDataDirectory,'test.pdf');
	c.send();

# SOAP Web Services

In some enterprise settings, "Simple" Object Access Protocol (SOAP) is the format for XML data returned by a web service.
SOAP web services are very much possible in Titanium, though they are the least simple option.

## Avoid SOAP if you can

Although you can use SOAP web services (this may be your only option, especially if you are dealing with a 3rd party or legacy interface),
it is recommended to avoid using SOAP web services in a Titanium application.  SOAP retains the disadvantages of XML:

* The overhead of XML over the wire
* The need to translate from an XML format to a JavaScript object format

And compounds them because SOAP is even more verbose (much more XML being transported over the wire), and the results are even more difficult
to parse.  Some programming languages provide high-level tools, WSDL parsers, and other mechanisms to work around the complexities of a SOAP
format, but JavaScript has historically never had any of those types of tools.  This remains the case today, and as such, there are very few
high-level libraries and tools to support SOAP in JavaScript.

## The low-tech approach

The approach taken by a number of Titanium projects we have worked on is to stay very low-tech and POST manually-created SOAP envelopes (XML
strings) to a web service endpoint.  If you understand how HTTP and SOAP work together, you can manually construct a SOAP envelope to send
to your server, with the appropriate contents:

	var client = Ti.Network.createHTTPClient();
	client.onload = function() {
		var doc = this.responseXML.documentElement;
		//manually parse the SOAP XML document
	};

	var soapRequest = "<?xml version=\"1.0\" encoding=\"UTF-8\"?> \n" +
 	"<SOAP-ENV:Envelope xmlns:SOAP-ENV=\"http://schemas.xmlsoap.org/soap/envelope/\" \n" +
		"xmlns:SOAP-ENC=\"http://schemas.xmlsoap.org/soap/encoding/\" \n" +
		"xmlns:xsi=\"http://www.w3.org/2001/XMLSchema-instance\" \n" +
		"xmlns:xsd=\"http://www.w3.org/2001/XMLSchema\" \n" +
		"xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" \n" +
		"xmlns:wsse=\"http://schemas.xmlsoap.org/ws/2002/12/secext\"> \n" +
		"<SOAP-ENV:Body id=\"_0\"> \n" +
			"<GetUserDetailsReq> \n" +
				"<Request> \n" +
					"<SessionToken xsi:type=\"ns:IVRSessionToken\">XXXX</SessionToken> \n" +
				"</Request> \n" +
			"</GMGetUserDetailsReq> \n" +
		"</SOAP-ENV:Body> \n" +
	"</SOAP-ENV:Envelope>";

	client.open('POST', 'https://someserver.com/someendpoint.asmx');
	client.send({xml: soapRequest});

Bear in mind the above SOAP envelope is completely made up and derived from another service.  In order to use your own SOAP web services in
this fashion, you will need to understand what the contents of a SOAP request to your server actually looks like as an HTTP request.  Here,
other third party tools can help, particularly ones that let you inspect the raw HTTP requests and responses for your web service.  On the
Mac, you might consider using [SOAP Client](http://ditchnet.org/soapclient/).  The Eclipse Web Tools project also has a bit of SOAP
[oriented tooling](http://www.eclipse.org/webtools/ws/).

## Using the Suds library

Also possibly useful is the [Suds client library for Titanium](http://github.com/kwhinnery/Suds).  It is an unofficial library and not
in any way supported by Appcelerator, but it may be useful as a reference in building your own SOAP client.

# HTTP headers

It is often necessary to manually add HTTP headers to your requests.  This can be accomplished easily by using the `setHeader` function
on HTTPClient.  __NOTE: HTTP Headers must be set AFTER client.open(), but before client.send(), as below:__

	var client = Ti.Network.createHTTPClient();
	client.open('POST','http://someserver.com/files/new');
	client.setHeader('Content-Type','text/csv');
	client.send('foo,bar,foo,bar');

