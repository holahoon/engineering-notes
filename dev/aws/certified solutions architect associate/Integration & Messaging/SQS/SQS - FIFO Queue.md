
- FIFIO = First In First Out (ordering of messages in the queue)
![[Pasted image 20240222212904.png]]
- Limited throughput: 300 msg/s without batching, 3000 msg/s with batching
- Exactly-once send capability (be removing duplicates)
- Messages are processed in order by the consumer

✏️ Note that you have to end the name of your queue with `.fifo`

![[Pasted image 20240222213159.png]]



