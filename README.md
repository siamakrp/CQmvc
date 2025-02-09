# CQmvc

A Quick And Clean PHP MVC Framework
======================================

CQmvc is a PHP MVC Framework that is a lightweight and Clean and Quick implementation IMHO.
---------------------------------------------------------------------------------- 
It's focus is on Request Model Bindings, Reducing need to imports or includes ,Creating a Clean and Quick Design with the help of Action Parameters Maping and View Constructors and Isolationg Controller-Action-View logic from UI Scripts, Creating a Quick yet easy to troubleshoot errors with forcing Developers to use PHP Type Hints, Handling MVC base URLs in base Controller and so in.
--------------------------------------------------------

In CQmvc, you can POST data with the exact Object Graph Navigation Model, that is, CQmvc with the help of a Runtime class, translates, auto expands and initiates and maps to your existing parameters defined in your controller's actions. 
------------------------------------------------------------------------------------------------


CQMvc A PHP MVC Framework
 
Copyright 2015 by Mohamad Zeinali mohamad.basu@gmail.com
 
Licensed to the Apache Software Foundation (ASF) under one
or more contributor license agreements.  See the NOTICE file
distributed with this work for additional information
regarding copyright ownership.  The ASF licenses this file
to you under the Apache License, Version 2.0 (the
"License"); you may not use this file except in compliance
with the License.  You may obtain a copy of the License at
 
  http://www.apache.org/licenses/LICENSE-2.0
 
Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
KIND, either express or implied.  See the License for the
specific language governing permissions and limitations
under the License.

There are othere codes inside the Framework that may provided from other sources and have licensed
to their respective owners. Checkout source codes for more informations.

This List maybe incomplete, Contact me if you feel something is missing and I'll sort things up!


A simple PHP CAPTCHA script
Copyright 2011 Cory LaViska for A Beautiful Site, LLC. (http://abeautifulsite.net/)

Licensed under the MIT license: http://opensource.org/licenses/MIT

------------------------------------------------------------------------------------------------------


Lets Start with a few definitions and some examples.

**1) Definitions And Frameworks Overview:**

You all know what is MVC, -> Controller<->Model<->View

Each Controller, have some functions called Actions in which get requests and with the help of business layer and Model classes, load proper Views.

Models in most cases, are some Plain Old Classes that define Domain Model structures. In most cases, all data that need to be posted to the application, are just some fields to create a Model Object. Then, having this Model Object in hand, most validations and business specific processes are done to this Model Object and finally outcomes are generated and returned in Views.

Views are collection of Classes and Scripts that help creating User Interfaces.


In CQmvc, there is a base Controller called Ctrl.
all Controllers should extend Ctrl.

Views were implemented a little different that other implemetations you ever seen (at least, as far as I know)
each View consists of a View Class and a View script.
View class extends from Framework's View class and dynamically loads the View Script.

Like other implementions in other languages (.NET ASP MVC, JAVA SPRING-WEB and ...), There is no Base Model Class in this framework. Data Technology is totally up to you. Whether you want an ORM or just a simple PDO based connection, CQmvc wont interviene.
This is just like the other frameworks in the other Languages. For example in ASP MVC.NET, One can decide to use Entity Framework or NHibernate or ...  

In order to use CQmvc, All the framework specifiec Classes and Resources should be in the root directory and your Application Classes in the App folder. The overal structure is as below:


App -> folder

res -> folder

.htaccess

Ctrl.php

FileBase.php

index.html

MvcUrl.php

Route.php

Runtime.php

SimpleCaptcha.php

Store.php

UUID.php

Validator.php

View.php

Like I said, this structure should be in the root and your Application specific codes live in the App folder.
inside App, the structure should be like this:


Control -> this folder contains Control classes.

View -> this folder contains View Classes and Scripts

Route -> this folder contains Route Class(es)

Those three folders are the main folders of any Applications.
In CQmvc, there is a consept of Classpath. Meaning, some folders are in Frameworks Classpath and every classes that you put in there, with a few Rules, can be seen by Controllers and Views and thus no need to include or imports or whatever.
This can be a greate saving in developers coding time.

These Classpath folders are:

Model

ViewModel

Helper

Res

Also Control and View Folder are in Classpath by default. Despite that, For those who like everything to be
Organized and Clean in coding, Some scpecial subfolders inside those folders are in Classpath too.

For example imagine you have a Controller defined as MyControl.php inside Control class. All Classes that are only used by
this Controller, can be inside Model/MyControl/ or Helper/MyControl/ or Res/MyControl/ or ViewModel/MyControl/

Also All Views that are Rendered by MyControl, canbe inside View/MyControl/

And you don't need a single include or whatever for those classes. Classes that are shared between some Controllers Or Views, Should be live directly inside those folders otherwise you need to include them.

Oh! almost forgot to say that, Ther is a single naming rule to make that Classpath thing work!
Every Class should have the same name as its file. So in our example, The MyControl should be defined in a file Called MyControl.php.

Please note that this does not mean that every file should contain exactly one Class.
For example imagine I've defined a Model Called MyModel inside MyModel.php.
This MyModel uses a helper class Called MyModelHelper. I can easily put MyModelHelper inside MyModel.php if and only if MyModel class gets called first in the Controllers or Views or other places otherwise it should be explicitly included.

Besides that, it can find classes that have namesapces and those namespaces exactly determine where the class is.

To show how, suppose you have defined a Data Access Object class inside the DLA folder of your project.

DAL/MYSQL/MyAccessClass.php

and here is the MyAccessClass.php


    namespace DAL\MYSQL;

    class MyAccessClass {}

Doing so, you don't need to include the DAL/MYSQL/MyAccessClass.php inside your codes ...

Only one thing before starting the examples, and its about the Views.

Like I mentioned before, Views in this Framework, are slightly different.

Each View consists of a View class and a View Script. For example UserView is created by defining a class called UserView extending View in UserView.php and a Script as _UserView.php UserView dynamically finds and Loads that _UserView.php script.
The easiest way is to follow that naming rule. But you can change the script name and tell the UserView class what is its name.


**2) Examples**

These examples is proved inside the source code in the App folder.

a) First of all we create the Test Controller inside Control folder of App directory.


App/Control/Test.php


    class Test extends Ctrl {
	
	
	public function index() {
	
		//How it handles views
		
		$this->view(new Index());
		$this->render();
		//or use this, it'll GZip the the output
		//$this->renderGz();
	}
    }

In the above class, we created an Action called index. Its going to be the index of our application.

Inside it, new Index() creates a View Class. Inside 

App/View/Test/Index.php


    class Index extends View {
		
    }

Just like that! You could define that View directly inside:

App/View

just like we talked about.

its script is like this:

App/View/Test/_Index.php

<?php

?>
```html
<html>
<body>

<fieldset>
<legend><b>Form1</b></legend>
This simply gets user's name and his children and post the data
<form action="/Test/postTest" method="post">
id <input name="id" /> <br /> 
Name <input name="myModel->name" /><br />
children[0] <input name="myModel->children{0}" /><br />
children[1] <input name="myModel->children{1}" /><br />
<input type="submit" value="post" />
</form>
</fieldset>
</body>
</html>
```

In the above Script, we created a simple form. In that form, we are going to post some informations for our model and
a single id value.
We'r going to post his/her name, and two of his/her childern.

So what is the structure of myMode? How /Test/postTest could handle those wierd namings and map them to model?

Lets look at the Models structure.

App/Model/UserModel.php


    class UserModel {
	
	/**
	 * 
	 * @var array
	 */
	public $children;
	
	public $name;
	
	/**
	 * 
	 * @var FileBase
	 */
	public $profileImage;
	
	/**
	 * 
	 * @var unknown
	 */
	public $profileImageUrl;
	
	/**
	 * 
	 * @var array
	 */
	public $postedFiles;
    }
   
It is just a simple PHP Class.

Now the /Test/postTest Action inside Test controller:


    public function postTest(UserModel $myModel = null, $id = 0) {
		
		print "id: $id<br />";
		
		echo "Name $myModel->name <br />";
		
		echo "Children<br />";
				
		foreach ($myModel->children as $x) {
			
			echo "$x<br />";
	}
    }

In the postTest Action, we define post parameters as Action's Arguments And should hint him what the expected types are.

Also its a good practice to bring default values for every Arguments.

Now All you need to do is to fill in some blah blah and click the submit button.

Its printing those you filled ha? But how?

Easy! Frameworks Runtime gets the Request data and routes and converts and maps them to the Action's Parameters.

Inside the Form, you can think of the {} as [] for arrays and the -> for dot. So the myModel->children{0} means the first child of the model and the children should be an array defined in Model Class.


Now lets take a more complex example:


b) Suppose the children array in UserMode consists of a objects of type ChildModel


App/Model/ChildModel.php


    class ChildModel {
	
	public $name;
	public $lastName;
    }

So we have this form:


```html
<form action="/Test/complexFormTest" method="post" enctype="multipart/form-data" >

<fieldset>
<legend>User1</legend>
	Name <input name="myModels{0}->name" /></br>
	children{0}->name <input name="myModels{0}->children{0}->name" /><br />
	children{0}->lastName <input name="myModels{0}->children{0}->lastName" /><br />
	children{1}->name <input name="myModels{0}->children{1}->name" /><br />
	children{1}->lastName <input name="myModels{0}->children{1}->lastName" /><br /><br />
	Profile Image <input name="myModels{0}->profileImage" type="file" /><br />
	<br />File1 <input name="myModels{0}->postedFiles{0}" type="file" /><br />
	File2 <input name="myModels{0}->postedFiles{1}" type="file" /><br />
</fieldset>
<br />

<fieldset>
<legend>User2</legend>
	Name <input name="myModels{1}->name" /></br>
	children{0}->name <input name="myModels{1}->children{0}->name" /><br />
	children{0}->lastName <input name="myModels{1}->children{0}->lastName" /><br />
	children{1}->name <input name="myModels{1}->children{1}->name" /><br />
	children{1}->lastName <input name="myModels{1}->children{1}->lastName" /><br /><br />
	Profile Image <input name="myModels{1}->profileImage" type="file" /><br />
	<br />File1 <input name="myModels{1}->postedFiles{0}" type="file" /><br />
	File2 <input name="myModels{1}->postedFiles{1}" type="file" /><br />
</fieldset>

<input type="submit" value="post" />
</fieldset>
</form>
```

In the above form, we want to submit multiple users data each with their own children and profile image and some other files specified to each user.

Here is the complexFormTest Action defined inside Test Controller:

    public function complexFormTest(array $myModels = null) {

		foreach ($myModels as $u) {
			
			echo "Name: $u->name <br />";
			
			echo "Profile Image Name ";
			print $u->profileImage->name."<br />";
			
			echo "Children:<ul>";
			
			/* @var $x ChildModel */
			foreach ($u->children as $x) {
					
				echo "<li>$x->name $x->lastName</li>";
			}
			print "</ul>";
			echo "Posted Files Name<ul>";
			
			/* @var $file FileBase */
			foreach ($u->postedFiles as $file) {
			
				print "$file->name <br />";
			}
			print "</ul>";
			echo "<hr />";
	    } 
    }  


Every file you post, is converted to the FileBase object that has some helper methods to save or validate the file.

You also can post multi dimentional arrays and ...

CQmvc also provides Templating using Master View Technique. 
Models and ViewModels can be passed to Views by Constructors directly or assign to View Class Fields.
Checkout the example provided with the source inside App folder.

CQmvc also has some helper Classes like SimpleCaptcha from (https://github.com/claviska/simple-php-captcha) Converted to a suitable Class and a UUID Class directly copy and pasted from http://php.net/manual/de/function.uniqid.php.
Also there is a Store Class for serializatoins and Also a Simple Validator Class.
