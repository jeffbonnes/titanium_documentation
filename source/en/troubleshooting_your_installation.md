<summary>
This guide will help you through problems that exist after you have installed Titanium and followed the [Getting Started Guide](getting_started.html). It will explain:

* The tools that can help isolate the cause of issues
* Steps to resolve permissions-related problems
* Some common problems and errors that users experience
</summary>

# Titanium and 3rd-party tools

If you have followed everything in the [Getting Started Guide](getting_started.html) and something is still not working, it is often possible to determine the cause using feedback provided by Titanium Developer and the third-party SDKs it uses.

## The Project Creation Process

<info>When you first run Titanium Developer, the most reliable way to ensure that it works correctly is to create a new project and launch it without any modification. If it doesn't work, proceed with the following troubleshooting process until it launches successfully, before attempting to troubleshoot your own projects.</info>

When you create a project using Titanium Developer, the command it uses will output some messages to the command line (also known as the "shell"), such as:

<pre>
"/opt/titanium/mobilesdk/linux/1.5.X-20101111/project.py",
 "testing", "com.testing.testing", "/home/appcel",
 "android", "/opt/android-sdk/add-ons"
</pre>

Here, `project.py` is run with various parameters.  If you run `project.py` yourself, without any arguments, it will give you more information about what they mean:

<pre>
Usage: project.py &lt;name&gt; &lt;id&gt; &lt;directory&gt; [iphone,android] [android_sdk]
</pre>

In this example, Titanium is unable to create a project successfully. By observing the output, we can see that the last parameter is incorrect, as it does not point to the root of Android's SDK directory, which in this case is located at `/opt/android-sdk`. Once the error has been corrected using Titanium Developer's "Edit Profile" screen, it is able to create the project without any issue, like this:

<pre>
"/opt/titanium/mobilesdk/linux/1.5.X-20101111/project.py",
 "testing", "com.testing.testing", "/home/appcel",
 "android", "/opt/android-sdk"
</pre>

![Titanium Developer Edit Profile SDK Path Configuration](../assets/images/guides/start_here/troubleshooting-edit-profile-sdk-path.png)

## Booting Third-party Emulators

If an emulator does does not boot at all when you launch your application, you can see whether you can boot it manually using the appropriate third-party's SDK tool. For Android, you would run the following command:

<code>
/path/to/android-sdk/tools/android
</code>

This will launch the "Android SDK and AVD Manager" or, more simply, the `android` tool:

![an Android Tools Screenshot](../assets/images/guides/start_here/troubleshooting-android-tools.png)

In this example, you can see the titanium has successfully created a virtual machine with a name beginning with `titanium_`.  If none at all are listed, even though you have attempted to launch your app at least once already, then either:

* Titanium has not been able to find or access the `android` tool, so you would need to check the path configured for the SDK in Titanium Developer and also the permissions that are set on it
* the `android` tool has not been able to save the settings, which would indicate a problem with the user-specific `.android` directory

If you click the `Start...` button to boot the emulator, you may receive an error.  For example:

![Android Tools Error Screenshot](../assets/images/guides/start_here/troubleshooting-android-tools-error.png)

This particular error means that either the user-specific or temporary files related to android cannot be modified.

## Titanium Developer Debug Tools

For issues that occur between the moment Titanium Developer loads until an emulator starts to boot, the `--debug` switch is helpful.  To use it, pass it to Titanium at startup:

<code>
Titanium Developer --debug
</code>

However, once the emulator boot sequence has started, the logging information shown on the `Test & Package` / `Run Emulator` tab is more likely to assist you. Of the options in the `Filter` drop-down list, the `Trace` filter is by far the most useful, as it shows all the commands run by Titanium and their respective output.

![Titanium Developer Trace Logging Screenshot](../assets/images/guides/start_here/troubleshooting-run-emu-trace.png)

<note>Whether you are debugging from the command line or Titanium itself, a handy tip is to copy the logging output into a featureful text editor, and use its search features. Pay particular attention to `error` and `denied` messages, to help you to find a solution.</note>

By inspecting the logs, you gain a much better understand of how Titanium really works. A lot of the commands you could actually run yourself manually at the system's command line. As such, you need to be certain that those commands exist on your system and you have the sufficient access to use them, which usually means ensuring that the files and directories involved have the correct read, write and/or execute filesystem permissions. This behavior is characteristic of all modern operating systems and, consequently, the troubleshooting and resolution methods are the same.

# File Permissions Solution

Local filesystem permissions can often be the cause of Titanium's problems.  However, before you consider modifying them, use the information that Titanium Developer outputs to determine whether the files and directories it relies on actually exist. Once this is done, use the following information to correct their permissions.

The files required by Titanium can be split into a number of categories; Common System files,  User-specific, Project and Temporary.

## Common System Files

Common system files contain core programs and scripts, and their associated files, to which all users must have access in order to run Titanium. Their locations and descriptions are explained in the table below.

<table>
<tr>
	<th>Location</th> <th>Contains Files and Directories Including</th> <th>Known As</th>
</tr>
<tr>
	<td>`/path/to/Titanium\ Developer-&lt;version&gt;/`</td>
	<td>`Titanium Developer`</td>
	<td>Appcelerator Titanium Developer directory</td>
</tr>
<tr>
	<td>`/path/to/titanium/`</td>
	<td>`mobilesdk`, `modules`, `runtime`, `sdk`</td>
	<td>Appcelerator Titanium SDK directory</td>
</tr>
<tr>
	<td>`/path/to/android-sdk/`</td>
	<td>`add-ons`, `docs`, `platforms`, `tools`</td>
	<td>Google Android Development Kit (ADK) directory</td>
</tr>
<tr>
	<td>`/path/to/android-sdk/tools/`</td>
	<td>`adb`, `android`, `apkbuilder`, `ddms`</td>
	<td>Google ADK Tools directory</td>
</tr>
<tr>
	<td>`/path/to/android-sdk/platforms/`</td>
	<td>`android-&lt;version&gt;`</td>
	<td>Google ADK Platforms directory</td>
</tr>
</table>

These resources should have ownership granted to the system administrator, with the following permissions for the administrator and all users (ie the "Users" or "Everyone" group for Windows and the implicit group "other" for *nix):

<table>
<tr>
	<th>Resource Type</th> <th>Permissions</th>
</tr>
<tr>
	<td>directories</td>
	<td>read and execute (traverse)</td>
</tr>
<tr>
	<td>files</td>
	<td>read</td>
</tr>
<tr>
	<td>python scripts (`.py` extension)</td>
	<td>read and execute</td>
</tr>
<tr>
	<td>binary files</td>
	<td>read and execute</td>
</tr>
</table>

As differentiating binary files from standard files can be difficult, simply treat all files as standard files when setting permissions and then set execute permissions explicitly on the following resources:

<table>
<tr>
	<th>Binary File Locations</th>
</tr>
<tr>
	<td>`/path/to/Titanium Developer-&lt;version&gt;/Titanium Developer`</td>
</tr>
<tr>
	<td>`/path/to/Titanium Developer-&lt;version&gt;/installer/installer`</td>
</tr>
<tr>
	<td>`/path/to/android-sdk/tools/*`</td>
</tr>
<tr>
	<td>`/path/to/android-sdk/platforms/android-3/tools/*`</td>
</tr>
<tr>
	<td>`/path/to/android-sdk/platforms/android-4/tools/*`</td>
</tr>
<tr>
	<td>`/path/to/android-sdk/platforms/android-7/tools/*`</td>
</tr>
<tr>
	<td>`/path/to/android-sdk/platforms/android-8/tools/*`</td>
</tr>
</table>

System administrators should have the following **additional** permissions, to allow them to update the software:

<table>
<tr>
	<th>Resource Type</th> <th>Permissions</th>
</tr>
<tr>
	<td>directories</td>
	<td>write</td>
</tr>
<tr>
	<td>all files</td>
	<td>write</td>
</tr>
</table>

## User-specific Files

User-specific files and directories are unique to the user, and contain Titanium settings, third-party SDK settings, emulator images and other session-related data.

<table>
<tr>
	<th>Location</th> <th>Contains Files and Directories Including</th> <th>Known As</th>
</tr>
<tr>
	<td>`/path/to/$USER_HOME/.titanium/`</td>
	<td>`android2.sdcard`, `appdata`, `.titanium`</td>
	<td>Appcelerator Titanium User Settings directory</td>
</tr>
<tr>
	<td>`/path/to/$USER_HOME/.android/`</td>
	<td>`androidtool.cfg`, `avd`, `repositories.cfg`</td>
	<td>Google ADK User Settings directory</td>
</tr>
</table>

<note>$USER_HOME in this table is synonymous with the user's home or profile directory</note>

These resources should have ownership granted to the respective user and the following filesystem permissions configured:

<table>
<tr>
	<th>Resource Type</th> <th>Permissions</th>
</tr>
<tr>
	<td>directories</td>
	<td>read, write and execute (traverse)</td>
</tr>
<tr>
	<td>all files</td>
	<td>read and write</td>
</tr>
</table>

## Project Files

The files you are probably most aware of when working with Titanium are those in the project directory:

<table>
<tr>
	<th>Location</th> <th>Contains Files and Directories Including</th> <th>Known As</th>
</tr>
<tr>
	<td>`/path/to/&lt;project-name&gt;/`</td>
	<td>`build`, `Resources`</td>
	<td>Titanium project directory</td>
</tr>
</table>

These resources should have ownership and the following permissions granted to the Titanium user:

<table>
<tr>
	<th>Resource Type</th> <th>Permissions</th>
</tr>
<tr>
	<td>directories</td>
	<td>read, write and execute (traverse)</td>
</tr>
<tr>
	<td>all files</td>
	<td>read and write</td>
</tr>
</table>

Ordinarily, multiple developers would work on the same project using a versioning system such as git, because concurrent access to the project directory is not supported. However, sometimes you may have multiple local user accounts using the same project a different times. In these cases, it would be useful for you to set up a system group, make each user a member of it and then grant permissions to this group instead of a specific user.

When you launch the project's application, Titanium will compile the application, and generate files in subdirectories of the project build directories:
<table>
<tr>
	<th>Location</th> <th>Contains Files and Directories Including</th> <th>Known As</th>
</tr>
<tr>
	<td>`/path/to/&lt;project-name&gt;/build/android/*`</td>
	<td>`assets`, `bin`, `gen`, `lib`, `res`, `src`</td>
	<td>Titanium Project Android Build directory</td>
</tr>
<tr>
	<td>`/path/to/&lt;project-name&gt;/build/iphone/*`</td>
	<td>`assets`, `bin`, `gen`, `lib`, `res`, `src`</td>
	<td>Titanium Project iPhone Build directory</td>
</tr>
</table>

The build directory can sometimes be the source of certain problems, causing an application not to build, install or launch. See the  "When All Else Fails" section below that contains a solution to them.

## Temporary Files

The exception to the above categories are the temporary files created automatically in a common location by third-party SDKs.

<table>
<tr>
	<th>Location</th> <th>Contains Files and Directories Including</th> <th>Known As</th>
</tr>
<tr>
	<td>`/path/to/tmp/android/`</td>
	<td>`emulator-&lt;hash&gt;`</td>
	<td>Google Android Temporary Files directory</td>
</tr>
<tr>
	<td>`/path/to/tmp/adb.log`</td>
	<td></td>
	<td>Google Android adb log file</td>
</tr>
</table>

As they were created automatically, the permissions set may be appropriate for the user using Titanium at the time, but be too restrictive for others to also use them. You can try to reconfigure them as follows, but they are likely to be reset again by the software:

<table>
<tr>
	<th>Resource Type</th> <th>Permissions</th>
</tr>
<tr>
	<td>directories</td>
	<td>read, write and execute (traverse)</td>
</tr>
<tr>
	<td>all files</td>
	<td>read and write</td>
</tr>
</table>

The solution is to simply close Titanium Developer and all emulators, and delete these directories. They will be recreated when an emulator runs again.

## When All Else Fails (let Titanium fix it!)

If you have set the filesystem ownership and permissions correctly using the guidance above, but it still does not work, there are certain files that you can clear and let Titanium recreate, which will often resolve the residual issues. This provides two benefits; it not only confirms that filesystem access for files has been set to the defaults, it also ensures file integrity.

* close all emulators
* close Titanium
* delete or rename the following filesystem locations :

<table>
<tr>
	<th>Files/Directories to Clear</th> <th>Known As</th><th>Action</th>
</tr>
<tr>
	<td>`/path/to/tmp/android/`</td>
	<td>Google Android Temporary Files directory</td>
	<td>Delete</td>
</tr>
<tr>
	<td>`/path/to/tmp/adb.log`</td>
	<td>Google Android adb log file</td>
	<td>Delete</td>
</tr>
<tr>
	<td>`/path/to/$USER_HOME/.android/`</td>
	<td>Google ADK User Settings directory</td>
	<td>Delete</td>
</tr>
</table>

The project build directory must be handled differently, because if certain of its sub-directories do not exist, the project will fail to compile.
<table>
<tr>
	<th>Location</th> <th>Contains Files and Directories Including</th> <th>Known As</th>
</tr>
<tr>
	<td>`/path/to/&lt;project-name&gt;/build/android/*`</td>
	<td>`assets`, `bin`, `gen`, `lib`, `res`, `src`</td>
	<td>Contents of Titanium Project Android Build directory</td>
</tr>
<tr>
	<td>`/path/to/&lt;project-name&gt;/build/iphone/*`</td>
	<td>`assets`, `bin`, `gen`, `lib`, `res`, `src`</td>
	<td>Contents of Titanium Project iPhone Build directory</td>
</tr>
</table>

The safest way to clear these directories is as follows:

* make a copy of the build directory, and place this copy into the project directory (alongside its original). This copy will automatically have the appropriate permissions for you to access any files it contains
* rename the build directory (ie to `build-orig`)
* rename the copy as `build`
* inside the `build/android` and `build/iphone` directories, there are sub-directories named `assets`, `bin`, `gen`, `lib`, `res` and `src`. Delete all the **contents** from each one of these

Lastly,

* open Titanium
* select project, launch application

# Common Problems

This section describes a few of the most common issues reported by users, and the recommended action you can try to help resolve them.

<info>It is assumed that you have already
</info>
## Project Creation Error: Error Creating Project

<warning>
**Symptoms**

* Cannot create project
* `Run Emulator` drop-down menus constantly show a "loading..." message
</warning>

On the `New Project` page, you can see whether an iPhone or Android SDK has been detected, by observing the indicators shown near the bottom of the screen, above the buttons. When you click the `Create Project` button, you may receive a message that states, `Project Creation Error: Error Creating Project. Please try again`.

![Project Creation Error Screenshot](../assets/images/guides/start_here/troubleshooting-project-creation-error.png)

Also, if you view the `Run Emulator` tab for an existing project, you will see that the `SDK` and `Display` drop-down list options constantly state they are "loading...".

![Run Emulator Loading Error Screenshot](../assets/images/guides/start_here/troubleshooting-run-emulator-sdk-always-loading.png)

No errors are output from Titanium's `--debug`, but it does state the command it attempted to run:

<pre>
[20:45:33:572] [Titanium.API] [Information] (JavaScript.KKJSList)
[ "/opt/titanium/mobilesdk/linux/1.4.X-20101102/android/avd.py", ""/opt/android-sdk"", ]
** Message: console message: file:///opt/titanium/modules/linux/tiui/1.0.0/ui.js @139:
/opt/titanium/mobilesdk/linux/1.4.X-20101102/android/avd.py,"/opt/android-sdk"
</pre>

Using this information, we need to ensure that the directories and files exist and have the appropriate permissions configured, namely:

* Appcelerator Titanium SDK directory
* Google Android Development Kit (ADK) directory
* any `.py` (python) files, in particular `avd.py`

Once you have confirmed they exist, the [File Permissions Solution](#file_permissions_solution) section will guide you in resolving the rest of the issue.

## Blank, Gray "Run Emulator" Logging Area and Missing All Other Options

<warning>
**Symptoms**

* Blank `Run Emulator` tab
</warning>

In Titanium Developer, when you click on the `Test & Package` / `Run Emulator` tab, the logging area may be blank and gray, and all other controls missing:

![Screenshot showing Titanium Run Emulator tab with all controls missing](../assets/images/guides/start_here/troubleshooting-run-emu-missing-all-options.png)

This is sometimes due to incorrect file permissions set on the related Temporary Files. See the [File Permissions Solution](#file_permissions_solution) section to resolve this problem.

## Missing "Run Emulator" SDK and Display Options

<warning>
**Symptoms**

* Missing `SDK` and `Display` options on `Run Emulator` tab
</warning>

In Titanium Developer, when you click on the `Test & Package` tab followed by the `Run Emulator` tab beneath it, with the intention of launching your application, there should be three drop-down list options (`SDK`, `Display` and `Filter`) and two buttons (`Launch` and `Stop`), as follows:

![Screenshot showing Titanium Run Emulator tab with all options](../assets/images/guides/start_here/troubleshooting-missing-options-normal.png)

If a problem exists, the `SDK` and `Display` options may not be shown:

![Screenshot showing options missing on Run Emulator tab](../assets/images/guides/start_here/troubleshooting-missing-options-problem.png)

This is likely caused by Titanium not being able to access the Appcelerator Titanium SDK directory, or the mobilesdk/&lt;platform&gt;/&lt;titanium_sdk_version&gt; directory it contains. Use the [File Permissions Solution](#file_permissions_solution) section to resolve the issue.

Then, in Titanium, switch to the `Edit` tab, click the `Save Changes` button (to force the application to be rebuilt), return to the `Run Emulator` tab, and the previously-missing options should be restored.

## Timed out waiting for emulator to be ready

<warning>
**Symptoms**

* Emulator does not start
</warning>

You launch your application, but an emulator window does not appear. Eventually, Titanium gives a `trace` logging output as follows:
<pre>
[DEBUG] /opt/android-sdk/tools/emulator
-avd titanium_7_WVGA800
-port 5560
-sdcard /home/appcel/.titanium/android2.sdcard
-logcat '*:d *'
-no-boot-anim
-partition-size 128
[TRACE] NAND: could not create temp file for system NAND disk image: Permission denied[DEBUG] signal caught: 3
[DEBUG] calling emulator kill on 21325
[TRACE] Traceback (most recent call last):
[TRACE] File "/opt/titanium/mobilesdk/linux/1.4.X-20101112/android/builder.py", line 1011, in <module>
[TRACE] s.run_emulator(avd_id,avd_skin)
[TRACE] File "/opt/titanium/mobilesdk/linux/1.4.X-20101112/android/builder.py", line 296, in run_emulator
[TRACE] handler(3,None)
[TRACE] File "/opt/titanium/mobilesdk/linux/1.4.X-20101112/android/builder.py", line 281, in handler
[TRACE] os.kill(p.pid, signal.SIGTERM)
[TRACE] OSError: [Errno 3] No such process
[INFO] Building testing2 for Android ... one moment
[INFO] Titanium SDK version: 1.4.3 (11/12/10 08:26 c377118)
[DEBUG] Waiting for device to be ready ...
[TRACE] adb devices returned 0 devices/emulators
[ERROR] Timed out waiting for emulator to be ready, you may need to close the emulator and try again
</pre>

Pay particular attention to the lines:
<pre>
[TRACE] NAND: could not create temp file for system NAND disk image:
Permission denied[DEBUG] signal caught: 3
</pre>

This implies that the emulator cannot access the `.apk` application package files from the Project Build directories, which can often happen when multiple system users are working on the same project, even at different times. This can lead to a file permissions problem in the project's build directories. Use the [File Permissions Solution](#file_permissions_solution) section to resolve the issue, and then launch the application again.


## zipalign Unable to open app.apk as zip archive

<warning>
**Symptoms**

* Emulator boots, but application does not install or load
</warning>

<pre>
[TRACE] Cannot write /home/appcel/workspace/testing2/build/android/bin/app-unsigned.apk
[DEBUG] /opt/android-sdk/tools/zipalign -v 4 /home/appcel/workspace/testing2/build/android/bin/app.apk /home/appcel/workspace/testing2/build/android/bin/app.apkz
[ERROR] System Error while compiling Android classes.dex
[ERROR] /opt/android-sdk/tools/zipalign Unable to open '/home/appcel/workspace/testing2/build/android/bin/app.apk' as zip archive
[TRACE] D/SntpClient( 60): request time failed: java.net.SocketException: Address family not supported by protocol
</pre>

This message indicates that when Titanium attempts to use zipalign to open `app.apk`, it is prevented from doing so, meaning one of them, or their associated files, is missing or has the incorrect permissions. Use the [File Permissions Solution](#file_permissions_solution) section to resolve the issue, and then launch the application again.

## apkbuilder.bat - THIS TOOL IS DEPRECATED

<warning>
**Symptoms**

* None - only aesthetic
</warning>

<pre>
[ERROR] /opt/android-sdk/tools/apkbuilder
[ERROR]
[ERROR] THIS TOOL IS DEPRECATED. See --help for more information.
[ERROR]
</pre>

This error refers to the apkbuilder tool developed by Google that is located in the Google ADK Tools directory. The source of the message is Google's `adb`, located in the same directory. Titanium uses this tool to produce it's emulator logging output. Hence, this is not really a Titanium message.

Google has deprecated apkbuilder in favor of the ApkBuilderTask java class. However, apkbuilder still works without any other issues. At some point, a future release of Titanium Developer will be created to utilize the java class but, until then, this message can be safely ignored.
