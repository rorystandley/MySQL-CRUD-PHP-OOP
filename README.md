**To use this class with MySQLi please find it <a href="https://github.com/rorystandley/MySQLi-CRUD-PHP-OOP">here</a>**

A simple PHP Class for using with MySQL create, read, update and delete functions. Using OOP this class can easily be added to to enhance existing functions or create more.

mysql-crud-php-oop is being sponsored by the following tool; please help to support us by taking a look and signing up to a free trial
<a href="https://tracking.gitads.io/?repo=mysql-crud-php-oop"> <img src="https://images.gitads.io/mysql-crud-php-oop" alt="GitAds"/> </a>

**Using The Class**
--

**Database Credentials**

You will need to change some variable values in the Class, that represent those of your own database. Change the following -

```php
private $db_host = "localhost";  // Change as required
private $db_user = "username";  // Change as required
private $db_pass = "password";  // Change as required
private $db_name = "database";	// Change as required
```

**Test MySQL**

Start by creating a test table in your database -

```mysql
CREATE TABLE IF NOT EXISTS CRUDClass (
  id int(11) NOT NULL AUTO_INCREMENT,
  name varchar(255) NOT NULL,
  email varchar(255) NOT NULL,
  PRIMARY KEY (id)
);

INSERT INTO CRUDClass VALUES('','Name 1','name1@email.com');
INSERT INTO CRUDClass VALUES('','Name 2','name2@email.com');
INSERT INTO CRUDClass VALUES('','Name 3','name3@email.com');
```

**Select Example**

Use the following code to select * rows from the databse using this class

```php
<?php
include('class/mysql_crud.php');
$db = new Database();
$db->connect();
$db->select('CRUDClass'); // Table name
$res = $db->getResult();
print_r($res);
```

Use the following code to specify what is selected from the database using this class

```php
<?php
include('class/mysql_crud.php');
$db = new Database();
$db->connect();
$db->select('CRUDClass','id,name','name="Name 1"','id DESC'); // Table name, Column Names, WHERE conditions, ORDER BY conditions
$res = $db->getResult();
print_r($res);
```

**Join Example**

Start by creating another table in your database -

```mysql
CREATE TABLE IF NOT EXISTS CRUDClassChild (
  id int(11) NOT NULL AUTO_INCREMENT,
  parentId int(11) NOT NULL,
  name varchar(255) NOT NULL,
  PRIMARY KEY (id)
);

INSERT INTO CRUDClassChild VALUES('','1','Child 1');
INSERT INTO CRUDClassChild VALUES('','1','Child 2');
INSERT INTO CRUDClassChild VALUES('','2','Child 1');
```

Use the following code to select rows using a join in the database using this class

```php
<?php
include('class/mysql_crud.php');
$db = new Database();
$db->connect();
$db->select('CRUDClass','CRUDClass.id,CRUDClass.name,CRUDClassChild.name','CRUDClassChild ON CRUDClass.id = parentId','CRUDClass.name="Name 1"','id DESC'); // Table name, Column Names, JOIN, WHERE conditions, ORDER BY conditions
$res = $db->getResult();
print_r($res);
```

**Update Example**

Use the following code to update rows in the database using this class

```php
<?php
include('class/mysql_crud.php');
$db = new Database();
$db->connect();
$db->update('CRUDClass',array('name'=>"Name 4",'email'=>"name4@email.com"),'id="1" AND name="Name 1"'); // Table name, column names and values, WHERE conditions
$res = $db->getResult();
print_r($res);
```

**Insert Example**

Use the following code to insert rows into the database using this class

```php
<?php
include('class/mysql_crud.php');
$db = new Database();
$db->connect();
$data = $db->escapeString("name5@email.com"); // Escape any input before insert
$db->insert('CRUDClass',array('name'=>'Name 5','email'=>$data));  // Table name, column names and respective values
$res = $db->getResult();  
print_r($res);
```

**Delete Example**

Use the following code to delete rows from the database with this class

```php
<?php
include('class/mysql_crud.php');
$db = new Database();
$db->connect();
$db->delete('CRUDClass','id=5');  // Table name, WHERE conditions
$res = $db->getResult();  
print_r($res);
```

**Full SQL Example**

Use the following code to enter the full SQL query

```php
<?php
include('class/mysql_crud.php');
$db = new Database();
$db->connect();
$db->sql('SELECT id,name FROM CRUDClass');
$res = $db->getResult();
foreach($res as $output){
	echo $output["name"]."<br />";
}
```

**jQuery and AJAX Example**

Use the following code to select names from the database with this class using jQuery and AJAX

```html
<!DOCTYPE html>
<!--[if lt IE 7]>      <html class="no-js lt-ie9 lt-ie8 lt-ie7"> <![endif]-->
<!--[if IE 7]>         <html class="no-js lt-ie9 lt-ie8"> <![endif]-->
<!--[if IE 8]>         <html class="no-js lt-ie9"> <![endif]-->
<!--[if gt IE 8]><!--> <html class="no-js"> <!--<![endif]-->
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
        <title></title>
        <meta name="description" content="">
        <meta name="viewport" content="width=device-width">
        <script src="//ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"></script>
        <script>
        	$(function(){
        		$.ajax({
        			url:"ajax/select.php",
        			dataType:"json",
        			success:function(data){
        				$.each(data, function(index){
        					$("#names").append("<li>"+data[index].name+"</li>")
        				});
        			}
        		});
        	});
        </script>
    </head>
    <body>
        <!--[if lt IE 7]>
            <p class="chromeframe">You are using an <strong>outdated</strong> browser. Please <a href="http://browsehappy.com/">upgrade your browser</a> or <a href="http://www.google.com/chromeframe/?redirect=true">activate Google Chrome Frame</a> to improve your experience.</p>
        <![endif]-->
        <ul id="names"></ul>
    </body>
</html>
```

```php
<?php
header('content-type: application/json; charset=utf-8');
include('../class/mysql_crud.php');
$db = new Database();
$db->connect();
$db->select('CRUDClass','name'); // Table name, Column Name
$res = $db->getResult();
echo json_encode($res);
```
