

## 1 MongoDB <br>
<p>Let’s start with a few key points about MongoDB itself:
stores data in JSON-like documents that can have various structures
uses dynamic schemas, which means that we can create records without predefining anything
the structure of a record can be changed simply by adding new fields or deleting existing ones
The above-mentioned data model gives us the ability to represent hierarchical relationships, to store arrays and other more complex structures easily.
</p>

<br>

## 2 Terminologies<br>
<p>Understanding concepts in MongoDB becomes easier if we can compare them to relational database structures.
Let’s see the analogies between Mongo and a traditional MySQL system:<br>
 1. Table in MySQL becomes a Collection in Mongo<br>
 2. Row becomes a Document<br>
 3. Column becomes a Field<br>
 4. Joins are defined as linking and embedded documents<br>
This is a simplistic way to look at the MongoDB core concepts of course, but nevertheless useful.
Now, let’s dive into implementation to understand this powerful database.
</p>
 
 <br>
 
## 3 Maven Dependencies<br>
<p> We need to start by defining the dependency of a Java Driver for MongoDB:</p><br>

```java
<dependency>
    <groupId>org.mongodb</groupId>
    <artifactId>mongo-java-driver</artifactId>
    <version>3.4.1</version>
</dependency>
```
<br>

## 4 Using MongoDB<br>
<p>Now, let’s start implementing Mongo queries with Java. We will follow with the basic CRUD operations as they are the best to start with.</p>

<br>

## 5 Make a Connection with MongoClient<br>
<p>First, let’s make a connection to a MongoDB server. With version >= 2.10.0, we’ll use the MongoClient:</p>

```java
MongoClient mongoClient = new MongoClient("localhost", 27017);
```
<p>And for older versions use Mongo class:</p>

```java
Mongo mongo = new Mongo("localhost", 27017);
```

## 6 Connecting to a Database<br>
<p>Now, let’s connect to our database. It is interesting to note that we don’t need to create one. When Mongo sees that database doesn’t exist, it will create it for us:</p>

```java
DB database = mongoClient.getDB("myMongoDb");
```
<p>Sometimes, by default, MongoDB runs in authenticated mode. In that case, we need to authenticate while connecting to a database.
We can do it as presented below:</p>

```java
MongoClient mongoClient = new MongoClient();
DB database = mongoClient.getDB("myMongoDb");
boolean auth = database.authenticate("username", "pwd".toCharArray());
```
## 7 Show Existing Databases<br>
<p>Let’s display all existing databases. When we want to use the command line, the syntax to show databases is similar to MySQL:</p>

```java
show databases;
```

<p>In Java, we display databases using snippet below:</p>

```java
mongoClient.getDatabaseNames().forEach(System.out::println);
```
<p>The output will be:</p>

```java
local      0.000GB
myMongoDb  0.000GB
```
<p>
Above, local is the default Mongo database.
 </p>
 
## 8 Create a Collection<br>
<p>Let’s start by creating a Collection (table equivalent for MongoDB) for our database. Once we have connected to our database, we can make a Collection as:</p>
```java
database.createCollection("customers", null);
```
<p>
Now, let’s display all existing collections for current database:
 </p>
 
```java
database.getCollectionNames().forEach(System.out::println);
```
<p>
The output will be:</p>
```java
customers
```

## 9 Save – Insert<br>
<p>The save operation has save-or-update semantics: if an id is present, it performs an update, if not – it does an insert.
When we save a new customer:</p>

```java
DBCollection collection = database.getCollection("customers");
BasicDBObject document = new BasicDBObject();
document.put("name", "Shubham");
document.put("company", "Baeldung");
collection.insert(document);

```
<p>The entity will be inserted into a database:</p>

```java
{
    "_id" : ObjectId("33a52bb7830b8c9b233b4fe6"),
    "name" : "Shubham",
    "company" : "Baeldung"
}
```
<p>Next, we’ll look at the same operation – save – with update semantics.</p>

## 10 Save – Update<br>
<p>
Let’s now look at save with update semantics, operating on an existing customer:
 </p>
 
```java
{
    "_id" : ObjectId("33a52bb7830b8c9b233b4fe6"),
    "name" : "Shubham",
    "company" : "Baeldung"
}
```
<p>
Now, when we save the existing customer – we will update it:</p>

```java
BasicDBObject query = new BasicDBObject();
query.put("name", "Shubham");
 
BasicDBObject newDocument = new BasicDBObject();
newDocument.put("name", "John");
 
BasicDBObject updateObject = new BasicDBObject();
updateObject.put("$set", newDocument);
 
collection.update(query, updateObject);
```
<p>The database will look like this:</p>

```java
{
    "_id" : ObjectId("33a52bb7830b8c9b233b4fe6"),
    "name" : "John",
    "company" : "Baeldung"
}
```
<p>
As you can see, in this particular example, save uses the semantics of update, because we use object with given _id.</p>

## 11 Read a Document from a Collection<br>

<p>Let’s search for a Document in a Collection by making a query:</p>

```java
BasicDBObject searchQuery = new BasicDBObject();
searchQuery.put("name", "John");
DBCursor cursor = collection.find(searchQuery);
 
while (cursor.hasNext()) {
    System.out.println(cursor.next());
}
```
<p>
It will show the only Document we have by now in our Collection:</p>

```java
[
    {
      "_id" : ObjectId("33a52bb7830b8c9b233b4fe6"),
      "name" : "John",
      "company" : "Baeldung"
    }
]
```

## 12 Delete a Document<br>
<p>
Let’s move forward to our last CRUD operation, deletion:</p>

```java
BasicDBObject searchQuery = new BasicDBObject();
searchQuery.put("name", "John");
 
collection.remove(searchQuery);
```
<p>
With above command executed, our only Document will be removed from the Collection.</p>

## Conclusion<br>
<p>
This article was a quick introduction to using MongoDB from Java.
The implementation of all these examples and code snippets can be found over on GitHub – this is a Maven based project, so it should be easy to import and run as it is.</p>
