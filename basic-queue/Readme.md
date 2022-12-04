# basic queue

## It is a basic implementation of rabbitmq Queue here we only have a sender and a reciever

---
### Steps followed 
- Reciever

        1. Open a connection and a channel,
	    2. Declare the queue from which we're going to consume. Note this matches up with the queue that send publishes to.
        3. Range over the channel and process the messages

- Sender

        1.  Connect to RabbitMQ server
	    2.  Create a channel, which is where most of the API  for getting things done resides:
	    3.  To send, we must declare a queue for us to send to; then we can publish a message to the queue:
        4.  Publish to that queue using the channel

#### STEPS TO RUN
open a terminal and setup reciever to pool over queue for messages
```
go run reciever.go
```        
open another terminal and setup sender to send new messages to the queue

```
go run send.go
```