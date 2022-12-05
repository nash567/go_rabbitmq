# Topic Exchange
<p>
Topic exchanges route messages to queues based on wildcard matches between the routing key and the routing pattern, which is specified by the queue binding. Messages are routed to one or many queues based on a matching between a message routing key and this pattern.

The routing key must be a list of words, delimited by a period (.). **Examples are agreements.us and agreements.eu.stockholm**  which in this case identifies agreements that are set up for a company with offices in lots of different locations. The routing patterns may contain an asterisk (“\*”) to match a word in a specific position of the routing key **(e.g., a routing pattern of "agreements.\*.\*.b.\*"** only match routing keys where the first word is "agreements" and the fourth word is "b"). A pound symbol (“#”) indicates a match of zero or more words **(e.g., a routing pattern of "agreements.eu.berlin.#" matches any routing keys beginning with "agreements.eu.berlin").**

The consumers indicate which topics they are interested in (like subscribing to a feed for an individual tag). The consumer creates a queue and sets up a binding with a given routing pattern to the exchange. All messages with a routing key that match the routing pattern are routed to the queue and stay there until the consumer consumes the message.
</p>

![alt topic](https://www.cloudamqp.com/img/blog/topic-exchange.png)

<p>
The binding key must also be in the same form. The logic behind the topic exchange is similar to a direct one - a message sent with a particular routing key will be delivered to all the queues that are bound with a matching binding key. However there are two important special cases for binding keys:

    * (star) can substitute for exactly one word.
    # (hash) can substitute for zero or more words.

</p>

<p>
We're going to use a topic exchange in our logging system. We'll start off with a working assumption that the routing keys of logs will have two words:
</p>

---

>Topic exchange is powerful and can behave like other exchanges.

>When a queue is bound with "#" (hash) binding key - it will receive all the messages, regardless of the routing key - like in fanout exchange.

>When special characters "*" (star) and "#" (hash) aren't used in bindings, the topic exchange will behave just like a direct one.


- To receive all the logs:
    ```
    go run receive_log_topic.go "#"
    ```
- To receive all logs from the facility "kern":
    ```
    go run receive_logs_topic.go "kern.*"  
    ```
- Or if you want to hear only about "critical" logs:
    ```
    go run receive_logs_topic.go "*.critical"
    ```    
- You can create multiple bindings:
    ```
    go run receive_logs_topic.go "kern.*" "*.critical"
    ```    
- And to emit a log with a routing key "kern.critical" type:
    ```
    go run emit_log_topic.go "kern.critical" "A critical kernel error"
    ```    