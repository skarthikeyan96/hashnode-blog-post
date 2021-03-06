## Getting Started with PouchDB

PouchDB is an open-source JavaScript database inspired by Apache CouchDB that is designed to run well within the browser.
In this article , I will be explaining about the installation of pouchDB and also some basic CRUD operations involved.

Before playing with database we need to install it , below I have mentioned the installation methods for the database.
Installation
For installing pouchDB in your system , you can either include the cdn or install using package manager like npm , bower etc.

**Using npm :**

```
npm install --save pouchdb
```

after installing using , you have to use `require` inorder to use pouchDB for your application.

```
let PouchDB = require('pouchdb');
let db = new PouchDB('my_database');
```

Now the installation part is done , we will see some basic CRUD operations using pouchDB. You can also refer to pouchDB’s official documentation, mentioned the link below,

[Getting started Guide with PouchDB](https://pouchdb.com/getting-started.html)

# **WORKING WITH DATABASE :**

## **CREATING THE DATABASE :**

You can create the local database using PouchDB constructor. Method goes as follows

```jsx
let PouchDB = require('pouchdb');
let db = new PouchDB('test_db1');

console.log ("Database created Successfully.");
```

## **GETTING THE INFO ABOUT THE DATABASE :**

You can get the information about the database using `info()` method

```jsx
let PouchDB = require('pouchdb');
let db = new PouchDB('test_db1');

console.log ("Database created Successfully.");

//You can get the basic information about the database using the method named info()

db.info((err,info) => {
 if(!err){
      console.log(info)
 }
});
```

Now if you run the file from command line , you will be getting the output as below,

```jsx
{
  adapter:"idb"
  auto_compaction:false
  db_name:"test_db222"
  doc_count:0
  idb_attachment_format:"binary"
  update_seq:0
}
```

## **DELETING THE DATABASE :**

You can delete the database using `db.destroy` method. Example for deleting the method goes as follows

```jsx
//Requiring the package
var PouchDB = require('PouchDB');

//Creating the database object
var db = new PouchDB('test_db222');

//deleting database
db.destroy(function (err, response) {
   if (err) {
      return console.log(err);
   } else {
      console.log ("Database Deleted");
   }
});
```

# **WORKING WITH DOCUMENTS :**

PouchDB is a no-sql database , so instead of storing the data in relational form , data is stored in the form of documents. You can perform the basic CRUD operations in documents.

![https://miro.medium.com/max/572/1*ZwPF0XFH4q03H2gwZFYz3Q.png](https://miro.medium.com/max/572/1*ZwPF0XFH4q03H2gwZFYz3Q.png)

[Comparison chart from pouch db official docs](https://pouchdb.com/guides/documents.html)

## **CREATING A DOCUMENT :**

Documents can be created using `db.put` method.

```jsx
let PouchDB = require('pouchdb');
let db = new PouchDB('test_db222');
console.log ("Database created Successfully.");
//creating a new database
//Preparing the document
let doc = {
   _id : '002',
   name: 'Karthi',
   age : 23,
   designation : 'Designer'
   }
//Inserting Document
db.put(doc, function(err, response) {
   if (err) {
      return console.log(err);
   } else {
      console.log("Document created Successfully",response);
   }
});
```

You will get the following message from the console if you run the program

```
Document created successfully
```

```jsx
//Document created Successfully
{ok: true, id: "003", rev: "1-325eb8a7f91c4035a033fca9f5c32f52"}
```

If you try to add the same document , you will get the following error

```jsx
//CustomPouchError 
{
status: 409, 
name: "conflict", 
message: "Document update conflict", 
error: true, 
id: "003"
}
```

## **DISPLAYING THE DOCUMENT :**

You can display the document using `db.get` which will take a `key` as argument which is supposed to be unique.

```jsx
let PouchDB = require('pouchdb');
let db = new PouchDB('test_db222');
//success message
console.log ("Database created Successfully.");
//getting the document from the database
db.get('002',(err,info)=>{
if(!err){
     console.log("db value",info)
}
else{
     console.log("err field",err)
}
});
```

You will get the output as follows

```jsx
{name: "Karthi", age: 23, designation: "Designer", _id: "002", _rev: "1-38fd304f70974adeb370e3cf8bc89f39"}
```

## **UNDERSTANDING REVISIONS (`_rev`)**

The new field `_rev` is called **revision-marker** which will be updated whenever you create or update the documents.We will be using `_rev` when we are updating the documents.

## **UPDATING THE DOCUMENTS :**

So inorder to update the documents , you need to get the `_rev` but you don’t need to manually add the `_rev` as it is already present in the `doc` which we will be fetching.

```jsx
let PouchDB = require('pouchdb');
let db = new PouchDB('test_db222');
//creating the db
console.log ("Database created Successfully.");
//get the data and update
db.get('002').then(function (doc) {
  // update their age
  doc.age = 40;
  // put them back
  return db.put(doc);
}).then(function () {
  // fetch data again
  return db.get('002');
}).then(function (doc) {
  console.log(doc);
});
```

Now you will get the following result in the console

```jsx
{name: "Karthi", age: 40, designation: "Designer", _id: "002", _rev: "3-9a7dac1a7b1a4332b9d44d9dd5dca757"}
```

## **DELETING THE DOCUMENTS:**

You can use the `db.delete()` method to delete the documents in the database.

```jsx
let PouchDB = require('pouchdb'); //require the package 
let db = new PouchDB('test_db222');
//created the db
console.log ("Database created Successfully.");
//get the doc
db.get('002').then(function (doc) {
// remove the doc
return db.remove(doc._id, doc._rev);
}).then(function(err,info){
//print the data
if(!err){
console.log("info",info)
}else{
console.log(err)
}

});
```

You will get the following output with result

```jsx
{
  ok: true,
  id: "002", 
  rev: "4-d6f058da696f4760b31492c9f00ad1ec"
}
```
## **Conclusion:**

That's pretty much it. Thank you for taking the time to read the blog post. If you found the post useful , add ❤️ to it and let me know if I have missed something in the comments section. Feedback on the blog are most welcome.


Let's connect on twitter : (**[https://twitter.com/karthik_coder](https://twitter.com/karthik_coder)**)

![https://media.giphy.com/media/eujb1tWaj3ZxS/giphy.gif](https://media.giphy.com/media/eujb1tWaj3ZxS/giphy.gif)


## **References :**

1. [pouchDB official Docs](https://pouchdb.com/guides/)
2. [pouchDB tutorial — Tutorials point](https://www.tutorialspoint.com/pouchdb/index.htm)