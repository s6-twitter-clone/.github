# Twiddit

Twiddit is a simple twitter clone for semester 6.
The main focus for this semester is cloud native architectures and scalability.
Because of these factors the project will have a microservice architecture.

## Scope

This project will focus on replicating the basic features of twitter.
Below you will find the user stories for each actor.

**Users can:**
- [ ] Send posts
- [ ] Follow users
- [ ] Like posts
- [ ] Remove  (their own) posts
- [ ] Block users
- [ ]  Search posts by topic/tag
- [ ]  View the top ten trending topics/trends


**Admins can:**
- [ ] remove/archive user accounts
- [ ] assing moderators

**Moderators can:**
- [ ] remove posts


## Architecture
Twiddit is built using a microservice architecture and uses the [Event-Carried State Transfer](https://martinfowler.com/articles/201701-event-driven.html) pattern. 

This means that each service stores a copy of the data they need. Date is owned by a single service and data can only be updated by the service that owns it. When a piece of data changes an event is sent containing the new data.
Services can subscribe to specific events and update their internal copy of data when an event comes in.

Below you can see the C2 diagram for Twiddit which showcases the different services.
![architecture](https://github.com/s6-twitter-clone/documentation/blob/master/images/diagrams/architecture.png)

## Structure
The project consists of the following repositories.
```
├── documentation
├── post-service           
├── user-service            
├── feed-service			
├── follow-service			
├── frontend					
└── trend-service
```

### documentation
This directory contains a general readme and documents for the project.
Here you will also be able to find the architecture diagrams and wireframes for the project.

### post-service
This repository will host the post service.
The post service is used to create and retrieve posts. As such the post service *owns* the post data. Only the post service can send events to update post data.

[Post service](https://github.com/s6-twitter-clone/post-service/)

### user-service
This repository contains the user service which will be in charge of authorization.
The user service will also be in charge of managing users

[User service](https://github.com/s6-twitter-clone/user-service/)

### feed-service
The Feed service is in charge of creating personalized timelines for users based on their own posts and the posts of the users they follow. The Feed service itself does not own any data. To create timelines it consumes events from the post service, the user service and the follower service. 

[User service](https://github.com/s6-twitter-clone/feed-service/)

### follower-service
The follower service is in charge of keeping track of followers and blocked users.
when a user follows another user, they get their posts in their content feed.
Blocking a user prevents them from seeing your content.

[Follower service](https://github.com/s6-twitter-clone/follower-service/)

### frontend
The frontend repository contains the frontend for Twiddit.
Twiddit uses React as JS framework and Bootstrap for styling. 
To serve the production build of the frontend Nginx is used as a web server.

[Frontend](https://github.com/s6-twitter-clone/frontend/)

### trend service
The trend service keeps track of the currently most popular subjects on the platform.
To do this it consumes the events from the post service and periodically count the number of occurrences of the tags within the posts. The top ten results will then be stored

[Trend service](https://github.com/s6-twitter-clone/trend-service/)