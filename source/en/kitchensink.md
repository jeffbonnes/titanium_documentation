<summary>
    This guide will help you get started with the Kitchen Sink demo application.  After reading this guide, you should be able to:
    
* Download and Install the Kitchen Sink app
* Be familiar with how Kitchen Sink is structured

</summary>

# Background

Kitchen Sink for Mobile is a developer companion application, showcasing the use of nearly every API and option available in the Titanium Mobile SDK. 

# Downloading Kitchen Sink

The Kitchen Sink source code is hosted on [GitHub](http://github.com/appcelerator/KitchenSink).

You have two options for accessing the source:

* You can clone the project using the git SCM system (if you are a git user)
* You can download the project directly by clicking on the "Download Source" button (pictured below)

![github](http://developer.appcelerator.com.s3.amazonaws.com/documentation-examples/kitchensink_download.png)

# Importing Kitchen Sink into Titanium Developer

Once you have downloaded the source code for Kitchen Sink, you must import the project into Kitchen Sink. Click the "Import Project" button below and navigate to the root directory of the source code folder you downloaded.

![import](http://developer.appcelerator.com.s3.amazonaws.com/documentation-examples/kitchensink_import.png)


Once imported, your project should show up in the left pane of Titanium Developer.

![import2](http://developer.appcelerator.com.s3.amazonaws.com/documentation-examples/kitchensink_import2.png)


If you run into problems running the imported project, it is possible the build artifacts for the package you downloaded are causing issues on your system. If this happens, simply create a new project in Titanium Developer and copy the contents of the Kitchen Sink Resources directory into the Resources directory of the new project. This should enable you to run all of the examples.

# The structure of Kitchen Sink

Kitchen Sink is structured to make it fairly easy to find various functionality of the Titanium API.

![ks](http://developer.appcelerator.com.s3.amazonaws.com/documentation-examples/kitchensink_iphone.png)

Kitchen Sink is structured with the following main tabs:

* **Base UI** - the Base UI tab contains various user interface examples and tests.
* **Controls** - the Controls tab contains various Widget user interface examples.
* **Phone** - the Phone tab contains various device specific examples.
* **Platform** - the Platform tab contains various platform specific examples.
* **Mashups** - the Mashups tab contains various examples with working with remote data.

# Troubleshooting

Importing and running the Kitchen Sink should work for you straight away, but if you run into problems you can create and run a 
brand new Titanium Mobile project and copy the demo code over into it.

First, create a new Titanium Mobile project and run it in the emulators to satisfy yourself that it works.  Now,
replace everything but `[project root]/build` in your newly created application with the contents from the Kitchen Sink demo 
(Resources directory, custom Info.plist, tiapp.xml - basically everything from the old Kitchen Sink except for the 
`[project root]/build` directory).  Now when you re-run your newly created project, you will have all the demos up and running.

