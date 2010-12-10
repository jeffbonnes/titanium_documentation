<summary>
This guide will teach you how to use the `Titanium.Network.HTTPClient-object` API to create registration and login for your application using PHP and MySQL. 

[Click here](http://www.souzoufidelity.com/files/httpclientsource.zip) to download the source for this tutorial.

In this tutorial, you will:

* Learn how to send login &amp; registration variables to a server script via the `Titanium.Network.HTTPClient-object` API
* Learn how to query the database to validate user login &amp; registration via `PHP`
* Learn how to interpret the server-provided `JSON` output based on the data that has been sent
</summary>

#Introduction

I will be building this application for iPhone, however it should not matter which platform you wish to use.

##Before We Start

In this tutorial, I will be using the XAMPP package for Apache, MySQL &amp; PHP.
If you do not have Apache, MySQL and PHP set up already, please [download](http://www.apachefriends.org/en/xampp.html) and [install](http://dalibor.dvorski.net/downloads/docs/InstallingConfiguringDevelopingWithXAMPP.pdf).
<note>
You can use a different package for Apache, MySQL &amp; PHP if you so desire - XAMPP is only personal preference.
</note>

Once you have your AMP (Apache, MySQL, PHP) server installed and running, [create a new database](http://w3schools.com/sql/sql_create_db.asp) called db_mobile.
After you've created the new database, [create a new table](http://w3schools.com/sql/sql_create_table.asp) in it called mobile_users with the following columns:

* 'id' INT NOT NULL AUTO_INCREMENT PRIMARY KEY
* 'email' VARCHAR( 255 ) NOT NULL
* 'user' VARCHAR( 255 ) NOT NULL
* 'pass' VARCHAR( 255 ) NOT NULL

#Creating The Project

Once you have your working local webserver and database configured, go ahead and start up the **Titanium Developer** - we're going to create a new project.
Click on the `New Project` button to create a new project. Change the project type from the default **Desktop** to **Mobile** and name the project `MyDatabaseApp`. As for the rest of the fields, I will let you decide what to put in them.

![NewProject](http://img.skitch.com/20101205-rjsuuc7ec1e2pthmydmf73x4bn.png)

Once your project has been successfully created, navigate to the project's `Resources` folder. In here you will find some default generated items such as the application entry point, **app.js** and a few icons and pictures.
In the `Resources` folder, go ahead and create 3 new javascript files:

* config.js
* RegistrationWindow.js
* LoginWindow.js

As you can assume, the `config.js` file will hold our **global variables** as well as other configurations. The `RegistrationWindow.js` file will hold our code behind our registration window and the `LoginWindow.js` will hold the code behind our login window. 

##app.js
Go ahead and remove all of the code in `app.js` and put in the following code:
<code class="javascript">
//set the default background color
Titanium.UI.setBackgroundColor('#FFF');

//create tab group
var tabGroup = Titanium.UI.createTabGroup();

//create our registration window
var registerWindow = Titanium.UI.createWindow({
    title: 'Register',
    backgroundColor: '#FFF',
    url: "RegisterWindow.js"
});

//create our login window
var loginWindow = Titanium.UI.createWindow({
	title: 'Login',
	backgroundColor: '#FFF',
	url: "LoginWindow.js"
});

//create the tab to hold the registration window
var registerTab = Titanium.UI.createTab({  
    icon: 'register.png',
    title: 'Register',
    window: registerWindow
});

//create the tab to hold the login window
var loginTab = Titanium.UI.createTab({
	icon: 'login.png',
	title: 'Login',
	window: loginWindow
});

//add our tabs to the tabgroup
tabGroup.addTab(registerTab);
tabGroup.addTab(loginTab);

//display the tabgroup
tabGroup.open();
</code>

The code above simply sets up our default UI that we will use to display our `Registration` and `Login` window. This tutorial assumes you understand the basic concepts of tabs &amp; windows so I'm not going to go into detail explaining this code snippet.
You can go ahead and compile and run your application if you want to make sure everything is going smoothly.
Here is what you should see:
<info>
Results may vary depending on iPhone or Android
</info>

![first result](http://img.skitch.com/20101205-r3uedp1gctk6rt1iepuyb7r7tg.png)

Once finished, let's create our `Registration` form. 

#Registration Form

This will be the `Registration` form for our users. It will contain fields for them to put in their basic information such as email address, username and desired password as well as contain validation behind the form.

##Building The Registration Form

<note>
Again, I'm not going to go into detail for how to build this UI. If you do not understand what is going on here, please familiarize yourself [with this](ui_design.html).
</note>
Simply copy and paste this code into **RegistrationWindow.js**:

<code class="javascript">
var curWindow = Titanium.UI.currentWindow;

var scrollView = Titanium.UI.createScrollView({
    contentWidth:'auto',
    contentHeight:'auto',
    top:0,
    showVerticalScrollIndicator:true,
    showHorizontalScrollIndicator:true
});

var view = Titanium.UI.createView({
    width:"100%",
    height: 400,
    top:10,
    paddingBottom: 15
});

var lbl_Email = Titanium.UI.createLabel({
    text:'Email:',
    height:30,
    width:"98%",
    color:'#222',
    font:{fontSize:16},
    top: 10
});

var txt_Email = Titanium.UI.createTextField({
	width: "98%",
	color: "#222",
	paddingLeft: 5,
	border: 1,
	borderColor: "gray",
	borderRadius: 3,
	font:{fontSize:16},
	height: 40,
	top: 40
});
	
var lbl_User = Titanium.UI.createLabel({
    text:'Username:',
    height:30,
    width:"98%",
    color:'#222',
    font:{fontSize:16},
    top: 90
});

var txt_User = Titanium.UI.createTextField({
	width: "98%",
	color: "#222",
	paddingLeft: 5,
	border: 1,
	borderColor: "gray",
	borderRadius: 3,
	font:{fontSize:16},
	height: 40,
	top: 120
});
	
var lbl_Pass1 = Titanium.UI.createLabel({
    text:'Password:',
    height:30,
    width:"98%",
    color:'#222',
    font:{fontSize:16},
    top: 170
});

var txt_Pass1 = Titanium.UI.createTextField({
	width: "98%",
	color: "#222",
	paddingLeft: 5,
	border: 1,
	borderColor: "gray",
	borderRadius: 3,
	font:{fontSize:16},
	height: 40,
	top: 200,
    passwordMask: true
});

var lbl_Pass2 = Titanium.UI.createLabel({
    text:'Repeat Password:',
    height:30,
    width:"98%",
    color:'#222',
    font:{fontSize:16},
    top: 250
});

var txt_Pass2 = Titanium.UI.createTextField({
	width: "98%",
	color: "#222",
	paddingLeft: 5,
	border: 1,
	borderColor: "gray",
	borderRadius: 3,
	font:{fontSize:16},
	height: 40,
	top: 280,
    passwordMask: true
});

var btn_Register = Titanium.UI.createButton({
	width: "98%",
	color: "#555",
	font:{fontSize: 22, fontWeight: "bold"},
	height: 50,
	top: 340,
	title: "Register"
});

view.add(lbl_Email);
view.add(txt_Email);
view.add(lbl_User);
view.add(txt_User);
view.add(lbl_Pass1);
view.add(txt_Pass1);
view.add(lbl_Pass2);
view.add(txt_Pass2);
view.add(btn_Register);

scrollView.add(view);	
curWindow.add(scrollView);
</code>

<info>
Notice that this was built for the `iPhone` UI, it **will** look different on `Android`.
</info>

If we compile and run our code now, the output should look something like this:

![register form](http://img.skitch.com/20101205-dbuwtmq1wtus5udw4c868ncjii.png)

If you are satisfied with your application's layout, let us move on to the next step.

##Adding The Code To Register

Here we're going to create two functions - one to grab the values from the text field and do minimal form validation and the second one will run our HTTPClient request and do something depending on the response we receive from the server. The response will be in `JSON` format. 

<note>
If you are unfamiliar with `JSON`, then I recommend [reading this](http://www.learn-ajax-tutorial.com/Json.cfm).
</note>

The first thing we're going to do is create a new function called `getUserInfo`. What this function will do is grab the values from each text field and do minimal form validation, then it will pass the textfield values to another function which we will create named `registerUser`. Let's go ahead and set up our `getUserInfo` function.

Add the following code after your `btn_Register` declaration:

<code class="javascript">
function getUserInfo(){
	var email = txt_Email.value;
	var user = txt_User.value;
	var pass1 = txt_Pass1.value;
	var pass2 = txt_Pass2.value;	
	if(pass1 == pass2){
		registerUser(email, user, pass1);
	}
	else{
		alert("Your passwords do not match.");
	}
}
</code>

As stated earlier, the above function simply grabs the text value from each of the textfields and does minimal form validation by checking to see if the two passwords match. If so, then it calls a second function which we will create next and passes the corresponding variables to it.

Next, let's add an [Event Listener](http://developer.appcelerator.com/apidoc/mobile/latest/Titanium.UI.Button.addEventListener-method.html) to our btn_Register object:

<code class="javascript">
btn_Register.addEventListener('click',getUserInfo);
</code>

<note>
We could probably create a global function that could also be referenced later when we create our `Login` form, but for the sake of this tutorial, I will stick to proprietary functions. 
</note>

Now, we get to the fun stuff! Here we will write the function that sends our user-entered variables to a server script using the Titanium.Network.HTTPClient-object API. This function will also be used to interpret the response we receive from the server which we will write in the next chapter.

I'm going to display the function in its entirety then break it down line-by-line.

Here is the whole function:

<code class="javascript">
function registerUser(email, user, pass){
	var request = Titanium.Network.createHTTPClient();
	var url = "http://localhost/mobile.php?action=add_user&email="+email+"&user="+user+"&pass="+pass;
	request.open("GET",url);
	request.onload = function(){
		var arrData = [];
		arrData = eval('('+this.responseText+')');
		var result = arrData[0].result;
		if(result == "true"){
			var alrt_Success = Titanium.UI.createAlertDialog({
				title: 'Success!',
				message: 'You have successfuly created a new account. Now you can log in.',
				buttonNames: ['OK']
			});
			alrt_Success.show();
		}
		else if(result == "another_user"){
			var alrt_Sorry = Titanium.UI.createAlertDialog({
				title: 'Sorry!',
				message: 'According to our database, that username is already in use.',
				buttonNames: ['OK']
			});
			alrt_Sorry.show();
		}
		else{
			var alrt_Fail = Titanium.UI.createAlertDialog({
				title: 'Error!',
				message: 'It seems we are experiencing problems... please try again later.',
				buttonNames: ['OK']
			});
			alrt_Fail.show();
		}
	};
	request.send();
}
</code>

Now that you've seen the big picture (and hopefully aren't intimidated by it), let's go through this function line-by-line

<code class="javascript">
var request = Titanium.Network.createHTTPClient();
</code>

This is simply creating a new `Titanium.Network.HTTPClient-object` variable.

<code class="javascript">
var url = "http://localhost/mobile.php?action=add_user&email="+email+"&user="+user+"&pass="+pass;
</code>

This line creates a new string variable with the `URL` of our server - in this case it's `localhost`. Otherwise, where `localhost` is, you would enter the `IP Address` or `URL` of the desired web-server.

After the `URL` of our server, you will notice that we have a `?` - this is where we add our `GET` variables delimited by the &amp; symbol. In our case, we are sending three different `GET` variables to the webserver - email, user and pass. The `action` variable that we are sending through the `URL` is letting the server know which function to run. Later on, we will use the **action=login** `GET` variable when writing our `Login` form logic.

<code class="javascript">
request.open("GET",url);
</code>

This designates two properties to our `Titanium.Network.HTTPClient-object` variable: the type of data to send - in our case `GET` and the `URL` string which we declared the in the line above this.

<code class="javascript">
request.onload = function(){...}
</code>

This line assigns a function to the `onload` item of our `Titanium.Network.HTTPClient-object` variable.

And now, inside our `onload` function:

<code class="javascript">
var arrData = [];
arrData = eval('('+this.responseText+')');
var result = arrData[0].result;
</code>

The first line creates a new variable array which will store our response data.

The next line evaluates and assigns the `JSON` responseText received from the server by performing an `eval` function with the parameter, **this.responseText**.
<note>
The keyword, **this** refers to the `parent object` which is our **request** variable.
</note>

The final line creates a simple `string` variable which will house the value of our **arrData[0].result** which we received from the server.
<note>
The only reason for doing this is so we don't have to type **arrData[0].result == ...** over and over.
</note>

The following `if, else` statements simply determine the course of action based off of our **result** variable.

<info>
If you're wondering where the "true" and "another_user" values come from, don't worry. That is something on the server-side we will write in the next chapter.
</info>

<code class="javascript">
if(result == "true"){
	var alrt_Success = Titanium.UI.createAlertDialog({
		title: 'Success!',
		message: 'You have successfuly created a new account. Now you can log in.',
		buttonNames: ['OK']
	});
	alrt_Success.show();
}
</code>

This code snippet, assuming the **result** value equals `true`, creates a new `Titanium.UI.AlertDialog-object` variable and assigns it some generic values and text to be displayed if the user has successfully created a new account.

The following code snippet shows the course of action based on the other possible **result** or if **result** happens to be `NULL` - meaning that it was unable to retrieve a proper `JSON` value.

<code class="javascript">
else if(result == "another_user"){
		var alrt_Sorry = Titanium.UI.createAlertDialog({
    	title: 'Sorry!',
    	message: 'According to our database, that username is already in use.',
    	buttonNames: ['OK']
	});
	alrt_Sorry.show();
}
else{
	var alrt_Fail = Titanium.UI.createAlertDialog({
    	title: 'Error!',
        message: 'It seems we are experiencing problems... please try again later.',
        buttonNames: ['OK']
	});
    alrt_Fail.show();
}
</code>

And now, the final line of code in our `registerUser` function.

<code class="javascript">
request.send();
</code>

This line of code calls the execution function our `request` variable named `send`.

You should now be able to compile and run the application. However, nothing will appear to be happening if you click the `Register` button. That is because we have not wrote our server script yet which brings us to the next chapter.

#Handling Registration

In this section, we will write a small `PHP` script that will query our `MySQL` database to check if the user already exists and if not, it will insert the user-provided values into the database.

After doing so, our `PHP` script will output `JSON` values depending on our database transactions.

Since we haven't created our server script yet, let's begin this chapter by doing so.

Go into your server's `htdocs` or `www` folder (whatever your webroot directory is named) and created a new text file called **mobile.php**.

Inside it, go ahead and write the following code:

<code class="javascript">
<?php echo "Test123"; ?>
</code>

Save the file and open up your web browser. Now, navigate to: http://localhost/mobile.php

You sould get the text response, Test123.

If all is well, then continue on to the first part of this chapter.

##Database Registration

<info>
If you are unfamiliar with the `PHP` server-scripting language, then I highly recommend you take a look at the [w3schools website](http://w3schools.com/php/php_intro.asp) for documentation and tutorials on `PHP`.
</info>

If you don't already have the **mobile.php** file opened in a text editor, then go ahead and open it.

We're going to write a `switch` statement to allow the script to identify whether it should execute an `add_user` command (for registration) or `login` command (for logging in).

Remove the prevoius code and Copy &amp; Paste the following code into your **mobile.php** file:

<code>
<?php
	switch($_GET['action'])
	{
		case 'add_user':
		break;
		case 'login':
		break;
	}
?>
</code>

Inside of the first `case`, **add_user**, we will put our code to retrieve the `GET` variables we passed it through our mobile application.

<code>
$email = $_GET['email'];
$user = $_GET['user'];
$pass = md5(trim($_GET['pass']));
</code>

The above code creates primitive `PHP` typeless variables and assigns them the values of our `GET` variables.

Take a look at the **$pass** variable. Notice I handled this one a little different. Since we will be storing the user's password in our database, we will want to hash it using a standard `md5` algorithm. I am also using `PHP`'s **trim** method to remove all white-spaces in front of and behind the `GET` variable.

<note>
You can use the `Titanium.Utils.md5HexDigest-method` to implement md5 `cryptographic hashing` but in this tutorial I will be using `PHP`'s md5 function to hash the user's password.

If you are unfamiliar with `cryptographic hashing` methods, then I recommend [reading this](http://en.wikipedia.org/wiki/Cryptographic_hash_function).
</note>

Next, we will perform a `mysql_query` against our database and, depending on the results, output our data.

Copy and paste the following code after our `PHP` variable declarations:

<code>
$resultArr = Array();
$connection = mysql_connect("localhost","root","");
$db = mysql_select_db("mobile",$connection);
$q_string = "SELECT * FROM mobile_users WHERE user = '$user'";
$query = mysql_query($q_string,$connection);
</code>

The first line of code creates a `PHP` Array object. This object will store our `JSON` array temporarily.

The second line of code connects the script to our `MySQL` database.

<note>
I'm using the default values for the MySQL connection that are created upon AMP installation.

Otherwise, it's 'URL', 'username', 'password'.
</note>

The third line selects the database that we will be using, in our case it's the **mobile** database we created at the [beginning of this tutorial](#before_we_start).

The fourth line creates the query string in which we will use to check to see if anybody has already registered using this username.

The fifth line executes the query and stores the value into our **$query** variable.

Next, I will show you how to output the result in a way that the mobile phone will be able to interpret it.

##Registration Results

After the previous code, go ahead and type in the following:

<code>
if(mysql_num_rows($query) > 0){
	$resultArr[0]['result'] = "another_user";	
}
else{
	$query = mysql_query("INSERT INTO mobile_users (user, email, pass) VALUES ('$user', '$email', '$pass')",$connection);
	if($query)
		$resultArr[0]['result'] = "true";
	else
		$resultArr[0]['result'] = "false";
}
echo json_encode($resultArr);
mysql_close($connection);
</code>

The first `if` statement we see checks to see if the query has returned a row count of greater than zero. In this case, if it does, that means that the desired username has already been chosen by someone else. If the username has indeed been chosen by someone else, we will assign our `PHP` **$resultArr** array at `index` 0, the **result** property a value of "another_user".

<note>
Now you see where these variables come into play when interpreting the server-response inside of our mobile application.
</note>

The `else` statement executes if the desired username does not already exist.

The first line in our `else` statement executes a `mysql_query` that will `INSERT` the username, email address and password provided by the user into the database (or at least attempt to).
If the query is successful, then we will assign our `PHP` **$resultArr** array at `index` 0, the **result** property a value of "true" meaning that the new user has been successfully created.
Otherwise, something has gone wrong and we will assign it "false".

The following line will encode our `PHP` **resultArr** array into `JSON` formatting and the output it onto the page.

<note>
You can manually run this script by going to http://localhost/mobile.php?action=add_user&user=TestUser&pass=12345&email=TestEmail

You do not have to run it from the mobile emulator.
</note>

The last line of code simply closes our `MySQL` database connection, since we will no longer need it to create a new user.

Your `PHP` file should look like this:
<code>
switch($_GET['action'])
{
	case 'add_user':
		$user = $_GET['user'];
		$pass = md5(trim($_GET['pass']));
		$email = $_GET['email'];
		$resultArr = Array();
		$connection = mysql_connect("localhost","root","");
		$db = mysql_select_db("mobile",$connection);
		$query = mysql_query("SELECT * FROM `mobile_users` WHERE `user` = '$user'",$connection);
		if(mysql_num_rows($query) > 0)
		{
			$resultArr[0]['result'] = "another_user";	
		}
		else
		{
			$query = mysql_query("INSERT INTO mobile_users (user, email, pass) VALUES ('$user', '$email', '$pass')",$connection);
			if($query)
				$resultArr[0]['result'] = "true";
			else
				$resultArr[0]['result'] = "false";
		}
		echo json_encode($resultArr);
		mysql_close($connection);
	break;
	case 'login':
	break;
}
</code>

Once you have finished putting in all of your server-side code, go ahead and compile and run your mobile application using `Titanium`.

Once open, go ahead and fill out your newly-functioning `Registration` form and click on your `Register` button.

You should see the following output:

![register success](http://img.skitch.com/20101206-j3pdm65ahrqnydi2qw7ryk966m.png)

Once your comfortable that your `Registration` form has been succesfully created &amp; implemented, you can continue on to the next chapter to create your `Login` form. 

#Login Form

In this chapter, I'm going to show you how to create a `Login` form for our existing users. What will happen is the user will fill out our `Login` form and when they click on the `Log In` button, our application will send the user-entered data to the server and our server will query the database to see if it finds any matches.

##Building The Login Form

Here, we're going to build the UI for our `Login` form. If you're this far in the tutorial, you should already know the logistics for UI building.

Go ahead and copy &amp; paste the following code into your **LoginWindow.js**.

<code class="javascript">
var curWindow = Titanium.UI.currentWindow;
var scrollView = Titanium.UI.createScrollView({
    contentWidth:'auto',
    contentHeight:'auto',
    top:0,
    showVerticalScrollIndicator:true,
    showHorizontalScrollIndicator:true
});
var view = Titanium.UI.createView({
    width:"100%",
    height: "auto",
    top:10,
    paddingBottom: 15
});
var lbl_User = Titanium.UI.createLabel({
    text:'Username:',
    height:30,
    width:"98%",
    color:'#222',
    font:{fontSize:16},
    top: 10
});
var txt_User = Titanium.UI.createTextField({
	width: "98%",
	color: "#222",
	paddingLeft: 5,
	border: 1,
	borderColor: "gray",
	borderRadius: 3,
	font:{fontSize:16},
	height: 40,
	top: 40
});
var lbl_Pass = Titanium.UI.createLabel({
    text:'Password:',
    height:30,
    width:"98%",
    color:'#222',
    font:{fontSize:16},
    top: 90
});
var txt_Pass = Titanium.UI.createTextField({
	width: "98%",
	color: "#222",
	paddingLeft: 5,
	border: 1,
	borderColor: "gray",
	borderRadius: 3,
    passwordMask: true,
	font:{fontSize:16},
	height: 40,
	top: 120
});
var btn_Login = Titanium.UI.createButton({
	width: "98%",
	color: "#555",
	font:{fontSize: 22, fontWeight: "bold"},
	height: 50,
	top: 180,
	title: "Log In"
});
view.add(lbl_User);
view.add(txt_User);
view.add(lbl_Pass);
view.add(txt_Pass);
view.add(btn_Login);
scrollView.add(view);	
curWindow.add(scrollView);
</code>

If you compile &amp; run, you should get the following result:

![login form](http://img.skitch.com/20101207-bb917mynqnqi6hf9fr2pxaa32a.png)

If you are happy with your layout, then go ahead to the next step.

##Adding The Code To Log In

Here, we're going to write the client-side logic for logging in.
If you don't already have your **LoginWindow.js** file open, go ahead and do so.

The first thing we're going to do is create our `getUserinfo` function which will be our event handler for our `btn_Register` object. It will grab the values from the `Username` and `Password` fields and pass those values to a function we will later create, named **LoginUser**.

To do so, let's go ahead and put this code in after your main var declarations:

<code class="javascript">
function getUserInfo(e){
	var user = txt_User.value;
	var pass = txt_User.value;
	loginUser(user,pass);
}

btn_Login.addEventListener('click',getUserInfo);
</code>

Now we're going to create our **LoginUser** function.

This function will receive our `User` and `Pass` variables from the prior function, and then send the user-provided login data to our server using the `Titanium.Network.HTTPClient-object` API and interpret the `JSON` output provided by the server.

Let's look at the function in it's entirety:

<code class="javascript">
function loginUser(user,pass){
	var request = Titanium.Network.createHTTPClient();
	var url = "http://localhost/mobile.php?action=login&user="+user+"&pass="+pass;
	request.open("GET",url);
	request.onload = function()
	{
		var arrData = [];
		arrData = eval('('+this.responseText+')');
		var result = arrData[0].result;
		if(result == "true")
		{
			Titanium.App.Properties.setString("user_id",arrData[0].user_id);
			var alrt_Success = Titanium.UI.createAlertDialog(
			{
				title: 'Success!',
				message: 'You are now logged in and can access member-only features.',
				buttonNames: ['OK']
			});
			alrt_Success.show();
		}
		else
		{
			var alrt_Sorry = Titanium.UI.createAlertDialog(
			{
			    title: 'Unsuccessful...',
			    message: 'You have provided the incorrect username and/or password.',
			    buttonNames: ['OK']
			});
			alrt_Sorry.show();
		}
	};
	loader.send();
}
</code>

Now let's break this function down a little bit. You should know how most of this code works from before, but we'll go through it anyway.

<code class="javascript">
var request = Titanium.Network.createHTTPClient();
</code>

This line simply creates a new `Titanium.Network.HTTPClient-object` variable.

<code class="javascript">
var url = "http://localhost/mobile.php?action=login&user="+user+"&pass="+pass;
</code>

In this line, we are pointing to the server script we want to execute, we are telling it which function to run - in this case, login and we are passing the user-provided login variables.

<code class="javascript">
request.open("GET",url);
</code>

This line let's the request know which method we're going to use - in this case `GET` and also we give it the `URL` that we created earlier.

<code class="javascript">
request.onload = function(){...}
</code>

This assigns our `request` variable's `onload` method.

<code class="javascript">
var arrData = [];
arrData = eval('('+this.responseText+')');
var result = arrData[0].result;
if(result == "true")
{
	Titanium.App.Properties.setString("user_id",arrData[0].user_id);
	var alrt_Success = Titanium.UI.createAlertDialog(
	{
		title: 'Success!',
		message: 'You are now logged in and can access member-only features.',
		buttonNames: ['OK']
	});
	alrt_Success.show();
}
else
{
	var alrt_Sorry = Titanium.UI.createAlertDialog(
	{
		title: 'Unsuccessful...',
		message: 'You have provided the incorrect username and/or password.',
		buttonNames: ['OK']
	});
	alrt_Sorry.show();
}
</code>

The rest of this code, as seen above, grabs our result set provided by the server in `JSON` format (which we will write in the next chapter) and depending on the value, it will either successfully log in or let the user know they have provided a wrong username and/or password.

<note>
Never let the user know which of the two items they have provided is incorrect. This is for security reasons.
</note>

Notice that this time, we have another value other than just result in our `arrData` variable. The server has provided us upon successful login with the user's ID. This is **the most precious** value that links a user to his/her account. Nobody should **ever** be able to figure this out. (other than the server admin).

<code class="javascript">
Titanium.App.Properties.setString("user_id",arrData[0].user_id);
</code>

What we are doing here is creating a global application variable with the user's ID. This way, we can easily reference this at later points in our application.

From here, we can create a new window that only logged-in users can access. I'm not going to cover that in this tutorial, however.

<info>
Upon user logout, you should always set the global application property to 0 or NULL.
</info>

Now that our `Login` form is created and functional, the only thing left to do is write our server script to handle user login and return the proper values that our application can interpret.

#Handling Login

Here, we're going to add the necessary `PHP` code to our server.

##Database Login

Go ahead and open your **mobile.php** file if it's not open already.

In it, find your case 'login' for your switch statement and in there put the following code:
<code>
$resultArr = Array();
$user = $_GET['user'];
$pass = md5(trim($_GET['pass']));
$connection = mysql_connect("localhost","root","");
$db = mysql_select_db("mobile",$connection);
$query = mysql_query("SELECT * FROM `mobile_users` WHERE `user` = '$user' AND `pass` = '$pass'");
if(mysql_num_rows($query) > 0)
{
	$row = mysql_fetch_assoc($query);
	$resultArr[0]['result'] = "true";
	$resultArr[0]['user_id'] = $row['id'];
}
else
{
	$resultArr[0]['result'] = "false";	
}
echo json_encode($resultArr);
mysql_close($connection);
</code>

You should notice a slight difference in our **$resultArr** variable. It now has another `key` called **id**.

This **id** is unique per user. Nobody should ever be able to access another user's **id** or they can get access to that user's entire account.

You should understand what this code now does so I will not go into detail about it.

Your **final** `PHP` file should look like this:
<code>
<?php
	switch($_GET['action'])
	{
		case 'add_user':
			$user = $_GET['user'];
			$pass = md5(trim($_GET['pass']));
			$email = $_GET['email'];
			$resultArr = Array();
			$connection = mysql_connect("localhost","root","");
			$db = mysql_select_db("mobile",$connection);
			$query = mysql_query("SELECT * FROM `mobile_users` WHERE `user` = '$user'",$connection);
			if(mysql_num_rows($query) > 0)
			{
				$resultArr[0]['result'] = "another_user";	
			}
			else
			{
				$query = mysql_query("INSERT INTO mobile_users (user, email, pass) VALUES ('$user', '$email', '$pass')",$connection);
				if($query)
					$resultArr[0]['result'] = "true";
				else
					$resultArr[0]['result'] = "false";
			}
			echo json_encode($resultArr);
			mysql_close($connection);
		break;
		case 'login':
			$resultArr = Array();
			$user = $_GET['user'];
			$pass = md5(trim($_GET['pass']));
			$connection = mysql_connect("localhost","root","");
			$db = mysql_select_db("mobile",$connection);
			$query = mysql_query("SELECT * FROM `mobile_users` WHERE `user` = '$user' AND `pass` = '$pass'");
			if(mysql_num_rows($query) > 0)
			{
				$row = mysql_fetch_assoc($query);
				$resultArr[0]['result'] = "true";
				$resultArr[0]['user_id'] = $row['id'];
			}
			else
			{
				$resultArr[0]['result'] = "false";	
			}
			echo json_encode($resultArr);
			mysql_close($connection);
		break;
	}
?>
</code>

##Login Results

As explained above, you will be returning that user's **unique id** in which the apllication can now access that users's specific profile information. For instance, if we were building a task scheduling application, we would be able to select all tasks from our database where their user_id was equal to the id we have stored in our application.

To access the logged in user's **id**, you can call:
<code>
var user_id = Titanium.App.Properties.getString("user_id");
</code>

This is the end! Go ahead and compile and run your code and if it works, congratulations on building your first `Registration` and `Login` application for the iPhone and/or Android!