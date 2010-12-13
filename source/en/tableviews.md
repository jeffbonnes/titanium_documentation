<summary>
This guide will show you how to work with tabular data in TableViews, a core UI component of many mobile
applications.  By the end of this guide, you should understand:

* The different between *Simple* and *Standard* TableViews
* How to create and style table views
* How to update your table view with additional data
* How to optimize your table view for large data sets
* The typical considerations involved in developing a Titanium application

</summary>

# The Titanium TableView

There are members of our community who had already developed a number of mobile applications in native code before discovering Titanium. When they were offered projects to target multiple platforms, some of them didn't have the time or inclination to learn a completely new language and environment. Others were concerned about the ever-increasing burden, and uninspiring job, of maintaining the same applications across numerous codebases. While these are indeed the initial motivations that make a lot of people reassess their development strategy, TableViews are a good example of how Titanium provides another equally-compelling advantage.

It would be a fairly complex task to build a table in native code that incorporates sufficient functionality to make it useful. Conversely, utilizing just its basic features, much of the most commonly-used functionality comes built-in with the standard Titanium API. For instance, with Titanium's 'Simple' TableViews you can:

* customize the look and feel of your tables and their rows
* add styled headers and footers to your tables
* dynamically add and remove rows. If there are too many rows to fit on the screen at once, the table will accommodate them by changing from a static to a movable, scrolling, view
* add a search bar that will filter rows as the user types

With far less code for you to write to achieve the same result, Titanium makes it very easy to get an impressive prototype up and running, while also being powerful-enough to allow the same code to be developed into a fully-fledged application suitable for public consumption.

# TableView basics

In Titanium, TableViews are created using the `createTableView()` method of the [Titanium.UI.TableView](http://developer.appcelerator.com/apidoc/mobile/latest/Titanium.UI.TableView-object) object, which acts as a container for [Titanium.UI.TableViewRow](http://developer.appcelerator.com/apidoc/mobile/latest/Titanium.UI.TableViewRow-object) objects. Rows containing similar information can be grouped together into *sections* by adding them to [Titanium.UI.TableViewSection](http://developer.appcelerator.com/apidoc/mobile/latest/Titanium.UI.TableViewSection-object) objects.

## The 'Simple' TableView

<info>
The term *Simple* TableView describes a table that can be created without TableViewRow objects needing to be explicitly defined.
</info>

To generate a simple table, use an approach like the following:

* create an array of javascript custom objects with your data stored in their `title` property and their `color` property set to your desired text color
* to make the table more meaningful, give it a header and footer:
    * create a styled view to be used as a table header
    * create a styled view to be used as a table footer
* create a [Titanium.UI.TableView](http://developer.appcelerator.com/apidoc/mobile/latest/Titanium.UI.TableView-object) using the [Titanium.UI.createTableView()](http://developer.appcelerator.com/apidoc/mobile/latest/Titanium.UI.createTableView-method.html) method and set the following properties:
    * `backgroundColor` - to prevent the table defaulting to `transparent`
    * `data` - your array of custom objects
    * `headerView` - your styled header view
    * `footerView` - your styled footer view
    * `top`, `bottom`, `left`, `right`, `height`, `width` positioning and dimension properties, common to almost all Titanium objects

<note>
In all these examples, the variable named `weatherData` is used to store the data. Obviously, in order for your app to be useful, in the real world the data would be retrieved on-the-fly from a live source, but static data is used here in the interests of clarity.
</note>

<code class="javascript">
var defaultColor = "#035385";
var window = Titanium.UI.createWindow({
  backgroundColor:'#999'
});
var weatherData = [
  { title:"Mountain View (North America) - Cloudy", color:defaultColor},
  { title:"Washington, DC (North America) - Mostly Cloudy", color:defaultColor },
  { title:"Brasilia (South America) - Thunderstorm", color:defaultColor },
  { title:"Buenos Aires (South America) - Clear", color:defaultColor },
  { title:"Sucre (South America) - Mostly Cloudy", color:defaultColor },
  { title:"London (Europe) - Overcast", color:defaultColor },
  { title:"Moscow (Europe) - Partly Cloudy", color:defaultColor },
  { title:"Prague (Europe) - Clear", color:defaultColor },
  { title:"St Petersburg (Europe) - Snow", color:defaultColor },
];
var headerLabel = Ti.UI.createLabel({
  backgroundColor:defaultColor,
  color:"white",
  font:{ fontSize: 16, fontWeight:"bold" },
  text:"The Weather App",
  textAlign:"center",
  height:35,
  width:320
});
var footerLabel = Ti.UI.createLabel({
backgroundColor:defaultColor,
  color:"white",
  font:{ fontSize:10 },
  text:"[data supplied by Google Weather API]",
  textAlign:"center",
  height:25,
  width:320
});
var table = Ti.UI.createTableView({
  backgroundColor:"white",
  data: weatherData,
  headerView:headerLabel,
  footerView:footerLabel,
  top:10,
  width:320
});
window.add(table);
window.open();
</code>

This results in a very presentable table:

![screenshot of a simple TableView with basic content](../assets/images/guides/tableviews/tableview-simple-basic.png)

In order to gain some insight into how the TableView is constructed, add a `click` event listener to the table, as follows:

<code class="javascript">
table.addEventListener('click', function(e){
  Ti.API.info("Row object  = "+e.row);
  Ti.API.info("Table index = "+e.index);
  Ti.API.info("Row title = "+e.row.title);
  Ti.API.info("Row color = "+e.row.color);
});
</code>

Now, when a row is clicked, the table listener outputs its object type to the log, along with the index at which it is positioned in the table, and the values of the `title` and `color` properties that you configured in your custom objects. For example:

<pre>
Row object  = [Ti.UI.TableViewRow]
Table index = 7
Row title = Prague (Europe) - Clear
Row color = #035385
</pre>

Here you can see that Titanium has intelligently interpreted your custom objects and generated fully-featured TableViewRows from them. This means that you aren't restricted to merely configuring each row's title and text color; you have the whole [Titanium.UI.TableViewRow](http://developer.appcelerator.com/apidoc/mobile/latest/Titanium.UI.TableViewRow-object) API at your disposal.

Therefore, you can easily add graphics to the rows, using TableViewRow properties, to make the information more attractive and intuitive, such as:

* `backgroundImage` - a default image displayed on each row, to provide a graphical backdrop to all other content the row contains
* `backgroundSelectedImage` - the image that is swapped with `backgroundImage` when a row is selected or clicked, much like the mouseover effects used in webpages
* `leftImage` - an image, often an icon, that graphically represents the information in a row
* `height` - just like all views, position (`left`, `right`, `top`, `bottom`) and dimension (`height`, `width`) properties may be used for TableView and TableViewRow objects

The weatherData array now looks like this:

<pre>
var weatherData = [
    { title: "Mountain View (North America)\nCloudy", color: defaultColor, backgroundImage:"images/tableviewrow-bg.png", backgroundSelectedImage:"images/tableviewrow-bg-selected.png", selectedColor: "blue", height: 75, leftImage: "http://www.google.com/ig/images/weather/cloudy.gif" },
    { title: "Washington, DC (North America)\nMostly Cloudy", color: defaultColor, backgroundImage:"images/tableviewrow-bg.png", backgroundSelectedImage:"images/tableviewrow-bg-selected.png", selectedColor: "blue", height: 75, leftImage: "http://www.google.com/ig/images/weather/mostly_cloudy.gif" },
    { title: "Brasilia (South America)\nThunderstorm", color: defaultColor, backgroundImage:"images/tableviewrow-bg.png", backgroundSelectedImage:"images/tableviewrow-bg-selected.png", selectedColor: "blue", height: 75, leftImage: "http://www.google.com/ig/images/weather/thunderstorm.gif" },
    { title: "Buenos Aires (South America)\nClear", color: defaultColor, backgroundImage:"images/tableviewrow-bg.png", backgroundSelectedImage:"images/tableviewrow-bg-selected.png", selectedColor: "blue", height: 75, leftImage: "http://www.google.com/ig/images/weather/sunny.gif" },
    { title: "Sucre (South America)\nMostly Cloudy", color: defaultColor, backgroundImage:"images/tableviewrow-bg.png", backgroundSelectedImage:"images/tableviewrow-bg-selected.png", selectedColor: "blue", height: 75, leftImage: "http://www.google.com/ig/images/weather/mostly_cloudy.gif" },
	{ title: "London (Europe)\nOvercast", color: defaultColor, backgroundImage:"images/tableviewrow-bg.png", backgroundSelectedImage:"images/tableviewrow-bg-selected.png", selectedColor: "blue", height: 75, leftImage: "http://www.google.com/ig/images/weather/cloudy.gif" },
	{ title: "Moscow (Europe)\nPartly Cloudy", color: defaultColor, backgroundImage:"images/tableviewrow-bg.png", backgroundSelectedImage:"images/tableviewrow-bg-selected.png", selectedColor: "blue", height: 75, leftImage: "http://www.google.com/ig/images/weather/partly_cloudy.gif" },
	{ title: "Prague (Europe)\nClear", color: defaultColor, backgroundImage:"images/tableviewrow-bg.png", backgroundSelectedImage:"images/tableviewrow-bg-selected.png", selectedColor: "blue", height: 75, leftImage: "http://www.google.com/ig/images/weather/sunny.gif" },
	{ title: "St Petersburg (Europe)\nSnow", color: defaultColor, backgroundImage:"images/tableviewrow-bg.png", backgroundSelectedImage:"images/tableviewrow-bg-selected.png", selectedColor: "blue", height: 75, leftImage: "http://www.google.com/ig/images/weather/snow.gif" },
];
</pre>

As repeating oneself often gets on peoples' nerves, and for good reason, it's best to express it a more succinctly by applying the common styling properties separately, as in this full script:

<code class="javascript">
var defaultColor = "#035385";
var window = Titanium.UI.createWindow({
    backgroundColor:'#999'
});
var weatherData = [
  { title: "Mountain View (North America)\nCloudy", leftImage: "http://www.google.com/ig/images/weather/cloudy.gif" },
  { title: "Washington, DC (North America)\nMostly Cloudy", leftImage: "http://www.google.com/ig/images/weather/mostly_cloudy.gif" },
  { title: "Brasilia (South America)\nThunderstorm", leftImage: "http://www.google.com/ig/images/weather/thunderstorm.gif" },
  { title: "Buenos Aires (South America)\nClear", leftImage: "http://www.google.com/ig/images/weather/sunny.gif" },
  { title: "Sucre (South America)\nMostly Cloudy", leftImage: "http://www.google.com/ig/images/weather/mostly_cloudy.gif" },
  { title: "London (Europe)\nOvercast", leftImage: "http://www.google.com/ig/images/weather/cloudy.gif" },
  { title: "Moscow (Europe)\nPartly Cloudy", leftImage: "http://www.google.com/ig/images/weather/partly_cloudy.gif" },
  { title: "Prague (Europe)\nClear", leftImage: "http://www.google.com/ig/images/weather/sunny.gif" },
  { title: "St Petersburg (Europe)\nSnow", leftImage: "http://www.google.com/ig/images/weather/snow.gif" },
  ];
// Row styling properties
for(var i=0, ilen=weatherData.length; i<ilen; i++){
	var thisObject = weatherData[i];
	thisObject.backgroundImage = "images/tableviewrow-bg.png";
	thisObject.color = defaultColor;
	thisObject.backgroundSelectedImage = "images/tableviewrow-bg-selected.png";
	thisObject.selectedColor = "black";
	thisObject.height = 75;
}
var headerLabel = Ti.UI.createLabel({
	backgroundColor:defaultColor,
	color:"white",
	font:{ fontSize: 16, fontWeight:"bold" },
	text:"The Weather App",
	textAlign:"center",
	height:35,
	width:320
});
var footerLabel = Ti.UI.createLabel({
	backgroundColor:defaultColor,
	color:"white",
	font:{ fontSize:10 },
	text:"[data and icons supplied by Google Weather API]",
	textAlign:"center",
	height:25,
	width:320
});
var table = Ti.UI.createTableView({
	backgroundColor:"white",
	data: weatherData,
	headerView:headerLabel,
	footerView:footerLabel,
	separatorColor:"white",
  top:10,
  width:320
});
window.add(table);
window.open();
</code>

This code produces the following result:

![screenshot of a simple TableView with enhanced content](../assets/images/guides/tableviews/tableview-simple-enhanced.png)

As you can see, in about 50 lines of code and a few graphical assets, you can create a very attractive and descriptive display of data.

## The SearchBar

By assigning a [Titanium.UI.SearchBar](http://developer.appcelerator.com/apidoc/mobile/latest/Titanium.UI.SearchBar-object) to the table's `search` property, and adding a few simple event listeners, you can quickly incorporate a useful mechanism for users to filter rows by in typing search terms.

<code class="javascript">
var searchBar = Titanium.UI.createSearchBar({
	hintText:'type to filter rows',
	showCancel:true
});
var table = Ti.UI.createTableView({
  backgroundColor:"white",
  data: weatherData,
  headerView:headerLabel,
  footerView:footerLabel,
  separatorColor:"white",
  top:10,
  width:320
});
searchBar.addEventListener('change', function(e)
{
  // search rows for value of string as user types
  e.source.value;
});
searchBar.addEventListener('return', function(e)
{
  // dismisses the keypad when user presses return
  e.source.blur();
});
searchBar.addEventListener('cancel', function(e)
{
  // dismisses the keypad when user presses the cancel button
  e.source.blur();
});
</code>

This results in a searchBar positioned above the table, containing some `hintText` to make it obvious to the user how it works, and a cancel button, to clear the user's text and reset the table's rows so that all are displayed.

![screenshot of a simple TableView with enhanced content](../assets/images/guides/tableviews/tableview-simple-enhanced-searchbar-unfiltered.png)

Typing into the field dynamically hides all rows except those that contain the string:

![screenshot of a simple TableView with enhanced content](../assets/images/guides/tableviews/tableview-simple-enhanced-searchbar-filtered.png)

## The Standard TableView

For want of a better word, a *Standard* TableView describes a table that has been created using pre-defined TableViewRow objects, rather than relying on the TableView's automated process for their creation.

'Simple' TableViews are appropriate for many situations, and should certainly not be limited to just application prototyping. However, when your table requirements are more involved, with row layouts that consist of one or more child views (ie views, labels, buttons etc) and enhanced functionality, you will need to explicitly create your TableViewRows.

## Improving the data and view layout

Previously, our data wasn't very practical or realistic, as it combined unrelated (or, at least, highly denormalized) pieces of information into the same field, such as location and weather conditions.

<pre>
title: "Mountain View (North America)\nCloudy",
</pre>

Setting the title of a row to this string means that it is unavoidable that the location information must be updated whenever weather conditions change.

Furthermore, user interaction cannot be developed to utilize the information independently of one another. For example, you may desire for the table to be filtered by either continent or weather condition when the user touches the respective string. This would not be possible with the current configuration.

The solution is to split your static, dynamic and interactive data into separate views.

<note>
With this new approach to your application comes an enhanced dataset, provided by a hypothetical Weather API web service, in a format similar to the following:

<pre>
var weatherData = {
  "reports" : [
    {
        "city": "Mountain View",
        "continent": "North America",
        "condition": "Cloudy",
        "icon": "http://www.google.com/ig/images/weather/cloudy.gif",
        "temp_f": 66,
        "temp_c": 19
    },
    {
        "city": "Washington, DC",
        "continent": "North America",
        "condition": "Mostly Cloudy",
        "icon": "http://www.google.com/ig/images/weather/mostly_cloudy.gif",
        "temp_f": 37,
        "temp_c": 3
    },
      ...
      ...
  ]
};
</pre>

From now on, the full dataset will be stored in an external file named [data.js](../assets/javascripts/guides/tableviews/data.js), and called from the main script using the [Ti.include](http://developer.appcelerator.com/apidoc/mobile/latest/Titanium-module) method.
</note>

## Programmatic row creation with Titanium Views

For the reasons explained in the previous section, you can iterate through the dataset using a loop, and assign each item to a separate view:, as this code demonstrates:

<code class="javascript">
Ti.include('data.js');
var defaultColor = "#035385";
var window = Titanium.UI.createWindow({
    backgroundColor:'#999'
});
var headerLabel = Ti.UI.createLabel({
	backgroundColor:defaultColor,
	color:"white",
	font:{ fontSize: 16, fontWeight:"bold" },
	text:"The Weather App",
	textAlign:"center",
	height:35,
	width:320
});
var footerLabel = Ti.UI.createLabel({
	backgroundColor:defaultColor,
	color:"white",
	font:{ fontSize:10 },
	text:"[data and icons supplied by Google Weather API]",
	textAlign:"center",
	height:25,
	width:320
});
var searchBar = Titanium.UI.createSearchBar({
	hintText:'type to filter rows',
	showCancel:true
});
var tableData = [];
for(var i=0,ilen=weatherData["reports"].length; i<ilen; i++){
  var thisObj = weatherData["reports"][i];
  var thisLabelCity = Ti.UI.createLabel({
    color:defaultColor,
    text:thisObj.city,
    top:17,
    left:50
  });
  var thisLabelContinent = Ti.UI.createLabel({
    color:defaultColor,
    text:"(" + thisObj.continent + ")",
    continent:thisObj.continent,
    top:thisLabelCity.top,
    top:thisLabelCity.top + 20,
    left:thisLabelCity.left
  });
  var thisLabelWeatherCondition = Ti.UI.createLabel({
    color:defaultColor,
    text:thisObj.condition,
    left:170
  });
  var thisLabelTemp = Ti.UI.createLabel({
    color:defaultColor,
    font:{ fontWeight:"bold" },
    text:thisObj.temp_f.toString()+"F",
    textAlign:"center",
    temp_f:thisObj.temp_f,
    temp_c:thisObj.temp_c,
    current_unit:"F",
    right:10,
    height:30,
    width:30
  });
  var thisRow = Ti.UI.createTableViewRow({
    backgroundImage:"images/tableviewrow-bg.png",
    backgroundSelectedImage:"images/tableviewrow-bg-selected.png",
    className:"weatherdata",
    leftImage:thisObj.icon,
    layout:",vertical",
    selectedColor:"black",
    height:75
  });
  thisRow.add(thisLabelCity);
  thisRow.add(thisLabelContinent);
  thisRow.add(thisLabelWeatherCondition);
  thisRow.add(thisLabelTemp);
  tableData.push(thisRow);
}
var table = Ti.UI.createTableView({
	backgroundColor:"white",
	data:tableData,
	headerView:headerLabel,
	footerView:footerLabel,
	separatorColor:"white",
	search:searchBar,
  top:10,
  width:320
});
searchBar.addEventListener('change', function(e)
{
	e.source.value;
});
searchBar.addEventListener('return', function(e)
{
	e.source.blur();
});
searchBar.addEventListener('cancel', function(e)
{
	e.source.blur();
});
window.add(table);
window.open();
</code>

<note>
Notice in particular where extra custom properties have been created in the Titanium objects to store additional data. For example:

* `thisLabelContinent` - as the data stored in `title` has been modified for presentation purposes, a property named `continent` has been set with the raw continent data
* `thisLabelTemp` - to easily retrieve the temperature in Fahrenheit and Centigrade, the respective values are stored in `temp_f` and `temp_c`
</note>

## TableView user interaction and events

Modern Graphical User Interfaces with ingenious user interaction have undoubtedly contributed to smartphones' remarkable success in recent years. To add some of this magic to your Weather App you might incorporate the following functionality:

1. Filter by Continent - allow the user to click on the continent string displayed on a row, and for all other rows describing locations in other continents to be filtered out (hidden)
2. Toggle Temperature Units - allow the user to click on the temperature displayed on any row, and for the temperature of all rows to toggle between Fahrenheit and Centigrade
3. Delete Row - allow the user to swipe-right on a row, and for it to be deleted
4. Restore Row - allow the user to swipe-left on a row, and for the last deleted row to be restored at the bottom of the table

### 1. Filter by Continent

One way to provide this functionality would be to determine when the user has clicked on the continent label, using the continent labels' click event listener, and to use it to retrieve the row's raw continent data from the `continent` property that we explicitly added.  This data would then be used for the `searchBar.value` property, causing the default behavior of the searchBar to filter out any rows that do not contain that value. The following code placed inside the `for` loop would achieve the intended result:

<code class="javascript">
	thisLabelContinent.addEventListener('click', function(e){
		searchBar.value = e.source.continent;
	});
</code>

### 2. Toggle Temperature Units

By having a variable named `currentTempUnits` in the script's global context, it will allow you to keep track of whether temperatures are currently being displayed in Fahrenheit or Centigrade.

<code class="javascript">
  var currentTempUnits = "F";
</code>

In order to iterate through each temperature label to switch between the two temperature values, the reference for each label object is added to the `labelTempArray`.  Then, in the label's click event listener,  every label in the array has their `text` changed to the alternate unit of temperature and the `currentTempUnits` variable is toggled to reflect the displayed information.

<code class="javascript">
	labelTempArray.push(thisLabelTemp);
	thisLabelTemp.addEventListener('click', function(){
		for(var i=0,ilen=labelTempArray.length; i<ilen; i++){
			if(currentTempUnits === "F"){
				labelTempArray[i].text = labelTempArray[i].temp_c + "C";

			} else {
				labelTempArray[i].text = labelTempArray[i].temp_f + "F";
			}
		}
		if(currentTempUnits === "F"){
			currentTempUnits = "C";
		} else {
			currentTempUnits = "F";
		}
	});
</code>

## Adding, removing, and inserting rows

The [Titanium.UI.TableView](http://developer.appcelerator.com/apidoc/mobile/latest/Titanium.UI.TableView-object) object provides numerous methods for manipulating your rows, such as:

* appendRow(TableViewRowObject) - add row to the end of the table, by passing a TableViewRow object as a parameter
* updateRow(TableViewRowObject) - replace an existing TableViewRowObject in the table with a new or modified TableViewRowObject
* deleteRow(TableViewRowObject) - delete row from the table, identified by the table's row index number or the
* insertRowBefore(index, TableViewRowObject) - insert a row into a table, positioned before the table row index parameter
* insertRowAfter(index, TableViewRowObject) - insert a row into a table, positioned after the table row index parameter


### 3. Delete Row

A `swipe` event listener attached to the table will allow the TableView deleteRow() method to be triggered when the user swipes using a left to right motion. Just prior to he row being deleted, a reference to the object is pushed to the `rowDeletedArray` array, so that it may be restored by the reverse motion.

See code below.

### 4. Restore Row

The `rowDeletedArray` is created outside the loop and event so that it will persist throughout the life of the script's context.

<code class="javascript">
var rowDeletedArray = [];
</code>

As rows swiped, they are deleted or restored based on the direction of the swipe, which either adds or removes references to the TableViewRow objects from the `rowDeletedArray` variable. Javascript's arrayRemove functionality provided by John Resig's [Array Remove](http://ejohn.org/blog/javascript-array-remove/) solution.

<code class="javascript">
//Array Remove - By John Resig (MIT Licensed)
arrayRemove = function(array, from, to) {
  var rest = array.slice((to || from) + 1 || array.length);
  array.length = from < 0 ? array.length + from : from;
  return array.push.apply(array, rest);
};
table.addEventListener('swipe', function(e){
	if(e.direction === "right"){
		rowDeletedArray.push(e.source.row);
		table.deleteRow(e.index);
	}
	if(e.direction === "left"){
		table.appendRow(rowDeletedArray.length-1);
		arrayRemove(rowDeletedArray,-1);
	}
});
</code>

## Updating the data set

Rather than add and remove rows individually, the whole array of custom objects or TableViewRow objects can be set using the `setData()` method, just like the `data` property was used at table creation.

* setData(object) - replace the existing rows with those derived from custom objects or explicitly-created TableViewRow objects

## Pull to refresh (iOS)

[this section will be added later]

# The Finished Weather App

After implementing all the functionality discussed, the following script is the result:

<code class="javascript">
Ti.include('data.js');
var defaultColor = "#035385";
var window = Titanium.UI.createWindow({
  backgroundColor:'#999'
});
var headerLabel = Ti.UI.createLabel({
  backgroundColor:defaultColor,
  color:"white",
  font:{ fontSize: 16, fontWeight:"bold" },
  text:"The Weather App",
  textAlign:"center",
  height:35,
  width:320
});
var footerLabel = Ti.UI.createLabel({
  backgroundColor:defaultColor,
  color:"white",
  font:{ fontSize:10 },
  text:"[data and icons supplied by Google Weather API]",
  textAlign:"center",
  height:25,
  width:320
});
var searchBar = Titanium.UI.createSearchBar({
  hintText:'type to filter rows',
  showCancel:true
});
var tableData = [];
var labelTempArray = [];
var currentTempUnits = "F";
var rowDeletedArray = [];
for(var i=0,ilen=weatherData["reports"].length; i<ilen; i++){
  var thisObj = weatherData["reports"][i];
  var thisLabelCity = Ti.UI.createLabel({
    color:defaultColor,
    text:thisObj.city,
    top:17,
    left:50
  });
  var thisLabelContinent = Ti.UI.createLabel({
    color:defaultColor,
    text:"(" + thisObj.continent + ")",
    continent:thisObj.continent,
    top:thisLabelCity.top,
    top:thisLabelCity.top + 20,
    left:thisLabelCity.left
  });
  var thisLabelWeatherCondition = Ti.UI.createLabel({
    color:defaultColor,
    text:thisObj.condition,
    touchEnabled:false,
    left:170
  });
  var thisLabelTemp = Ti.UI.createLabel({
    color:defaultColor,
    font:{ fontWeight:"bold" },
    text:thisObj.temp_f.toString()+"F",
    textAlign:"center",
    temp_f:thisObj.temp_f,
    temp_c:thisObj.temp_c,
    right:10,
    height:30,
    width:30
  });
  var thisRow = Ti.UI.createTableViewRow({
    backgroundImage:"images/tableviewrow-bg.png",
    backgroundSelectedImage:"images/tableviewrow-bg-selected.png",
    className:"weatherdata",
    filter:thisObj.continent + " " + thisObj.condition,
    leftImage:thisObj.icon,
    layout:",vertical",
    objectName:"weatherRow",
    selectedColor:"black",
    height:75
  });
  thisRow.add(thisLabelCity);
  thisRow.add(thisLabelContinent);
  thisRow.add(thisLabelWeatherCondition);
  thisRow.add(thisLabelTemp);
  tableData.push(thisRow);
  labelTempArray.push(thisLabelTemp);
  thisLabelTemp.addEventListener('click', function(){
    for(var i=0,ilen=labelTempArray.length; i<ilen; i++){
      if(currentTempUnits === "F"){
        labelTempArray[i].text = labelTempArray[i].temp_c + "C";
      } else {
        labelTempArray[i].text = labelTempArray[i].temp_f + "F";
      }
    }
    if(currentTempUnits === "F"){
      currentTempUnits = "C";
    } else {
      currentTempUnits = "F";
    }
  });
  thisLabelContinent.addEventListener('click', function(e){
  searchBar.value = e.source.continent;
  });
}
var table = Ti.UI.createTableView({
  backgroundColor:"white",
  data:tableData,
  headerView:headerLabel,
  footerView:footerLabel,
  filterAttribute:'filter',
  separatorColor:"white",
  search:searchBar,
  top:10,
  width:320
});
searchBar.addEventListener('change', function(e)
{
  e.source.value;
});
searchBar.addEventListener('return', function(e)
{
  e.source.blur();
});
searchBar.addEventListener('cancel', function(e)
{
  e.source.blur();
});
//Array Remove - By John Resig (MIT Licensed)
arrayRemove = function(array, from, to) {
  var rest = array.slice((to || from) + 1 || array.length);
  array.length = from < 0 ? array.length + from : from;
  return array.push.apply(array, rest);
};
table.addEventListener('swipe', function(e){
  if(e.direction === "right"){
    rowDeletedArray.push(e.source.row);
    table.deleteRow(e.index);
  }
  if(e.direction === "left"){
    table.appendRow(rowDeletedArray.length-1);
    arrayRemove(rowDeletedArray,-1);
  }
});
window.add(table);
window.open();
</code>

# Performance tips

Without some appreciation of how TableViews can be built effectively, they may exhibit slow performance and even crash your application. Here are some ways you can overcome table problems.

## Optimize the results

Before you stipulate that your mobile app must contain a table showing thousands of records at once, spare a thought for the user; will they actually be able to make use of all of them, especially as some may be on the move and using a small device. Just like people rarely reach further than the third page of Google's search results, your users are unlikely to want to scroll down lots of screens of table rows before coming to the one they are interested in.

## Quality over quantity / Limit the scope

Ensure the rows contain only the most relevant results. There is no need for your iHamburgerFinder app to return a list of every fast-food outlet nationwide, when the user needs to reach one on foot.  Consider limiting the results to those restaurants within a reasonable distance of the user's current location and that are open at the time of the inquiry.

## Enhance the UI to encourage manageable results

Related to the previous point, rather than giving the user countless numeric options that they are most likely to dismiss and rely on the "show all" setting instead, determine which search options will have the most impact on refining the results and provide a few simple graphics representing them that can be easily-enabled. For example, to limit distance.

## Incrementally populate table with rows

Just as wouldn't expect your favorite online book store to display the hundreds of books that meet your criteria on a single page, paginate your tables with a "show more results" option rather than passing a massive array of objects to the TableView at its creation. This approach will vastly improve the application's responsiveness when a table initializes.

## Minimize views in programmatic rows

There are many more calculations involved for your mobile platform to produce a *Standard* TableView compared with a *Simple* TableView. Hence, don't use the former when the latter will suffice.

## ALWAYS group similar rows with `className`

Rendering rows is an expensive operation, particularly when their layouts are complex.  By setting the `className` property with a common value for each row layout, you enable the platform to make its calculations once and reuse them for the other similar rows. You should use this property any time you utilize a TableView.







* * *
Much like a number of other views, such as Ti.UI.TextArea and Ti.UI.Picker, the Titanium.UI.TableView object is an enhanced Titanium.UI.ScrollView
some similarities in the API

