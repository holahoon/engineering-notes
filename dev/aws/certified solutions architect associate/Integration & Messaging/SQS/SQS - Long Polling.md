
- When a consumer requests messages from the queue, it can optionally "wait" for messages to arrive if there are non in the queue
- This is called **Long Polling**
- **LongPolling decreases the number of API calls made to SQS while increasing the efficiency and latency of your application**
- The wait time can be between 1 sec to 20sec (20 sec preferable)
- Long Polling is preferable to Short Polling
- Long Polling can be enabled at the queue level or at the API level using **WaitTimeSeconds**

![[Pasted image 20240222203643.png]]
