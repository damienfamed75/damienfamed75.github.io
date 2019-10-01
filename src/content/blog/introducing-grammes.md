---
title: "Introducing Grammes"
date: 2019-09-30T19:21:24-05:00
draft: false
image: "images/blog/grammes-desk.png"
description: "Grammes. A Golang package built to communicate with Apache TinkerPop graph computing framework using Gremlin."
author: "Damien Stamates"
type: "post"
---

Grammes is a Go (Golang) package built to communicate with Apache TinkerPop™ Graph computing framework using Gremlin.

Gremlin is a graph traversal language used by graph databases such as JanusGraph®, Microsoft Azure Cosmos DB, Amazon Neptune, and DataStax® Enterprise Graph.

Something to be noted is that the compatibility for Microsoft Azure Cosmos DB, Amazon Neptune, and Datastax® Enterprise Graph are not yet finished.

#### Why Did We Make Grammes

##### Implementing Graph Databases in Go

Implementing a graph database into a Go workflow is difficult when there aren’t any full-fledged packages or libraries to interact with Gremlin completely from inside Go.

##### A Direct Method to Interact with Gremlin in Go

There were no ways to traverse (query) the server without feeding in some strings with Groovy in them. Then in return, you’d receive raw and non-formatted data.

---

#### Creating Grammes

##### A Simple and Easy Client

While designing Grammes the choice was obvious to make a new client interact with the Gremlin server. This client handles all the connection, authentication, and graph database traversing (querying) for the user, but if you don’t want to use a client we’ve added a package to traverse (query) the database without the client. After finalizing all the design decisions, what we achieved was a simple and easy client with custom configurations. This is achievable by passing in client configuration functions as arguments.

##### Choosing a Graph Database for Testing

For testing and development, we used JanusGraph®. Grammes is compatible with Amazon Neptune, Cosmos DB, DataStax® Enterprise Graph, and more as long as you set up the structs. (it is planned to implement these into the package itself.)

---

#### What We Did

##### Building Blocks for the Traversal (Queries)

Using Grammes’ traversal (query) building blocks, there are endless possibilities when building commands for the Gremlin server, without having to touch the console once. This is possible because the traversal and its associated functions mirror the Gremlin counterparts (java).

##### Custom Types from Gremlin to Grammes

Grammes has packages for most types in Gremlin so you can use them in the same way you would in the Gremlin console. These include types such as Multiplicity, Tokens, Search Predicates, and Scopes.

##### Structures for Vertices, Edges, and Properties

For these types, we have structures and methods related to them. Grammes handles all the traversal (query) construction for altering the vertices if you so wish to use these structures. Now you can interact with the vertices on the graph in memory without manually traversing (querying) the database every time.

---

#### The Finished Product

##### Features of Grammes

- _Grammes handles all authentication and secure connections,_
  - allowing you to use usernames, passwords, and challenge IDs all within the client itself to establish a connection to your Gremlin server.

- _Traversing (querying) a Gremlin server_
  - is the most important feature of Grammes. Many functions belonging to the Client use this feature when adding vertices, getting outer edges from a vertex, and more.

- _Storing vertices and edges in structures_
  - accomplishes one of the main goals of the Grammes package. To interact with your graph without having to traverse *(query)* it every single time by hand; allowing you to interact with your vertex and its edges with only a `struct` of it.

- _Traversal (Query) building blocks,_
  - grants you the ability to build custom traversals *(queries)* directed toward the Gremlin server with *auto-complete*. From the Gremlin server, your command gets carried over to the graph database of your choice.

- _Custom types to match Gremlin._
  - In the Grammes package, we’ve created custom types to match those in Gremlin. This is so you can use those types exactly how you would in the Gremlin console to allow for many opportunities.

- _Functions to use Gremlin without making traversals (queries)_
  - help new users who don’t know the Gremlin language still interact with the database.
A “quick” package; interacts with a graph without a client.
Having an API for Gremlin interact with a graph database is one thing, but having an API that allows you to do it quickly is another. This package does all the above in an easy and concise way of allowing newcomers to learn without having to know Gremlin at all.

- _Plenty of documentation and examples available._
  - In the Grammes repository, there are various examples of how to use the various functions available. There is also plenty of documentation and diagrams to show how everything works in Grammes internally.

---

![Grammes Gopher Teaching](/images/blog/grammes-teaching.png)

#### Getting Started

##### Prerequisites

You need to set up all of the following tools to use Grammes

- [Go 1.11.1+](https://golang.org/dl/)
- [Java 1.8](https://www.java.com/en/download/)
- [JanusGraph®](https://github.com/JanusGraph/janusgraph/releases)

##### Cloning Grammes

Open up a terminal for yourself and begin by cloning the Grammes repository.

```sh
go get -u github.com/northwesternmutual/grammes
```

##### Setting up JanusGraph®

First off, direct yourself to the Grammes repository.

```sh
cd $GOPATH/src/github.com/northwesternmutual/grammes
```

Next, you want to change directory to the `scripts/` folder in the root of the project.

```sh
cd scripts/
```

Now run the `./janusgraph.sh` script to begin downloading and turning on the graph database. If you meet any issues please find your way to the bottom of the article.

```sh
./janusgraph.sh
```

While you're still here let's make sure the installation of JanusGraph® was successful by running the `gremlin.sh` script located in the same directory.

```sh
./gremlin.sh
```

If Gremlin ran successfully then run these commands and make sure it matches below.

```sh
gremlin> :remote connect tinkerpop.server conf/remote.yaml
===>Configured localhost/127.0.0.1:8182
gremlin> :exit
```

Finally, if the script ran successfully with no errors then you may check if the graph and server are running by checking the status.

```sh
./janusgraph status
```

##### Using Grammes

_Once you have your graph database working then it's time to begin using Grammes in your project._

Begin by creating a place for your code, i.e.,

```sh
$GOPATH/src/github.com/<username here>/<project nbame here>
```

Next, you want to create a `main.go` file in the project directory you created. For this example, my editor of choice is MS Visual Studio Code and I will be using it to initialize this file. You may use other commands or editors.

```sh
code main.go
```

In this you can begin by making it a typical empty `main.go` file like this:

<script src="https://gist.github.com/damienstamates/e0c9c6e944c25e8be7ea15627e89e817.js"></script>

Now let's import the Grammes project and begin using it by creating a client that will connect to a local server.

<script src="https://gist.github.com/damienstamates/823aede42638b07067a610d90f5ef781.js"></script>

Once you have all this written try running it on your machine! Just direct yourself to your file in your terminal and run this command.

```sh
go run main.go
```

After the client gets instantiated then you may begin traversing *(querying)* the Gremlin server via the `.ExecuteQuery()` and the `.ExecuteBoundQuery()` method in the client. To use this function you must put in a `Query` which is an interface for any kind of Query-able type in the package. These include: `graph.String` and `traversal.String`. To get one of these objects you must use one of these functions in the main package: `Traversal`, `GraphTraversal`, and `CustomTraversal`. For an example of traversing *(querying)* the Gremlin server for all the vertex labels:

<script src="https://gist.github.com/damienstamates/4f5c53b2868f15dd56d9f56fab67547a.js"></script>

Last but not least, Grammes, allows for the ability to interact directly to the graph using outputted Vertex structures. When using one of the functions for adding or getting vertices from the graph these return Vertex structures. With these structures, we are allowed to alter the properties and get other vertices from the outgoing edges without querying the server directly.

For more examples look in the `examples/` directory in the root of the Grammes repository. In there you’ll find *multiple examples* on how to use the API in-depth.

---

![Grammes Gopher and Gremlin Hugging](/images/blog/grammes-gopher-gremlin-hug.png)

#### Additional Resources

##### Documentation on Gremlin

To learn more about how to use Gremlin I highly recommend looking through their [TinkerPop3 Documentation](http://tinkerpop.apache.org/docs/current/reference/).
It’s full of examples and descriptions on every traversal step available and more.

##### Documentation on JanusGraph®

JanusGraph® is one of the main contenders of graph databases that Grammes was built on using.
They have plenty of resources on their website on how to use JanusGraph® and Gremlin [here](https://docs.janusgraph.org/latest/).

##### Examples

To find examples look in the `examples/` directory in Grammes.
This directory is located in the root of the repository.
In there you’ll find plenty of examples related to this package in-depth.
_Make sure you’re running JanusGraph® before you begin running any of the examples._

---

#### Troubleshooting

##### Fixing timeouts when starting JanusGraph®

If [Nodetool](http://cassandra.apache.org/download/) times out or any other part of the setup times out then the most common issue is that Cassandra® is already running on your machine. To fix this please run this command.

```sh
# only on Unix at the moment.
launchctl unload ~/Library/LaunchAgents/homebrew.mxcl.cassandra.plist
```
