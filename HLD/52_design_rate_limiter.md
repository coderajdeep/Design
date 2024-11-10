### Late Limiter prevent DDOS attack (API Gateway)
_In the context of rate limiting and traffic management, a burst refers to a sudden spike or increase in the number of requests or actions over a short period of time. For instance, a "burst of requests" means that multiple requests are sent in quick succession within a brief time window, potentially exceeding a system's expected or sustainable load._

There are several algorithms commonly used to implement rate limiting, each with different strengths and trade-offs. Here’s an overview of some popular rate limiter algorithms:

### 1. **Token Bucket**
   - **Description**: Tokens are added to a “bucket” at a constant rate (e.g., 1 token per second). Each request consumes one token. If no tokens are left, the request is denied. The bucket has a maximum capacity, so it allows bursts but at a controlled rate.
   - **Pros**: Good for handling bursts of requests. Smooths out request rates over time.
   - **Cons**: More complex to implement, especially in a distributed environment.

   **Example**: A bucket fills with 5 tokens per second, allowing a burst of up to 100 tokens.

### 2. **Leaky/Leaking Bucket**
   - **Description**: Similar to the token bucket, but requests are processed at a fixed rate, with excess requests being either queued or discarded. Think of it as water leaking from a bucket at a steady rate, regardless of how quickly water is added.
   - **Pros**: Provides a consistent, smooth request rate and handles bursts by queuing requests.
   - **Cons**: May add latency if requests are queued and not processed immediately.

   **Example**: Allows 10 requests per second, with additional requests queued up and processed at the fixed rate.

### 3. **Fixed Window Counter**
   - **Description**: In this method, a counter is maintained for each fixed time window (e.g., every minute, hour). Each request increments the counter, and if the count exceeds the limit within the window, requests are blocked until the next window.
   - **Pros**: Simple and easy to implement.
   - **Cons**: It can lead to “burstiness” at the boundaries of windows. For example, a client can send a large number of requests at the end of one window and the start of the next.

   **Example**: 100 requests per minute, reset every minute.

### 4. **Sliding Window Log**
   - **Description**: This algorithm maintains a log of timestamps for each request and checks how many requests were made in the sliding window by counting entries within the time window. Each new request triggers a check on the log to remove expired entries.
   - **Pros**: Accurate enforcement of limits over a true sliding time window.
   - **Cons**: Memory-intensive, as it needs to store timestamps for each request, which may be impractical for very high loads.

   **Example**: Keep a log of requests in the last minute, dropping entries older than a minute.

### 5. **Sliding Window Counter**
   - **Description**: Combines aspects of fixed window and sliding window log. The time window is divided into smaller sub-windows (e.g., 1-second intervals in a 1-minute window), and requests are counted per sub-window. When checking rate limits, it sums up the counts in the relevant sub-windows.
   - **Pros**: Reduces burstiness while using less memory than the sliding window log.
   - **Cons**: Slightly more complex to implement but efficient for high-throughput systems.

   **Example**: Divide a minute window into 10-second sub-windows and count requests across the last few windows.


### 6. **Concurrency Limiter (Not mentioned in lecture)**
   - **Description**: Limits the number of concurrent requests rather than the rate of incoming requests. This is often used to limit the number of simultaneous actions a user can perform.
   - **Pros**: Useful for scenarios where it’s important to control concurrent load (e.g., database connections, API calls).
   - **Cons**: Doesn’t directly control the rate; instead, it restricts the number of parallel operations.

   **Example**: A user can have up to 5 concurrent API calls.

### Comparison of Rate Limiting Algorithms

| Algorithm              | Strengths                                          | Weaknesses                                  |
|------------------------|----------------------------------------------------|---------------------------------------------|
| Fixed Window Counter   | Simple, easy to implement                          | Susceptible to burstiness at boundaries     |
| Sliding Window Log     | Accurate, smooth rate limiting                     | Memory-intensive, slower with high traffic  |
| Sliding Window Counter | Balances accuracy and memory efficiency            | More complex than fixed windows             |
| Token Bucket           | Allows bursts within limits, smooths over time     | Complex to implement in distributed systems |
| Leaky Bucket           | Smooth and consistent request handling             | Queues can add latency                      |
| Concurrency Limiter    | Limits concurrent actions, protects resources      | Doesn’t directly limit request rate         |

### Choosing an Algorithm
The choice of algorithm depends on the specific needs of the application. If handling burst traffic is important, **Token Bucket** or **Leaky Bucket** may be a good fit. For applications where smooth request flow is needed, **Leaky Bucket** or **Sliding Window Counter** can work well. **Concurrency Limiters** are ideal for scenarios that require control over concurrent resource usage.