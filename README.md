

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
