# INTRO 
Web app architecture refers to the design and structure of a web application. It outlines how different components of the application interact with each other to deliver functionality to users. A well-designed architecture ensures scalability, maintainability, and performance of the web application.

### Key components of web app architecture:

- Client-side(front-end): This is the part of the application that runs on the user's device (browser). It handles user interaction, rendering the user interface, and sending requests to the server.
- Server-side(back-end): This is the part of the application that runs on a server. It processes requests from the client, retrieves data from databases, and sends responses back to the client.
- Database: This stores data used by the application. It can be a relational database (like MySQL or PostgreSQL) or a NoSQL database (like MongoDB or Cassandra).
- Application logic: This is the code that implements the business logic of the application. It handles tasks like data validation, calculations, and decision-making.
- APIs: These are interfaces that allow different components of the application to communicate with each other.
- Network: This is the infrastructure that connects the client and server.

# WEB APP ARCHITECHURES
## The Client-Server Model
The client-server model is a common method of implementing distributed applications. It is a fundamental architectural pattern in distributed computing systems. It divides the workload between two primary components:   

- Clients: These are entities that request services from the server. They typically represent users or applications.   
- Servers: These are entities that provide services to clients. They manage resources and handle requests.

## One-, Two-, and Three-Tier Architectures
A tier in a software architecture refers to a distinct layer or level of functionality within the overall system. Each tier handles specific tasks and interacts with other tiers through well-defined interfaces.

In web application architectures, common tiers include:

- Presentation layer: Handles user interaction, input, and output (e.g., web browser, mobile app).
- Business logic layer: Contains the application's core functionality, business rules, and data processing.
- Data access layer: Interacts with the database to store and retrieve data.
  
### One-Tier Architecture:

Description: In this simple model, all components of the application, including the presentation layer, business logic layer, and data access layer, reside on a single machine.<br>
Advantages: Easy to implement and manage.<br>
Disadvantages: Limited scalability and potential performance issues as the application grows.<br>

### Two-Tier Architecture:

Description: This model separates the presentation layer (typically the client-side) from the application and data access layers (server-side).<br>
Advantages: Improved scalability and maintainability compared to one-tier.<br>
Disadvantages: Can become complex for large-scale applications.<br>

### Three-Tier Architecture:

Description: This model further divides the application layer into two separate tiers: the business logic layer and the data access layer. This provides a more modular and flexible structure.<br>
Advantages: Enhanced scalability, maintainability, and security.<br>
Disadvantages: Increased complexity and potential overhead due to network communication between tiers.<br>

### Four-Tier Architecture (and Beyond):

Description: While less common, four-tier architectures can be used for extremely complex applications. They introduce an additional layer, often for middleware services or load balancing.<br>
Examples: A four-tier architecture might include a separate layer for caching or message queuing.<br>
Advantages: Can provide additional flexibility and performance benefits for specific use cases.<br>
Disadvantages: Increased complexity and overhead.<br>

## Peer-to-Peer Architecture
Peer-to-Peer (P2P) architecture is a distributed computing model where nodes (peers) communicate directly with each other, without the need for a central server. Each peer can act as both a client and a server, sharing resources and services with other peers in the network.

Key characteristics of P2P networks:

-Decentralization: No single entity controls the network.
-Distributed resources: Resources are shared across multiple peers.
-Scalability: P2P networks can scale to accommodate a large number of peers.
-Resilience: The network is more resilient to failures, as there is no single point of failure.

## Distributed Computing Architectures

### Monolithic Architecture
A monolithic architecture is a traditional approach where all components of an application are tightly coupled and deployed as a single unit. This simplifies development and deployment, as there's a single codebase to manage. However, as the application grows, it can become increasingly difficult to scale, maintain, and update. Changes to any part of the application can potentially affect the entire system, leading to increased complexity and risk.
* **Advantages:** Simple to develop and deploy, easier to test and debug.
* **Disadvantages:** Difficult to scale, maintain, and update; prone to technical debt.

### Microservices Architecture
In contrast, microservices architecture breaks down an application into smaller, independent services that communicate through well-defined APIs. Each service focuses on a specific business capability and can be developed, deployed, and scaled independently. This modular approach offers several advantages, including improved scalability, flexibility, and resilience. Changes to one service are less likely to impact others, and individual services can be scaled to meet specific demands. However, microservices also introduce additional complexity, as developers must manage multiple services and coordinate their interactions.
* **Advantages:** Scalability, flexibility, resilience, independent development and deployment.
* **Disadvantages:** Increased complexity, potential for network overhead, and coordination challenges.

### Serverless Architecture
Serverless architecture is a relatively new approach that eliminates the need for developers to manage servers. Instead, the cloud provider handles the infrastructure, automatically scaling resources based on demand. This can significantly reduce operational overhead and costs. However, serverless architectures can also introduce limitations, such as vendor lock-in and potential for cold starts (when a function is invoked for the first time after a period of inactivity).
* **Advantages:** Cost-effective, scalable, eliminates server management overhead.
* **Disadvantages:** Vendor lock-in, potential for cold starts, and limitations on resource control.

**Key Differences:**

| Feature | Monolithic | Microservices | Serverless |
|---|---|---|---|
| Deployment | Single unit | Multiple services | Function-level |
| Coupling | Tight | Loose | Loose |
| Scalability | Challenging | Granular | Automatic |
| Maintainability | Difficult | Easier | Easier |
| Complexity | Lower | Higher | Medium |



