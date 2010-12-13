<summary>
In this guide, you will learn about testing your application on platform specific emulators and devices.
By the end of this guide, you should understand:

* How to launch your application with the iPhone & Android emulators
* How to install and launch your application on iPhone & Android devices
* How to output log messages to the Titanium Developer console
* How to adjust log levels within Titanium Developer
</summary>

#Overview

The main focus of this guide will center around the **Test & Package** tab of Titanium Developer.  You will use the **Run Emulator** & **Run on Device** sections of this tab to test your application.

#Platform SDKs

Before you can test or deploy with the various mobile platforms you will need to download & install their specific software development kits (SDKs).  Below are links to the supported platform SDKs:

* iPhone (iOS) - [http://developer.apple.com/devcenter/ios/index.action](http://developer.apple.com/devcenter/ios/index.action)
* Android - [http://developer.android.com/sdk/index.html](http://developer.android.com/sdk/index.html)

<note>
The iOS setup is straightforward.  No further configuration should be required beyond installing the Xcode and iOS SDK.

NOTE: iPhone (iOS) development is only supported on Mac OS X.
</note>

<note>
Android SDK setup is a bit more complicated.  After you've downloaded the SDK you can follow the installation guide found [here](http://developer.android.com/sdk/installing.html).  Pay particular attention to the [Adding Platforms and Other Components](http://developer.android.com/sdk/installing.html#AddingComponents) section.  You'll need to install at least 1 version of the **Platform SDK** (typically the latest one) as well as the **Google APIs by Google Inc** 3rd Party add-on.
</note>

Once you have completed the Android configuration noted earlier you'll also need to tell Titanium Developer where the SDK is located.  You do this by selected the **Profile** perspective and updating the **Android SDK** value under **Environment Settings**.

![Android SDK Location](http://developer.appcelerator.com.s3.amazonaws.com/documentation-examples/environment_settings.png "Android SDK Location")


You can verify the SDKs are properly configured by looking for the green checkmark indicators on the **New Project** screen:

![New Project](http://developer.appcelerator.com.s3.amazonaws.com/documentation-examples/new_project.png "New Project")

#Getting Started

With the mobile platform SDKs in place you are ready to move on with testing.  Get started by performing the following steps:
    
    1.  Launch Titanium Developer
    2.  Create a new project or select a pre-existing project
    3.  Select the Test & Package tab

<note>
The auto-generated project created via **New Project** is actually a working skeleton application.  You can use this to experiment with the various testing options even if you don't have a project of your own to play with yet.
</note>

#Run Emulator

On the **Test & Package** tab there are currently two options to choose from; iPhone & Android.  Select the platform you would like to test on.

![Run Emulator](http://developer.appcelerator.com.s3.amazonaws.com/documentation-examples/run_emulator.png "Run Emulator")

##Emulator Options

After selecting the platform to test you just need set the options for the test and click **Launch**.  For the most part the options presented for each emulator are the same.

* **SDK**: Used to select the version of the platform SDK to test with
* **Filter**: Used to select the level of log information to display in the Titanium Developer console as the application is running.  This can be adjusted while the emulator is running as well.
* **Screen**: This is an Android only option.  Just as it sounds, this options allows you select the type of screen to test on.
* **Launch**: Starts the emulator
* **Stop**: Stops the emulator

**iPhone Emulator Options**:    
![Run Emulator iPhone](http://developer.appcelerator.com.s3.amazonaws.com/documentation-examples/run_emulator_options_iphone.png "Run Emulator iPhone")

**Android Emulator Options**:
![Run Emulator Android](http://developer.appcelerator.com.s3.amazonaws.com/documentation-examples/run_emulator_options_android.png "Run Emulator Android")

Below is an example of what you should expect to see after successfully launching each of the emulators.

![Running Emulators](http://developer.appcelerator.com.s3.amazonaws.com/documentation-examples/running_emulators.png "Running Emulators")

#Run on Device

To test your application on a physical device perform the following steps:

    1.  Connect the device you'll be installing the application on via USB
    2.  Select the corresponding mobile platform on the Test & Package tab
    3.  Click the Install Now button (see additional notes below)
    4.  Launch/Test your application on the device
    
<note>
Android doesn't require any additional setup beyond what was discussed earlier.

To install an application on an iOS device you must first complete a series of steps required by Apple (e.g.) register iPhone, obtain developer certificate, etc.  These steps are outlined on the **iPhone** sub tab of the **Run on Device** tab.  A screenshot of these steps is displayed below.
</note>

![iPhone Device Setup](http://developer.appcelerator.com.s3.amazonaws.com/documentation-examples/run_on_iphone.png "iPhone Device Setup")

#Logging

Titanium offers a number of logging options that will prove useful as you develop your applications.  Use these to dump out information during interesting moments of time within your application as needed.

<code type="javascript">
Ti.API.info(message); //information messages
Ti.API.error(message); //error messages
Ti.API.debug(message); //debug messages
Ti.API.warn(message); //warning messages
</code>

Review the console output to aid with debugging as you develop:
![Console Messages](http://developer.appcelerator.com.s3.amazonaws.com/documentation-examples/emulator_console_log.png "Console Messages")


Review the [Titanium.API docs](http://developer.appcelerator.com/apidoc/mobile/latest/Titanium.API-module) for more information.
