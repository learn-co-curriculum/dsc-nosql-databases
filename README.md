# NoSQL Databases

## Introduction

In this lesson, we'll learn about some of the various kinds of NoSQL databases, and their use cases.

## Objectives
You will be able to:  

* Explain why NoSQL is useful  
* Explore real-world use cases of NoSQL databases


## Why NoSQL?

Relational databases are a cornerstone of a modern technology. They're reliable, dependable, and they seem to be pretty much everywhere. Since their invention at IBM by Edgar Codd in 1970, they've rapidly grown to be used all over the place. Their creation allowed companies to track, store, and analyze data in ways that simply couldn't be done before. For the majority of situations, they're a great choice. However, as technology has progressed into the era of internet and smartphones, we've run into many different sorts of data that aren't a natural fit for a relational format. Let's examine a few of these situations, and see why NoSQL might be a better choice. 

Let's assume that we need to store chat logs between customer service and customers through a web interface. These chats could be very short, or very long -- some chats may only be 2 or 3 messages, while others may conceivably be in the hundreds or thousands. For each message in the chat, we want to store metadata associated with the message, so that we can know things like which party sent the message, the time it was sent, the time the other party read the message, etc. This is a great use case for a NoSQL database, because it would be a very poor fit for a relational database. For starters, each chat can be any size, meaning that we can't just clearly define a table that links which messages belong to which chat. If every chat had only five messages, we might be able to make it work, with a column for "message 1", "message 2", and so on -- but we can't, because a chat has no set size, and can always grow larger in the future. A relational database would also waste a lot of space storing redundant information, and getting all the messages in a chat could result in some nasty runtimes for our SQL query if this data was stored in separate tables in something like the third normal format. 

For each of the database types below, we'll explore some brief real-world examples of when they're the ideal choice. As with the example above, the common thread that runs among all of them is that they aren't a great fit for a relational database. 


## Document Stores

A **_Document Store_** is a database that stores records as unique documents in the database. These documents can be arbitrarily long, and can even contain other documents inside of them! The chat log example we saw above is a prime use case for a document store. In a document store, we could store each message and its accompanying metadata as a document, and then embed each of those documents in order in a chat document. In this way, we can easily access the data as needed. 

In these Document Stores, each document contains key-value pairs, with the actual data being stored in as the value. This makes Document Stores incredibly flexible, because each document can be unique. There is no constraint saying each document must have the same keys! This makes it great for working with data where we don't know what shape it will take (as we saw above, with chat logs that can be arbitrarily long or short), or perhaps when we don't know what data will be stored at all. This would be a problem in a relational database, because we would need to know what column the data belongs in before we could store it. With a Document Store, we can just create a key on the fly for the data that matters to us!

Note that while this flexibility makes it easy for us to store data on the fly, this also makes it harder for us to query data and get exactly what we need. Since each different document can potentially have its own **_Schema_**, this means that we have to know what we are looking for. This also means that we have to be diligent in our naming conventions, because `chatLog` is different than `ChatLog`. This means that if we run a query across all documents to get all data with the key `chatLog`, we'll completely miss any data where they key is written as `ChatLog`!

##### Popular  Document Store Databases: MongoDB and Couchbase

| <img src="https://curriculum-content.s3.amazonaws.com/data-science/images/mongo-db-logo.png" height=60% width=60%> | <img src="https://curriculum-content.s3.amazonaws.com/data-science/images/couchbase-logo.png"> |
|---------------------|---------------------|

## Key-Value Stores and Column Stores

Although **_Key-Value Stores_** and **_Column Stores_** are two different kinds of databases, they both store their data in ways you're already familiar with, because you've seen them in Python and Pandas!  **_Key-Value Stores_** work exactly like they sound -- just like a Python dictionary! They are powered by a hash table under the hood, and to access data, you just pass in the unique key for the value that you want. 

**_Column Stores_** work a bit more like how pandas stores data under the hood. Recall that in pandas, a DataFrame is stored as a collection of `Series` objects, with each column getting it's own series. The same general principle is at work for Column Stores -- each column is stored separately, allowing for very fast reads when querying data. 

##### Popular Key-Value Store Database: Amazon DynamoDB

##### Popular Column Store Database: Google BigTable

| <img src="https://curriculum-content.s3.amazonaws.com/data-science/images/bigtable.png" height=60% width=60%>    | <img src="https://curriculum-content.s3.amazonaws.com/data-science/images/dynamodb.png"> |
|---------------------|---------------------|

## Graph Databases

**_Graph Databases_** are based on the idea of the kind of graphs you might have seen in higher-level math classes -- the kind that are made up of **_Nodes_** and **_Edges_**. These sorts of databases excel at storing data where there are multiple associations between the entities stored in the database. The most obvious use case here is social media. Think for a second about what it would take to represent all of your friends on Facebook or Twitter in a relational database. This would be a pretty gargantuan data engineering task! However, with Graph Databases, it's quite easy. In a graph database, every user is a node, and each connection between you and one of the nodes representing one of your Facebook friends is an edge. In each node, we can store an arbitrary amount of metadata, which allows us to store things like user information. These sorts of databases really shine when you need to ask questions about the connections between users or objects in a database. For instance, what if we wanted to determine who the greatest influencers are among the Data Science community on Facebook? Even thinking about how you might approach this problem in a relational database is a surefire way to get a headache, but this sort of query is quite easy in a Graph Database -- one approach might be to examine all the Nodes with a metadata tag listing their job as "Data Scientist", and then examine who has the most shared connections among this community. 

##### Popular Graph Databases: Neo4J and OrientDB


| <img src="https://curriculum-content.s3.amazonaws.com/data-science/images/neo4j-logo.png"> | <img src="https://curriculum-content.s3.amazonaws.com/data-science/images/orientdb-logo.png"> |
|---------------------|---------------------|



## RDDs and Big Data

The Term "RDD" is short for **_Resilient Distributed Dataset_**, and it's the backbone behind all the fuss surrounding "Big Data". The most popular platforms that support RDDs are Hadoop and Spark, which are related to one another. Spark is built on top of Hadoop and allows users a cleaner, faster way of dealing with distributed datasets. A _Resilient Distributed Dataset_ is a collection of an arbitrary number of server clusters that share a full dataset distributed between them. With multiple servers comes multiple opportunities for things like server failures, and our dataset is no good if we have no guarantee that part of our dataset might be missing when we go to query for it. Relational databases are stored on a single server, which means that when the server is down, there's no doing anything until it comes back up. However, what if our Hadoop cluster contains 100 servers, and 2 are down when we write our query?  Without a way of making sure our RDD is **_Fault Tolerant_**, we could get incorrect information back because we think we've queried the entire dataset, but don't realize that two servers weren't up to contribute their data to the query. The way that RDDs like Hadoop and Spark deal with this is by chopping the data into different labeled blocks, and then storing multiple different backups of each block across all the different server clusters. For example, let's say that we've divided our dataset into blocks labeled, A, B, and C. Server 1 will host block A, but will also contain a backup of block B, and a backup of Block C, in that order. Server 2 will host Block B, but will contain backups of Block B and Block C, while Server 3 will host C first and contain backups of A and B. When things are working smoothly, Server 1 will run the query passed only on block A, Server 2 on Block B, etc. If Server B goes down, then our query will run on Blocks A and C because of Servers 1 and 3 first, and then the backup will kick in and Server 1 will then run the query on Block B. 

If you've never worked with RDDs before, this probably sounds slow and unnecessarily redundant. On small or normal-sized datasets, you're absolutely right. However, in this day and age, there has been a massive explosion in the amount of data created and stored every day. When working with truly _Big Data_, the distributed architecture provides massive speedups to query times by distributing the workload across multiple servers. 

As an example, let's pretend that you're a Data Scientist for a major retailer that sells everything under the sun at discount prices -- we'll call this company WallMart. At the end of every day, your boss wants to know the total number of sales in each department. If your data was stored in a relational database, this would mean executing a query on a single, truly massive relational database, which can only work as fast as its processor allows. Each query would have to go through trillions of rows to find what it needs, aggregate the sales for each category, and then return the answer. Running this query on a traditional relational database would likely take days, or even weeks! 

However, this would be a very different story with an RDD like Hadoop or Spark. Instead of running this on a single machine, we can just put a small server in each store location to keep track of all that store's data and transactions. When we need to run a query, our distributed system will ask each store's server to get the total number of sales for each department _simultaneously_. For a single store, this is a much smaller query, so it will run quite quickly. Then, once we have the totals for each department from each store, we can just combine them together into grand totals for each, getting us the answer we needed in seconds instead of days! The idea of having each server run some query or function at the same time, and then combining the results is based on the idea of **_MapReduce_**, which is the secret sauce behind RDDs. When we ask each server to run the function or query at the same time, this is the "Map" step. When we combine the results from each server into a single aggregate, this is the "Reduce" step. 


| <img src="https://curriculum-content.s3.amazonaws.com/data-science/images/hadoop.png"> | <img src="https://curriculum-content.s3.amazonaws.com/data-science/images/spark.png" height=10% width=10%> |
|---------------------|---------------------|

## Summary

In this lesson, we learned about the various sorts of NoSQL Databases in the market today. We dug into the similarities and differences between them all, and also looked at a few examples where a NoSQL Database is a more natural fit for storing data than a traditional relational database. 
