# System Design Introduction

## Distributed System(DS)

1. What is a DS?
- Group of computers work together
- Abstract complexity from end user

2. Fallacies of DS

  a. Network is reliable
  b. Latency is zero
  c. Bandwidth is inginite
  d. Topology doesn't change
  e. Network is secure
  f. Only one administrator
  g. Data transport cost is zero

3. Common characteristics of a DS

  a. No shared clock
  b. No shared memory
  c. Shared resources
  d. Concurrency and consistency

4. Communication in DS

  a. Different parts of a DS needs to be able to talk
  b. Requires agreed upon format or protocol
  c. Lots of things can go wrong, need to handle them somehow
    1. Client can't find server
    2. Server crash mid request
    3. Server response is lost
    4. Client creashes

5. Benifits 
  a. More reliable, fault tolerant
  b. Scalability
  c. Lower latency, increased performance
  d. Cost effective

## System Design Performance Metrics

1. Scalability
  
  a. Ability of a system to grow and manage increased traffic
  b. Increased volume of data or requests
  * Bad system design could result in - block(limit), increased cost 

2. Reliability

  a. Probability a system could fail during a period of time
  b. Slighty harder to define than hardware reliability
  c. So, we need systems in place to test to stop bugs beign deployed to prod, to compansate hardware failures
  d. Measure Reliability:
    1. Mean time between failure(MTBF)
      MTBF = (Total Elapsed Time for period - Total downtime) / number of failures
           = (24 hrs - 4 hrs downtime) / 4 failures
           = 5 hrs MTBF
    * Means that this system have 5 hours on average before your system will have a failure

3. Availability

  a. Ammount of time a system is operational during a period of time
  b. Poorly designed software requiring downtime for updates is less available
  Ex: Database update
  c. Availability calculation:
    Availability % = (available time/total time) X 100
                   = (23 hrs / 24 hrs) X 100
                   = 95.83%
  d. Show table of downtime for 9's
    |Availability |Annual Downtime          |
    |-------------|-------------------------|
    | 99%         | 3 days, 15 hrs, 40 min  |
    | 99.9%       | 8 hrs, 46 mins          |
    | 99.99%      | 52 hrs, 36 seconds      |
    | 99.999%     | 5.26 minutes            |
    |-------------|-------------------------|

4. Reliability vs Availability

  1. Reliable system is always an available system
  2. Availability can maintained by redundancy, but system may not be reliable
  3. Reliable software will be more profitable because prodiding same service requires less backup resoruces
  4. Requirement will depend on function of the software 
    Ex: Social media site vs AWS Cloud / a plan

5. Efficiency:

  a. How well the system performs
  b. Latency & throughput often used as metrics(how much requests it could handle)

6. Managebility

  a. Speed and difficulty involved with maintaining system
  b. Observerbility, how hard to track bugs
  c. Difficulty of deploying updates
  d. Want to abstract away infastructure so product engineers don't have to worry about it

## Numbers Programmers should know

1. Latancy Numbers

  a. 

| Access Type         | Time              | Converted Time   |
| --------------------|-------------------|------------------|
| CPU Cycle           | .3ns              | 1 s              |
| CPU L1 Cache        | 1ns               | 3 s              |
| CPU L2 Cache        | 3 ns              | 9 s              |
| CPU L3 Cache        | 13 ns             | 43 s             |
| Main Memeory (RAM)  | 120 ns            | 6 minutes        |
| SSD                 | 150 micro seconds | 6 day            |
| HDD                 | 10 ms             | 12 months        |
| SF to NYC           | 40 ms             | 4 years          |
| SF to Australia     | 183 ms            | 19 years         |

  b. Key takeaways 
    1. Avoid network calls whenever possible
    2. Replicate data across data centers for disaster recovery as well as performance
    3. User CDNs to reduce latancy
    4. Keep freequently accessed data in memory if possible rather than seeking from disk, caching

2. Quick math for Capsity Estimates

  a. Data conversions
    
    1. 8 bits ----------------> 1 byte
    2. 1024 bytes ------------> 1 kilobyte
    3. 1024 kilobyte ---------> 1 Megabyte
    4. 1024 Megabyte ---------> 1 Gigabyte
    5. 1024 Gigabytes --------> 1 Terabyte
  
  b. Common data types

    1. Char - 1 byte
    2. Integer - 4 bytes
    3. UNIX Timestamp - 4 bytes

  c. Time

    1. 60 seconds X 60 minutes = 3600 seconds per hour
    2. 360 X 24 hours = 86400 seconds per day
    3. 86400 X 30 days = 2500000 seconds per month
   
  d. Time Estimates

    1. Estimate total number of requests app will receive
    2. Average Daily Active Users X Average Reads/Writes oer user

    Ex: 
    10 Million DAU X 30 photo requests = 300 Million photo requests per day
    10 Million DAU X 1 phote upload = 10 Million photo writes

    300 Million Requests / 86400 = 3472 requests per second
    10 Million Writes / 86400 = 115 writes per second

    * Number of external calls are not always equal to the sum of internal calls

  e. Memory Estimates

    1. Read requests per day X Average Request Size X 0.2  
    * 80 20 rule (most popular 20% of all posts, that needs to be cache)

    300 Million Requests X 500 Bytes = 150 Gigabytes
    150 GB X 20% = 30 Gigabytes <-- Cache storage
    30 GB X 3(replication) = 90 Gigabytes <-- Cache replication

  f. Bandwidth requirements

    1. Requests per day X Request Size

    300 Million Requests X 1.5 MB = 450000 Gigabytes
    450000 GB / 86499 seconds = 5.2 GB per second
  
  g. Storage requirements

    1. Writes per day X Size of write X Time to store data

    Ex:
    10 Million writes X 1.5 MB = 15 TB per day
    15 TB X 365 days X 10 years = 55 Petabytes

3. Horizontal vs Vertical Scaling
  
  a. Number of users increase > number of requests increase > Limitation in
    
    1. CPU - Certain function that require lot of processing power  
    2. Memory - If it needs to hold large amount of data in memory
    3. I/O - how fast a application can read from storage
    4. Bandwidth - amount of data you can push to the network

  b. Vertical scaling

    1. Easiest way to scale an Application
    2. Diminishing returns, limits to scalability
    3. Single point of failure 

  c. Horizontal scaling

    1. More complexity up front, but more efficient long term
    2. Redundancy build in
    3. Need load balancer to distribute traffic
    4. Cloud provider make this easier

  d. Trade offs

    1. Diminishing returns in VS > spend more on resources > no performance improvement
    2. Latency > less in HS with distributed servers around the world

4. System Design Components

  a. Load Balancers

    1. Balance incoming traffic to multiple servers
    2. Software based - Nginx, HAProxy
      a. can deploy on any server
    3. Hardware based - F5, Citirix 
      a. Can handle large amount of traffic
      b. Tends to vendor locking
      c. Costly
    4. Used to improve reliability and scalability of application
    5. Load balancer routing methods
      a. Round robin
        1. Simplest type of routing
        2. Can results in uneven traffic
      b. Least connections
        1. Routes based on number of client connections to server
        2. Userful for chat or streaming applications
      c. Least response time
        1. Routes based on how quickly servers responds
      d. IP Hash
        1. Routes client to server based on IP
        2. Useful for stateful sessions
    6. 2 main types L4 vs L7
      a. Layer 4
        1. Only has access to TCP and UDP data
        2. Faster
        3. Lack of information can lead to uneven traffic
      b. Layer 7
        1. Full access to HTTP protocol and data*
        2. SSL termination
        3. Check for authentication
        4. Smarter routing option / CPU intensive
  
  b. Caching

    Client <---> App Server <---> Cache <---> Database

    1. Why caching? Benifits?
      a. Improve performance of application
      b. Save money long term

    2. Speed and performance 
      a. Reading from memory is much faster than disk, 50-200 X faster
      b. Can server the same amount of traffic with fewer resources
      c. Pre-calculate and cache data
      d. Most apps have far more reads than writes, perfect for caching

    3. Caching Layers
      a. DNS
      b. CDN
      c. Application
      d. Database

    4. Caching Pseudocode
    ```
    # Retrieval
    def app_request(tweet_id):
      cache = {}

      data = cache.get(tweet_id)

      if data:
        return data
      elase:
        data = db_query(tweet_id)
        # set data in cache
        cache[tweet_id] = data
        return data
    ```

    ```
    # Writing
    def app_update(tweet_id, data):
      cache = {}

      db_update(data)

      cache.pop(tweet_id)
    ```
    5. Distributed Cache

      a. Works same as traditional cache
      b. Has built-in functionality to replicate data, shard data across servers, and locate proper server for each key
    
    6. Cache Eviction

      a. Needed for 2 reasons:
        1. Prevent stale data
        2. Caching only most valuable data to save resources
    
    7. TTL

      a. Set a time period berfore a cache entry is deleted
      b. Used to prevent stale date
    
    8. LRU/LFU

      a. Least Reacently used
        1. Once cache is full, remove last accessed key and add new key
      b. Least Frequently used
        1. Track number of times key is accessed
        2. Drop least used when cache is full
    
    9. Caching Strategies

      a. Cache Aside - most common
      b. Read Through
      c. Write Through
      d. Write Back

    10. Cache Consistency

      a. How to maintain consistency between database and cache effciently
      b. Importance depends on use case
  
  c. Database 

    Key Information: Most web apps are majority reads, around 95% +

    1. Database scaling
      a. Basic scaling techniques
        1. Indexes
          a. Index based on column
          b. Speed up read performance
          c. Writes and update become slightly slower
          d. More storage required for index
        2. Denomalization
          a. Add redundant data to tables to reduce joins
          b. Boost read performance
          c. Slow down writes (has to write same data into multiple tables)
          d. Risk inconsistent data across tables
          c. Code is harder to write
        3. Connection pooling
          a. Allow multiple application threads to use same DB connection
          b. Saves on overhead of independent DB connections
        4. Caching
          a. Not directly related to DB
          b. Cache sits in front of DB to handle serving content
          c. Can't cache everything (dianamic data/frequently updating data Ex - realtime drivers location)
        5. Vertical scaling
          a. Get a bigger server
          b. Easiast solution when starting out  

    2. Replication and Partioning
      a. Read replicas:
        1. Create replica servers to handle reads
        2. Master server dedicated only to weites
        3. Have to handle making sure new data reached replicas
      b. Partitioning
        1. Sharding (Horizontal partitioning)
          a. Schema of table stays the same, but split across multiple DBs
              Users Table - Master(A-M) , Master(N-Z)
          b. Downsides - Hot keys(Ex: more names comes in between A-M), no join across shards(Because the operations would be very slow)
        2. Vertical partitioning
          a. Devide schema of database into seperate tables
          b. Generally divide by functionality
          c. Best when most data in row isn't need for most queries
            Ex: ID, User, Email, Address --> ID, User | Email, Address
    
    3. Choose between SQL & NoSQL Database ???