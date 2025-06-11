## What happens when you seach google.com on th web browser

- When l read the question, my mind was like this is straight to the point but taking a closer look l discovered that l only knew a spoon full and they were a lot more to what met the eye and keyboard when l typed something into the search engine.

1. **DNS request**

- DNS (Domain Name System request) this is an inquiry made to a database that maintains the name of the website Unique Resource Locator (URL) and matches it to a specific IP address.
- So, when l type [https://www.google.com](https://www.google.com) in the browser the DNS matches the website to its unique IP address and then directs the user to the website with that IP Address. A DNS is a phonebook with names and their numbers but in this case, being websites and their IP Addresses.

2. **TCP/IP**

- TCP/IP stands for Transmission Control Protocol/Internet Protocol and this is the protocol that is used by the internet.
- These are a set of rules that define how clients (any internet user and their computer connected to the network making a request) and servers (the computer that stores web server software and a website’s component files) interact over a network which in this instance is the internet, and how data must be moved while broken down into packets (this is a small segment of a larger message)

3. **Firewall**

- To safeguard one’s self from hackers and unauthorized network accessors servers are mostly equipped with a firewall
- By definition, a firewall is a software that sets out rules about what can pass or leave a network, and everything that goes in and out of the servers goes through the firewall.
- In the case of our example 208.65.192.64, this will have to be processed by our firewall to check if it is safe or not then proceed according to the verdict from the firewall by either blocking the site or loading it.

4. **HTTPS/SSL**

- HTTPS stands for hypertext Transfer Protocol secure, different from its counterpart HTTP which is just Hypertext Transfer Protocol without the security aspect.
- This defines how different types of requests and responses are served to clients over a network and is also used by browsers to check the authenticity of a website before loading it.
- It handles all the requests via GET, PUT, POST, etc, and encrypts the data ensuring that the users’ data won’t be accessed without the right credentials

5. **Load-balancer**

- This is software that is installed on multiple servers in a mesh of servers to do resource allocation and balancing by making sure no server is being overwhelmed while others are free and running idle.
- The mesh server model was introduced to combat single server which created risks of Single Point of Failure (SPOF) points as one server will be the core of the entire network.
- HAproxy is a very famous example of a load-balancing software that employs the round-robin algorithm which distributes requests around different servers in the network evenly and consequentially.

6. **Web Server**

- After a load-balancer allocates a request to a specific server for processing, it will then find where the static content relating to the address requested is kept for an HTTPS response.
- By definition a web-server is a software program which stores static content like HTML pages, images and text files. Examples include Nginx and Apache

7. **Application sever**

- Having a web server is fundamental for any web page. But most sites don’t just need static pages where no interaction is happening such most websites are dynamic.
- That means that it’s possible to interact with the site, save the information on it, and log in with a username and a password

8. **Database**

- The last step in our web infrastructure is the Data Base Management System (DBMS).
- A database is a collection of data, and the DBMS is the program that is going to interact with the database by retrieving, adding, and modifying data in it.
- There are several types of database models. The two main ones are relational databases and non-relational databases. A relational database can be seen as a collection of tables representing objects, where each column is an attribute and each row is an instance of that object.
- We can perform SQL (Structured Query Language) queries on those databases. MySQL and PostgreSQL are two popular relational databases.
- A non-relational database can have many forms, as the data inserted in it doesn’t have to follow a particular schema. Here we can take NoSQL (MongoDB).
