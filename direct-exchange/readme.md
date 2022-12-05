# Direct exchange
<p>
Our logging system from the previous tutorial broadcasts all messages to all consumers. We want to extend that to allow filtering messages based on their severity. For example we may want the script which is writing log messages to the disk to only receive critical errors, and not waste disk space on warning or info log messages.

We were using a fanout exchange, which doesn't give us much flexibility - it's only capable of mindless broadcasting.

We will use a direct exchange instead. The routing algorithm behind a direct exchange is simple - **a message goes to the queues whose binding key exactly matches the routing key of the message**.



</p>

![alt direct binding](https://www.rabbitmq.com/img/tutorials/python-four.png)


- If you want to save only 'warning' and 'error' (and not 'info') log messages to a file, just open a console and type:
    ```
    go run receive_logs_direct.go warning error > logs_from_rabbit.log
    ```
- If you'd like to see all the log messages on your screen, open a new terminal and do:
    ```
    go run receive_logs_direct.go info warning error
    ```    
- to emit an error log message just type:
    ```
    go run emit_log_direct.go error "Run. Run. Or it will explode."
    ```    