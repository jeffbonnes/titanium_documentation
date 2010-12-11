<summary>
This guide will prepare you to work with local data sources in a Titanium mobile application.  By the end of
this guide, you should understand:

* The different modes of local storage and when to use each
* How to interact with a local SQLite database
* How to use application properties
* How to store and retrieve data from the local filesystem

</summary>

# Working With Local data

## What kind of data storage should I use?

# SQLite Databases

## SQL basics and further reading on SQLite

## Storing data

## Reading data

## Updating data

## Pre-populating a database

# Application Properties

## Reading and Writing Properties

## Storing complex objects as JSON in properties

# Filesystem Storage

Titanium makes it simple to perform basic filesystem [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) operations.  Before we dive into an example let's review a few of the most commonly used properties and methods:

#### Objects

* [Titanium.Filesystem](http://developer.appcelerator.com/apidoc/mobile/latest/Titanium.Filesystem-module) is the top level Filesystem module used for reading and saving files and directories on the device.
* [Titanium.Filesystem.File](http://developer.appcelerator.com/apidoc/mobile/latest/Titanium.Filesystem.File-object.html) is the file object which supported common filesystem based operations such as create, read, write, delete, etc.

#### Properties

* Data storage locations:
    * **applicationDataDirectory**: A readonly constant that indicates where your application data directory is located.  Place applications-specific files in this directory.
    * **resourcesDirectory**: A readonly constant where your application resources are located
    * **tempDirectory**: A readonly constant that indicates where your application can place temporary files

#### Methods

* **getFile**: Returns a fully formed file path as a Titanium.Filesystem.File object
* **deleteFile**: Delete the file
* **exists**: returns true if the file or directory exists on the device
* **extension**: return the file extension
* **move**: move the file to another path
* **rename**: rename the file
* **nativePath**: returns the fully resolved native path
* **read**: return the contents of file as blob
* **write**: write the contents to file
* **writeable**: returns true if the file is writeable
* **spaceAvailable**: return boolean to indicate if the path has space available for storage

## Writing files

This is best explained with a basic example.  We'll start off by creating a dummy JavaScript object that we'd like to store:
<code class="javascript">
var dataToWrite = {"en_us":{"foo":"bar"}};
</code>

Next we create a new directory:
<code class="javascript">
//NOTE: remember to use applicationDataDirectory for writes
var newDir = Titanium.Filesystem.getFile(Titanium.Filesystem.applicationDataDirectory,'mydir');
newDir.createDirectory();
Ti.API.info('Path to newdir: ' + newDir.nativePath);
</code>

Lets add a new file within the newly created directory:
<code class="javascript">
var newFile = Titanium.Filesystem.getFile(newDir.nativePath,'newfile.json');
</code>

An empty file is nice and all, but it would be even better with some data.  Lets fill it with the JavaScript object we created earlier:
<code class="javascript">
//Stringify the JavaScript object we created earlier and write it out to the new file
newFile.write(JSON.stringify(dataToWrite));
</code>

<note>
Our **dataToWrite** object must be serialized before we can write it to a file. [Titanium.JSON](http://developer.appcelerator.com/apidoc/desktop/latest/Titanium.JSON) provides a module for serializing and deserializing [JSON](http://www.json.org/).
</note>

## Reading files

What if we wanted to update the value of dataToWrite.en_us.foo?  For this we:

1. Read the file
2. Deserialize the object stored within the file
3. Update the property in question
4. Serialize the object
5. Write back the serialized object

<code class="javascript">
var newFile = Titanium.Filesystem.getFile(newDir.nativePath,'newfile.json');
var resources = JSON.parse(newFile.read().text);

resources.en_us.foo = 'baz'; //bar becomes baz
newFile.write(JSON.stringify(resources));
</code>

## Removing files

Nobody likes a mess.  Lets cleanup after ourselves.  First we'll remove the file and then directory:

<code class="javascript">
//We already have references to the file and directory objects.
//We just need to call their cooresponding delete methods.
newFile.deleteFile();
newDir.deleteDirectory();
</code>

The full gist for this example is available [here](https://gist.github.com/737045).