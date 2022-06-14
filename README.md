##High Level Approach
In this project, we implemented the Raft protocal for the key-value store database. The central idea of Raft is to have an understanding information changing way between servers and clients, members and leaders. A Normal Raft include two parts: Voting and Log updating.

The program starts by the hello messages between the server and clients. The clients will then send put and get request to servers in our system. If the currentleader is still unknown("FFFF"). Each server will then save up the commands to their own queue and will redirect them to the leader every time they receive the appenEntriesRPC from the leader, which means that the new leader is elected.

A common election will start if any of the server didn't receive a heartbeat from the leader in his timeout time. He will send a request vote messages to all servers in this system. The servers who received such messages will check the term and index of the sender and will decide if he want to vote for him. The server who receives the most votes will become the leader and he will then send an appendEntriesRPC to let all servers become followers. The new round starts from there.

In each appendRPC the leader sends to the server, he will also include his prevLogIndex and prevLogTerm so that the server can check if the leader is stale and if he wants the log entries that are further before. Once the server receive the log which starts at the position he wants, he will then cut off all the entries after that and append the log from the user to its own log. A log will only be committed if majority of the servers have that log. If the entry is committed, the command will then be excuted.

# Challenge we face:
We faced many challenges when we are writing this assignment. The main issue is to understand how election and log updating works. It took us some time to figure out how to reject stale messages from leaders in other partitions. Also, we got few points off for the advanced test becuase the server will send a fail message to the sender if it's coming from a server in other partition. It's what the instructor suggests us to do, but will make lose a few points for sending unnessary fail messages.

# Testing
We bacially run our test using the test files provided. We also printted things out to see what messages are send and what are received
