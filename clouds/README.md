## Declaration
This system is an example of using tcpx.It advises but not force users to design your system like this.

## Distribute Design
**中文版**:
<p align="center">
    <a href="https://user-images.githubusercontent.com/36189053/67370908-1eb7fe00-f5ae-11e9-8d7a-69e1075afbfe.png"><img src="https://user-images.githubusercontent.com/36189053/67370908-1eb7fe00-f5ae-11e9-8d7a-69e1075afbfe.png"></a>
</p>
**English**:
<p align="center">
    <a href="https://user-images.githubusercontent.com/36189053/67372543-a56dda80-f5b0-11e9-915f-2abea11f39c2.png"><img src="https://user-images.githubusercontent.com/36189053/67372543-a56dda80-f5b0-11e9-915f-2abea11f39c2.png"></a>
</p>

#### Explain
- **Active line with arrow** means clients straightly request to a server broker.
- **Dotted line with arrow** means interfering among inner server brokers.
- **Big square frame** means sub-clouds with brokers

#### Advantages
- Scalable.Each broker/sub-cloud can be easily horizontally scale out to balance big amount of requests.
- Strong availability. Certain broker fail does not influence cloud running.
- Service extensible. Can harmlessly extend services.

## Server brokers
Server are divided into brokers: center, register, userPool. They works below.

#### Center
Center handles all events from clients.It can not only scale out horizontally based on a certain event, but also scale out for varies events.

To interfere with user.It will grasp userInfo from pool and thus grasp which pool this user is in. Then It will build a connection(called bridge) to the specifc pool.

- Receiving message from client.
- Connect to specific userPool for further operaion.

#### Register
Register works for registering user info and pool info.It also works as a redis proxy.All interfering actions with redis can be handled here.

- Storage and provide user login info.
- Storage and provide pool info.

#### UserPool
UserPool can scale out without number limit. Once a user client send online to a user-pool broker, then this user will be joint with it.

- Save user connections

**Most Important Point:**
All server brokers can easily scale out without side effect.

## Broker doc
MessageID for different brokers are designed first. For broker userPool, messageID ranges 1-100. For center broker, messageID ranges 101-200.

