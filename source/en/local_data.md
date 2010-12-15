<summary>
This guide will prepare you to work with local data sources in a Titanium mobile application.  By the end of
this guide, you should understand:

* The different modes of local storage and when to use each
* How to use application properties
* How to interact with a local SQLite database
* How to store and retrieve data from the local filesystem

</summary>

# Working With Local data

Even the most rudimentary applications usually have some data storage requirements. As always, Titanium provides access to all the native functionality via its convenient, uniform interface. To use a device's local storage, the following objects are needed:

* `Titanium.App.Properties` is ideal for storing application-related settings
* `Titanium.Filesystem` facilitates file and directory manipulation
* `Titanium.Database` gives access to local SQLite3 databases


Each of these enable data to persist on a device across application restarts, power cycles, reinstallation and even migration to a new device.

## What kind of data storage should I use?

The decision about which of the three local storage options you choose is usually determined by the following:

1. **Properties**. Use these when one or all of the following is true:
    * the data consists of simple key/value pairs
    * the data is related to the application rather than the user
    * the data does not require other data in order to be meaningful or useful
    * there only needs to be one version of the data stored at any one time
2. **Filesystem**. Use this when one or all of the following is true:
    * the data is already provided in file format
    * the data is an image file
3. **Database**. Use this when one or all of the following is true:
    * there are many similar data items
    * data items relate to each other
    * you require tight flexibility over how the data will be presented when you retrieve it
    * the data accumulates over time, such as transactional, logging or archiving data


<note>
Although the local database has the capability to store images in blob (binary) format, this won't lead to optimal performance from your application. Instead, use `Titanium.Database` to store the image file path and name in the database, and `Titanium.Filesystem` to manage the physical files.

To keep the local filesystem organised, and thus easier to manage and backup, it's good practice to keep similar files, like these images, in the same location. Properties may be a good candidate for storing this filesystem path, common to all your application's images, as it would be considered an application setting.
</note>

# Application Properties

<info>
Both iOS and Android store properties in special files on the filesystem. Natively, iOS properties are known as `NSUserDefaults`, which are stored in `.plist` files in the application's library directory. Android stores them in standard xml text files at `/data/data/com.domainname.appname/shared_prefs/titanium.xml`.

An application's property data is loaded into memory as the application launches, and exists there until it closes. This allows very rapid access to it, but at the cost of the increased baseline memory used by the application.
</info>

## Reading and Writing Properties

`Titanium.App.Properties` has six sets of get/set methods for handling six different data types:

* **getBool() / setBool()**: for booleans (true, false)
* **getDouble() / setDouble()**: for double-precision floating point numbers
* **getInt() / setInt()**: for integers
* **getList() / setList()**: for arrays
* **getString() / setString()**: for strings

These *get* methods accept a property name and default value, while the *set* methods require the property name and property value pairs. This is demonstrated below:

<code class="javascript">
var window = Titanium.UI.createWindow({
    backgroundColor:'#999'
});
var myArray = [
	{name:'Name 1', address:'1 Main St'},
	{name:'Name 2', address:'2 Main St'},
	{name:'Name 3', address:'3 Main St'},
	{name:'Name 4', address:'4 Main St'}
];
Ti.App.Properties.setString('myString','This is a string');
Ti.App.Properties.setInt('myInt',10);
Ti.App.Properties.setBool('myBool',true);
Ti.App.Properties.setDouble('myDouble',10.6);
Ti.App.Properties.setList('myList',myArray);
// **********************************************
// Notice the use of the second argument of the get* methods below
// that would be returned if no property exists with that name
// **********************************************
Ti.API.info("String: "+Ti.App.Properties.getString('myString','This is a string default'));
Ti.API.info("Integer: "+Ti.App.Properties.getInt('myInt',20));
Ti.API.info("Boolean: "+Ti.App.Properties.getBool('myBool',false));
Ti.API.info("Double: "+Ti.App.Properties.getDouble('myDouble',20.6));
Ti.API.info("List: "+Ti.App.Properties.getList('myList'));
window.open();
</code>

This code outputs the following results:

<pre>
String: This is a string
Integer: 10
Boolean: true
Double: 10.600000381469727
List:
  {  'address' :  '1 Main St' 'name' :  'Name 1', },
  {  'address' :  '2 Main St' 'name' :  'Name 2', },
  {  'address' :  '3 Main St' 'name' :  'Name 3', },
  {  'address' :  '4 Main St' 'name' :  'Name 4', }
</pre>

## Storing JS objects as JSON in properties

If you have a complex Javascript object, you can convert it to a JSON string using `JSON.stringify()`, which will allow you to store it in the database using the [Titanium.App.Properties.setString()](http://developer.appcelerator.com/apidoc/mobile/latest/Titanium.App.Properties.setString-method.html) method.

<code class="javascript">
var window = Titanium.UI.createWindow({
    backgroundColor:'#999'
});
var weatherData =
{
	"reports" : [
    {
        "city": "Mountain View",
        "condition": "Cloudy",
        "icon": "http://www.google.com/weather/cloudy.gif"
    },
    {
        "city": "Washington, DC",
        "condition": "Mostly Cloudy",
        "icon": "http://www.google.com/weather/mostly_cloudy.gif"
    },
    {
        "city": "Brasilia",
        "condition": "Thunderstorm",
        "icon": "http://www.google.com/weather/thunderstorm.gif"
    }
  ]
};
Ti.App.Properties.setString('myJSON', JSON.stringify(weatherData));
var retrievedJSON=Ti.App.Properties.getString('myJSON', 'myJSON not found');
Ti.API.info("The myJSON property contains: " + retrievedJSON);
window.open();
</code>

This will output the following to the log:

<pre>
The myJSON property contains: {"reports":[{"icon":"http:\/\/www.google.com\/ig\/images\/weather\/cloudy.gif","condition":"Cloudy","city":"Mountain View"},{"icon":"http:\/\/www.google.com\/ig\/images\/weather\/mostly_cloudy.gif","condition":"Mostly Cloudy","city":"Washington, DC"},{"icon":"http:\/\/www.google.com\/ig\/images\/weather\/thunderstorm.gif","condition":"Thunderstorm","city":"Brasilia"}]}
</pre>

This stored JSON string can later be converted back to a Javascript object using `JSON.parse()`:

<code class="javascript">
var myObject = JSON.parse(Ti.App.Properties.getString('myJSON'));
</code>

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

* **getFile**: returns a fully formed file path as a Titanium.Filesystem.File object
* **deleteFile**: delete the file
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

# SQLite Databases
[this section is to be completed soon]
## SQL basics and further reading on SQLite
[this section is to be completed soon]
## Storing data
[this section is to be completed soon]
## Reading data
[this section is to be completed soon]
## Updating data
[this section is to be completed soon]
## Pre-populating a database
[this section is to be completed soon]