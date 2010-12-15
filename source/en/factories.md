<summary>
Titanium has a factory-style interface for creating UI components.  As your application grows larger,
you may consider implementing your own UI factories to extend Titanium base components and create reusable
components for your application.  This guide will show you how.
</summary>

# Titanium and UI factories

As you are no doubt aware, Titanium's UI APIs have a factory-style interface:

	Ti.UI.createView({
		height:400,
		backgroundColor:'red'
	});
	
As you develop your own applications, you will find that you are creating specialized versions of these components,
which may or may not be used all over your application.  Good JavaScript programming practices dictate that you would
try and encapsulate any reusable logic for your application into a single JavaScript namespace, so as
[not to pollute the global scope](javascript_best_practices.html):

	var myApp = {
		someFunction: function() {
			//do stuff
		},
		someVariable: true
	};
	
To your application's namespace, you should consider adding factory-style functions that create all of your UI components as well.
This will allow you to keep your application [component oriented](http://en.wikipedia.org/wiki/Component-based_software_engineering),
and encourage you to maintain (and test!) public interfaces for all of your custom components.  Using a factory-style implementation
for your UI components will also allow you to effectively extend Titanium UI components as needed.  Let's say your application uses
a header view that will be displayed in multiple windows.  You might create a header factory function like:

	myApp.ui = {}; //create a UI namespace within your app
	
	myApp.ui.createHeaderView = function(/*Object*/ _args) {
		var v = Ti.UI.createView({
			backgroundColor:_args.bgColor||'red',
			height:80
		});
		v.add(Ti.UI.createLabel({
			text:_args.title||'My Cool App'
		}));
		return v;
	};
	
Designing your interface in this way has a number of advantages.  You can reuse this code in multiple places in your application, and
since you take a JavaScript object as an argument, you can easily allow your custom control to be configured by the consumer of your API.
You'll also notice that for the sake of uniformity, I've named the function in a style similar to the UI creation functions in the Titanium
namespace - "create"+view type.  When creating your own functions, you might want to take this convention on step further by adding
the Titanium 'base' class to the end of your component type.  So you might have function names that follow the convention
"create"+business view type+Titanium base component:

* `createHeaderView`
* `createLoginButton`
* `createMainApplicationWindow`
* `createRssFeedTableView`

# Adding an API to your views

Your custom components will also often need to implement an external API, just like Titanium components do.  In order to add API functions to
a custom component, you simply add functions to the base object returned by your factory function:


	myApp.ui.createHeaderView = function(/*Object*/ _args) {		
		var v = Ti.UI.createView({
			backgroundColor:_args.bgColor||'red',
			height:80
		});
		var l = Ti.UI.createLabel({
			text:_args.title||'My Cool App'
		});
		v.add(l);
		
		//external API function
		v.blink = function() {
			v.animate({opacity:0,duration:1000},function() {
				v.animate({opacity:1,duration:1000});
			});
		};
		
		return v;
	};
	
Now, you can invoke functions on your custom component like so:

	var h = myApp.ui.createHeaderView();
	h.blink(); // will cause the view to fade out, then in
	
# Instrumenting your view with events

In addition to a programmatic API, you will likely want to allow your component to dispatch component- or application-specific events.  Building on
the previous example, let's say that we might be interested to know when the `blink` animation is complete when we include this
header component in our app.  This is easy to accomplish using custom events attached to UI components.  First, we slightly
modify our `blink` function to fire an event on the 'base' view when the animation is complete:

	v.blink = function() {
		v.animate({opacity:0,duration:1000},function() {
			v.animate({opacity:1,duration:1000});
			//now, fire an event on yourself to let any
			//interested parties know this animation is complete
			v.fireEvent('blinkComplete');
		});
	};
	
With this simple change, now anyone who creates a header view can subscribe to the `blinkComplete` event:

	var h = myApp.ui.createHeaderView();
	h.addEventListener('blinkComplete', function() {
		Ti.API.info('blink complete');
	});
	
	h.blink(); //console will print message from listener
	
These basic techniques will allow you to create component-oriented, event-driven applications in Titanium Mobile.  To take these concepts even
further and integrate JavaScript based component styling, check out the [Helium](http://github.com/kwhinnery/Helium) library for Titanium.
