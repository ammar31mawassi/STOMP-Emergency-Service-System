SPL Assignment 3 — STOMP Emergency Service System

This project implements a distributed emergency reporting platform based on the STOMP (Simple Text-Oriented Messaging Protocol).
It includes:

Java STOMP Server supporting both Thread-Per-Client (TPC) and Reactor patterns

C++ multithreaded client for subscribing to emergency channels, reporting events, and generating summaries

JSON-based event parsing and standardized STOMP messaging

The system allows users to join emergency channels (fire, police, medical), publish event reports, and receive real-time updates.

🛠️ Features
Server (Java)

Implements full STOMP frame processing

Supports:

CONNECT / CONNECTED

SEND

SUBSCRIBE / UNSUBSCRIBE

DISCONNECT

Two operation modes:

TPC — dedicated thread per client

Reactor — non-blocking event-driven model

Manages:

Active clients & connection IDs

Topic-based subscriptions

Broadcast messaging

Error handling via STOMP ERROR frames

Built using Maven

Client (C++):

Multithreaded:

One thread reads stdin

One thread listens to server messages

Sends and parses STOMP frames

Reads events from JSON and reports them as STOMP messages

Supports local storage of received reports for summary generation

📂 Project Structure
server/
  ├── src/
  ├── pom.xml
  └── target/

client/
  ├── src/
  ├── include/
  ├── bin/
  └── makefile

🚀 Running the Server
Compile
cd server
mvn compile

Run
mvn exec:java -Dexec.mainClass="bgu.spl.net.impl.stomp.StompServer" -Dexec.args="7777 tpc"
# OR
mvn exec:java -Dexec.mainClass="bgu.spl.net.impl.stomp.StompServer" -Dexec.args="7777 reactor"

💻 Running the Client
Compile
cd client
make

Run
./bin/StompEMIClient

📌 Example Client Commands
Command	Action
login 1.1.1.1:2000 user pass	Connect to server
join fire_dept	Subscribe to a channel
exit fire_dept	Unsubscribe
report events.json	Push event list to server
summary fire_dept user out.txt	Generate summary report
logout	Disconnect gracefully
📨 Sample STOMP Frame Sent by Client
SEND
destination:/police
user:amar
city:Liberty City
event name:Burglary
date time:1773279900

Suspect fled via Oak Street...
^@

📘 Key Concepts

STOMP 1.2 protocol formatting & framing

Reactor vs Thread-per-Client networking models

TCP messaging, framing, and header parsing

Multithreaded clients via std::thread + mutex

JSON event ingestion & persistence

🧪 Testing

Server validation:

java -jar StompServer.jar 7777 tpc


Client validation:

./bin/StompEMIClient

🏗 Technologies
Component	Language	Tools
Server	Java	Maven
Client	C++	make, threads, mutex
Protocol	STOMP 1.2	TCP networking
Events	JSON	provided parser