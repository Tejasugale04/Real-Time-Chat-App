# Realtime Chat Application using Spring Boot

## Introduction
We build a realtime one-to-one chat application using Spring Boot and WebSocket. The application allows user to join, chat, and leave chat room in real-time. Spring Boot provides a robust and scalable architecture for the application, while WebSocket enables real-time communication between the server and clients.  

## Key Features: 
### -  One-to-One Chat:
Connect with others in a personalized environment, fostering direct and meaningful conversations.
### -  Secure Communication:
Prioritizing user security, our application ensures that all interactions are protected, guaranteeing a safe and private chat experience. 
### -  Persistent Chat:
Enjoy the convenience of persistent conversations, allowing you to pick up right where you left off whenever you return to the chat room.

## Technologies Utilized: 
### -  WebSocket: 
The backbone of our real-time communication, WebSocket enables instant data exchange between the server and clients, ensuring a smooth and responsive chat interface. 
### -  Spring Boot:
Providing a solid and scalable foundation, Spring Boot powers the backend architecture, enhancing the overall performance of our chat application. 
### -  MongoDB:
Leveraging the capabilities of MongoDB for data storage, our application ensures efficient and reliable management of chat histories. 
### -  JavaScript, HTML, CSS:
The dynamic trio of web technologies come together to create an intuitive and visually appealing user interface, making the chat experience both functional and aesthetically pleasing.

## The flow of the application typically involves the following steps:
### 1.	User Authentication:
•	Users access the application and are authenticated using their credentials.  
•	Upon successful authentication, users are directed to the main chat interface.
### 2.	Joining a Chat Room:
•	Users have the option to join an existing chat room or create a new one.  
•	Upon joining a chat room, users can view the list of participants and previous chat messages.
### 3.	Real-Time Messaging:
•	Users can send and receive messages in real-time within the chat room.  
•	As users type and send messages, they are instantly displayed to other participants in the chat room.
### 4.	Interaction and Engagement:
•	Users can engage in one-to-one conversations or group discussions with other participants.  
•	They can react to messages, share media files, and use emojis to express themselves.
### 5.	Leaving the Chat Room:
•	Users have the option to leave the chat room at any time.  
•	Upon leaving, they can choose to save or discard the chat history, depending on the application's settings.
### 6.	Logging Out:
•	Users can log out of the application when they're done chatting.  
•	Upon logging out, their session is terminated, and they're redirected to the login page for future access.
Overall, the flow of the application revolves around seamless and interactive real-time communication between users within chat rooms, facilitated by WebSocket technology.

## Architecture: 
The architecture follows a client-server model. Spring Boot powers the backend server, handling business logic, data storage, and communication with clients. WebSocket technology enables real-time bidirectional communication between the server and clients, ensuring instant message delivery. On the frontend, JavaScript, HTML, and CSS are used to create an intuitive and visually appealing user interface.

## 	Outline of Modules:
### •	Chatroom Management:
Allows users to create, join, and leave chatrooms.  
### •	Real-time Messaging:
Facilitates instant messaging between users within chatrooms.  
### •	User Authentication:
Provides secure authentication mechanisms for user identification.  
### •	Data Storage:
Stores chat messages, user details, and chatroom information in the database.  

## Types of Users Involved:
There are primarily two types of users involved:
### •	Regular Users:
Individuals who join chatrooms, send messages, and interact with other users.
### •	Administrators: 
Responsible for managing chatrooms, monitoring user activity, and enforcing chatroom rules.

## 	Number of Collections:
In the MongoDB database, there are four collections used:
### •	ChatMessage:
Stores chat messages exchanged between users.
### •	ChatRoom:
Stores information about chatrooms, such as room name and participants.
### •	ChatNotification:
Handles notifications related to chat activities.
### •	User: 
Stores user details, including username and authentication credentials.



## Websocket
WebSocket is a computer communications protocol, providing full-duplex communication channels over a single TCP connection. WebSocket is distinct from HTTP.

The protocol enables interaction between a web browser (or other client application) and a web server with lower overhead than half-duplex alternatives such as HTTP polling, facilitating real-time data transfer from and to the server.

Once a websocket connection is established between a client and a server, both can exchange information until the connection is closed by any of the parties.

This is the main reason why WebSocket is preferred over the HTTP protocol when building a chat-like communication service that operates at high frequencies with low latency.

### Stomp JS
Simple (or Streaming) Text Oriented Message Protocol (STOMP), formerly known as TTMP, is a simple text-based protocol, designed for working with message-oriented middleware (MOM). It provides an interoperable wire format that allows STOMP clients to talk with any message broker supporting the protocol.

Since websocket is just a communication protocol, it doesn't know how to send a message to a particular user. STOMP is basically a messaging protocol which is useful for these functionalities.

  ![image](https://github.com/sandesh300/Real-Time-Chat-App/assets/92014891/a3368aad-fb65-4684-a167-79b54e7025ed)
  ![image](https://github.com/sandesh300/Real-Time-Chat-App/assets/92014891/dbbdbbd5-5a83-4af7-8fe4-7b1204a4e5c7)

## WebSocket Config
@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {

    @Override
    public void configureMessageBroker(MessageBrokerRegistry registry) {
        registry.enableSimpleBroker("/user");
        registry.setApplicationDestinationPrefixes("/app");
        registry.setUserDestinationPrefix("/user");
    }

    @Override
    public void registerStompEndpoints(StompEndpointRegistry registry) {
        registry.addEndpoint("/ws")
                .withSockJS();
    }

    @Override
    public boolean configureMessageConverters(List<MessageConverter> messageConverters) {
        DefaultContentTypeResolver resolver = new DefaultContentTypeResolver();
        resolver.setDefaultMimeType(MimeTypeUtils.APPLICATION_JSON);
        MappingJackson2MessageConverter converter = new MappingJackson2MessageConverter();
        converter.setObjectMapper(new ObjectMapper());
        converter.setContentTypeResolver(resolver);
        messageConverters.add(converter);
        return false;
    }
}

### docker-compose.yml

```yaml
services:
  mongodb:
    image: mongo
    container_name: mongo_db
    ports:
      - 27017:27017
    volumes:
      - mongo:/data
    environment:
      - MONGO_INITDB_ROOT_USERNAME=sandy
      - MONGO_INITDB_ROOT_PASSWORD=sandy

  mongo-express:
    image: mongo-express
    container_name: mongo_express
    restart: always
    ports:
      - 8081:8081
    environment:
      - ME_CONFIG_MONGODB_ADMINUSERNAME=sandy
      - ME_CONFIG_MONGODB_ADMINPASSWORD=sandy
      - ME_CONFIG_MONGODB_SERVER=mongodb

volumes:
  mongo: {}


