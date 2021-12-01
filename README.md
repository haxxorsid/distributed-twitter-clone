## About
Distributed simulation of twitter.

## How to run

Engine:

`dotnet fsi Server.fsx`

Client:

`dotnet fsi Client.fsx <no. of users> <IP address of the server>`

Example: `dotnet fsi Client.fsx 1000 10.20.244.195`

## Dependencies

1. FSharp
2. [Akka.NET](https://www.nuget.org/packages/Akka/)
3. [MathNet.Numerics](https://www.nuget.org/packages/MathNet.Numerics/) for Zipf distribution

## What is working
-	The client processes were run on a single machine as actors and the engine process was run on a different machine (again using actor model) considering the load it has to handle. (That is, client processes and engine are separate processes)
-	Users can register, sign in, and sign out.
-	Users can follow another user.
-	Users can tweet/retweet.
-	Users can receive live updates without querying.
-	Users can view timeline (tweets/retweets of users they followed), query based on hashtags and mentions.
-	We distributed number of subscribers in a Zipf distribution and recorded results for networks with 1000 users and 10,000 users.

## Implementation of simulator
-	Maximum Number of Users simulated?
10,000 users simulated.
-	Simulated periods of live connection and disconnection for users?
Yes, done. Users are logging in and logging out frequently.
-	Simulated a Zipf distribution on the number of subscribers. For accounts with a lot of 
subscribers, increase the number of tweets. Make some of these messages re-tweets?
Done. Zipf distribution was applied for deciding number of subscribers for a particular user. 
Number of tweets/retweets were increased for users with higher number of subscribers.

## Screenshot
Sample screenshots

Engine:
![alt text](https://github.com/haxxorsid/distributed-twitter-clone/blob/main/screenshots/img1.jpg "-")
![alt text](https://github.com/haxxorsid/distributed-twitter-clone/blob/main/screenshots/img2.jpg "-")

Client:
![alt text](https://github.com/haxxorsid/distributed-twitter-clone/blob/main/screenshots/img3.jpg "-")

## Zipf Plots
Note: Zipf distribution graphs (User ID vs No. of subscribers) are truncated/zoomed for better 
visualization.

1,000 users:
![alt text](https://github.com/haxxorsid/distributed-twitter-clone/blob/main/screenshots/img4.jpg "-")

10,000 users:
![alt text](https://github.com/haxxorsid/distributed-twitter-clone/blob/main/screenshots/img5.jpg "-")

## Performance
We recorded time taken to process tweets with respect to number of followers.

The processing of a tweets includes making database entries and distributing the tweet to all live 
subscribers.

1,000 users:
![alt text](https://github.com/haxxorsid/distributed-twitter-clone/blob/main/screenshots/img6.jpg "-")

10,000 users:
![alt text](https://github.com/haxxorsid/distributed-twitter-clone/blob/main/screenshots/img7.jpg "-")

## System Statistics
For 1,000 users:

Maximum number of followers for a given user was 133 (by Zipf distribution)

|         | LiveUserCount | TimeTaken (ms) |
|---------|---------------|----------------|
| Average | 645           | 0.04           |
| Min     | 528           | 0.0069         |
| Max     | 999           | 2.69           |

For 10,000 users:

Maximum number of followers for a given user was 1021 (by Zipf distribution)

|         | LiveUserCount | TimeTaken (ms) |
|---------|---------------|----------------|
| Average | 7572          | 0.89           |
| Min     | 3688          | 0.0045         |
| Max     | 9436          | 170.72         |

## Conclusion
We found that propagation time increased as the number of followers increased, because the time to 
propagate a tweet/re-tweet to followers increase. This also means, there is more activity in the network 
when there are more followers.
