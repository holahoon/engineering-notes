
- After a message is polled by a consumer, it becomes **invisible** to other consumers
- Be default, the "message visibility timeout" is **30 seconds**
- That means the message has 30 seconds to be processed
- After the message visibility timeout is over, the message is "visible" in SQS
![[Pasted image 20240222202407.png]]

- If a message is not processed within the visibility timeout, it will be processed **twice**
- A consumer could call the **ChangeMessageVisibility** API to get more time
- If visibility timeout is high (hours), and consumer crashes, re-processing will take time
- If visibility timeout is too low (seconds)
![[Pasted image 20240222202640.png]]


