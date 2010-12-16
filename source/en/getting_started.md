<summary>
This guide covers getting up and running with Appcelerator Titanium&trade;. After reading it, you should be familiar with:

* What the Titanium Platform is and what the SDKs provide
* Installing Titanium, creating a new Titanium application, and running the application in the simulator
* The structure of a Titanium application
* Where to go next after "Hello World"

</summary>

# Welcome to Titanium!

Thanks for checking out Titanium - we hope you'll have your first native application for desktop or mobile up and running
before dinner.  Before we get into installing and running Titanium, let's (very) briefly go over what the Titanium platform is,
how it works at a shallow level, and the kinds of capabilities you can expect to find.

![platform](http://developer.appcelerator.com.s3.amazonaws.com/documentation-examples/gs_overview.png)

## Titanium Desktop SDK

The Titanium Desktop SDK provides a runtime environment for creating native desktop applications in HTML, CSS, and JavaScript.
Titanium Desktop packages up your application source code (HTML/CSS/JavaScript) with a heavily augmented build of the popular
[Webkit](http://webkit.org) open source web browser engine.  Your Titanium Desktop application is basically a web page (or pages)
and a web browser packaged into a single executable program.  But your desktop app is more than just a local web page -
Titanium Desktop apps have access to enhanced native functionality, like file system access, media, the ability to run external
processes, native UI chrome, and more.  You can also package Ruby, Python, or PHP code with your application, further extending
the capabilities of your always-on desktop application.

## Titanium Mobile SDK

The Titanium Mobile SDK allows you to create, run, and package real native mobile applications for iOS, Android, and BlackBerry (beta)
devices using our cross-platform JavaScript APIs.  But unlike Titanium Desktop, where applications run inside a web browser engine,
Titanium Mobile applications are run against a standalone JavaScript engine which invokes native APIs.  As a developer,
you are in fact writing a native application - it's just that you're using cross-platform JavaScript rather than
non-portable Java or Objective-C.

Titanium Mobile apps use native UI and platform APIs, and run at close to full native speed.  The Titanium Mobile SDK works with the
native SDK tool chains to combine your JavaScript source code, a JavaScript interpreter, and your static assets into an application
binary that will be installed to an emulator or mobile device.  It's worth mentioning that you could write your application UI in
HTML and CSS, but typically you will use native UI components through a Titanium JavaScript API.

## Titanium Developer

Titanium Developer is a desktop application you will install on your computer to allow you to create, run, and package
Titanium Mobile or Desktop application projects.  Titanium Developer will also automatically keep your Mobile and Desktop SDK
installations up to date.  Titanium Developer is essentially a nice GUI over the top of scripts in the Desktop and Mobile SDKs
which create and run Titanium projects.

# Installing Titanium

<info>
Titanium Developer and the Mobile and Desktop SDKs are tested using the following operating systems:

* Mac OS X 10.6.4 (Snow Leopard)
* Windows 7, XP and Vista
* Ubuntu 9.10 (Karmic Koala)

You may find that other OS versions will work fine also, but these are the OS versions we test against.
</info>

To install the Titanium Developer application, navigate to the [Titanium Download Page](http://www.appcelerator.com/download)
and download the Titanium Developer installer for your operating system. If the download doesn't begin automatically, you can
manually select the download you are interested in.

After a push-button installation on your operating system, Titanium Developer will be available to launch.  The first time you run
Titanium Developer, it will automatically download and extract the most current versions of the Mobile and Desktop SDKs.
This can take some time.  For those of you interested in doing mobile development, you will also need to download and install
the native development SDKs for the devices you are targeting.

## Preparing for iOS development
For iOS you will need to have a Mac running OS 10.6 (Snow Leopard) and an iOS developer account (the account is free, but to run
on device, you will need to pay a $99/year fee for the iOS developer program).  Installing the iOS SDK is as easy as
[downloading the SDK and Xcode](http://developer.apple.com/iphone), mounting and running the disk image, and following the
onscreen instructions.

## Preparing for Android development
Before doing any sort of Android development, you will need [Sun/Oracle Java SDK (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
installed on your system.  Note that this is NOT the same as the Java Runtime Environment (JRE) installed on many computers.

<info>
Before you install anything, search your system for the `java` and `javac` executables, to determine whether they are already installed and are of the correct vendor and minimum version, which is Sun/Oracle 32-bit SDK 1.6. When found, run these commands:

~~~
/path/to/java -version
/path/to/javac -version
~~~

If you have multiple versions installed, you will need to ensure that the correct one is being used by default. To do this, run the following without the full path, and compare the results to those produced above:

~~~
java -version
javac -version
~~~

The default executables can be controlled by adjusting your system's PATH variable.
</info>

For Android, you will need to [download and unzip the Android SDK](http://developer.android.com/sdk/index.html) somewhere on your
system.  Once unzipped, run the `android` command from `[SDK HOME]/tools`.  This will bring up an attractive Java Swing UI which
will allow you to install the various Android SDK flavors.  In order to create a project, Titanium requires Android 1.6 (APIs level 4),
and version 6 of the Android tools or better to be installed through this tool:

![img](http://developer.appcelerator.com.s3.amazonaws.com/documentation-examples/gs_android.png)

You may want to download any additional Android OS versions you'd like to test against at this time.  Currently, Titanium Mobile supports
Android OS versions 1.6, 2.1, and 2.2.  OS versions are available for download under the "Available Packages" heading in the `android`
tool. It is definitely worth your time to browse the [Android SDK documentation](http://developer.android.com/guide/index.html) for more
on the tools available to you with the Android SDK.

## Preparing for BlackBerry development

At this time, the BlackBerry version of Titanium Mobile is only available to selected beta preview participants.  This will change
soon.  Be advised, however, that the BlackBerry version of Titanium Mobile requires a Windows environment, on which you will
install the Eclipse-based development tool chain provided by RIM.


# Hello World!

When Developer launches for the first time, you will be prompted to sign in with your Appcelerator Network account.  If you don't
have an Appcelerator Network account, you can create one using the form provided. Once you are signed in, you will be ready to
proceed with creating your first Titanium project.

![new project](http://developer.appcelerator.com.s3.amazonaws.com/documentation-examples/gs_new_project.png)

In the fields provided, you will specify:

* A project type - one of Mobile, Desktop, or iPad
* A project name - this will be the name that shows up on the Home Screen for a mobile app
* An application ID - this will be used for packaging/distribution later, and is usually specified in reverse-domain format
* A directory under which your project will be generated - if you specify `/Users/kevin`, your project folder is created as `/Users/kevin/YourProjectName`
* Your company/personal URL, whatever that may be
* A Titanium Mobile/Desktop SDK version to use - this specifies which build scripts and libraries will be used to generate and run your application

<note>
You may be prompted to specify a location for the Android SDK for a new mobile project, if this is your very
first project.  This will be the top-level folder of the Android SDK, containing the `tools` folder and other
Android platform artifacts.  You can change your SDK location later in Titanium Developer's "Profile" perspective.

![Android SDK](http://developer.appcelerator.com.s3.amazonaws.com/documentation-examples/gs_android_sdk.png)
</note>

Click the "Create Project" button to generate your new Titanium project.

## What just happened?
Titanium will generate the necessary files to run a Desktop or Mobile project in the directory you specified during
project creation.  Your new project will appear in Titanium Developer in a list on the lefthand side of the "Projects"
perspective.

![developer](http://developer.appcelerator.com.s3.amazonaws.com/documentation-examples/gs_edit_project.png)

<note>
Titanium Developer has three "Perspectives" - the first is the project perspective, where you will go to manage and run
your Titanium projects.  The second is the community perspective, which has Appcelerator news feeds and a sandbox to test
desktop code.  The third is a profile perspective, where you can edit your user account details and specify configuration
options for Titanium.
</note>

## Where's my code?
<img src="http://developer.appcelerator.com.s3.amazonaws.com/documentation-examples/gs_tiappxml.png" style="float:right;margin:0 0 20px 20px;"></img>
After your project is created, a starter project will be created for you in the directory you specified.  All project types share
a similar layout:

* A `build` directory, which contains the assets necessary for actually running your application code on your target OS(es).  This
directory can be safely ignored in version control, as it is dynamically generated by the Titanium SDK build scripts.
* A `Resources` directory, which contains your application source code and any other assets (images, files, etc.) you will ship with
your application.
* A `tiapp.xml` file, which contains static configuration for your application.

Titanium Developer does not (yet) provide a text editor, so it is expected that you will use the text editor or IDE of your
choice to actually write application code.  The Resources directory will already contain a simple application that you can run
from Titanium Developer right away.  [Check out the application project structure guide](app_structure.html) for more details.

## Running your application

Let's fire up the default application to make sure everything is working properly.  Under the "Test and Package" tab,
hit the launch button to fire up your application in a mobile emulator or a new desktop window, depending on your project type.

![run](http://developer.appcelerator.com.s3.amazonaws.com/documentation-examples/gs_android_testandpackage.png)

A default mobile application should look like this:

![android](http://developer.appcelerator.com.s3.amazonaws.com/documentation-examples/gs_android_emulator.png)

A default desktop application should look like this:

![desktop](http://developer.appcelerator.com.s3.amazonaws.com/documentation-examples/gs_inspector.png)

# What's Next?

Now that you have a functional Titanium environment, there are numerous guides available here to further your education.  The
following are the recommended next steps for mobile developers:

## The Kitchen Sink

There are numerous sample applications for you to look at, most notably the [Kitchen Sink](kitchensink.html) demo application.  The
Kitchen Sink works well as a companion during development, so you can pull out code samples and get an idea of what is possible in a
Titanium application.  [Check out the Kitchen Sink](kitchensink.html) right away if you haven't already, and keep it handy for all
your "how do I..." type questions.

## Titanium Fundamentals

Before you can progress too far into a Titanium application, you will probably want to understand the
[contents and structure of a Titanium Mobile application](app_structure.html).  This will help you to understand all the moving
parts of your mobile app.  To get a basic grasp of how Titanium works, check out [this architecture guide](architecture.html)
for more on Titanium's pre-compile, build, and runtime behavior.

Once you have an idea of "what's in the box" for a Titanium application, you will likely want to learn about how to program your
application's [user interface](ui_design.html).  Most of the JavaScript code you write will have to do with the construction and
behavior of UI elements, so it makes sense to wrap your brain around these concepts right away.

## Learning about the Titanium JavaScript environment

Having a strong grasp of the JavaScript programming language and the Titanium JavaScript environment is essential to becoming a
productive Titanium developer.  If you're brand new to JavaScript, be sure to check out our
[Introduction to JavaScript](intro_javascript.html) for a primer on one of the coolest and most pervasive programming languages
in the wild today.  JavaScript is showing up everywhere - on the server, on the desktop, and even on mobile devices! ;)

Even if you're a seasoned JavaScript pro, you'll probably want to read up on [the Titanium JavaScript Environment](javascript_environment.html),
where you will learn about the built-in functions and 'execution contexts' that define your application's runtime environment.
Essential reading for any Titanium developer.

For those looking to understand how best to structure their JavaScript code (specifically for a Titanium Mobile application),
you might want to [read up on the JavaScript best practices](javascript_best_practices.html) we've collected.  Hopefully they will
help you to create a more maintainable and elegant JavaScript application.

## Getting productive with Titanium

The rest of the guides found here are more oriented toward specific tasks or techniques for building Titanium applications,
which you can explore at your leisure.  We hope you find these guides helpful - or better yet, [[contribute]] to make them even
better!