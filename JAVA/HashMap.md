### 1. How HashMap works internally?
- https://youtu.be/wZLn2BN1TvY
- https://youtu.be/1CJbB6SzjVw

### Yes, a synchronized HashMap allows one null key and any number of null values. This is in contrast to Hashtable, which does not permit null keys or values at all.

### 2. Difference between Concurrent HashMap and Synchronized HashMap and HashTable?
In Java, both `ConcurrentHashMap` and `synchronized HashMap` are used to handle thread-safe operations, but they do so in different ways and are suited for different use cases. Here are the key differences:

### 1. Locking Mechanism
- **`synchronized HashMap`**: Locks the entire map for each read or write operation. This means only one thread can access the map at a time, leading to potential bottlenecks when multiple threads are trying to access it.
- **`ConcurrentHashMap`**: Uses a finer-grained locking mechanism called **lock stripping**. This divides the map into segments, and each segment can be locked independently. This allows multiple threads to read and write to different segments of the map simultaneously, improving throughput.

### 2. Performance
- **`synchronized HashMap`**: Slower in multithreaded environments because every operation (read or write) requires locking the entire map, causing contention and delays.
- **`ConcurrentHashMap`**: Faster in multithreaded contexts due to its segmented locking approach, allowing greater parallelism and less contention.

### 3. Null Values
- **`synchronized HashMap`**: Allows null keys and values.
- **`ConcurrentHashMap`**: Does not allow null keys or values. Attempting to insert null will throw a `NullPointerException`.

### 4. Concurrency Level
- **`synchronized HashMap`**: Does not support adjustable concurrency; it always locks the whole map.
- **`ConcurrentHashMap`**: Allows specifying the concurrency level (number of segments) during instantiation, which can improve performance depending on the expected usage pattern.

### 5. Iteration Behavior
- **`synchronized HashMap`**: Must be explicitly synchronized while iterating to prevent `ConcurrentModificationException`.
- **`ConcurrentHashMap`**: Allows safe iteration, even while other threads modify the map. It provides a weakly consistent iterator that reflects the state of the map at some point during or after the beginning of the iteration.

### When to Use Which?
- **Use `synchronized HashMap`** if you have minimal concurrent access needs and require null keys or values.
- **Use `ConcurrentHashMap`** for high concurrency situations where multiple threads frequently access and update the map.

In general, `ConcurrentHashMap` is preferred for multithreaded applications due to its better scalability and performance characteristics.

Both `ConcurrentHashMap` and `Hashtable` in Java are thread-safe implementations of the `Map` interface, but they have significant differences in terms of performance, design, and flexibility. Here’s a breakdown:

### 1. Locking Mechanism
- **`Hashtable`**: Uses a single lock for the entire map. This means that every read and write operation requires acquiring a lock on the entire `Hashtable`, which leads to contention when multiple threads are trying to access or modify it.
- **`ConcurrentHashMap`**: Utilizes a more granular locking mechanism known as **lock stripping**, which divides the map into multiple segments, each with its own lock. This allows multiple threads to operate on different segments of the map concurrently, improving performance in multithreaded environments.

### 2. Performance
- **`Hashtable`**: Slower under high concurrency due to the single global lock on the entire table. The synchronized access means that only one thread can perform read or write operations at a time, creating bottlenecks.
- **`ConcurrentHashMap`**: Faster in concurrent scenarios. Since only specific segments are locked rather than the entire map, multiple threads can work on different parts of the map simultaneously, reducing contention.

### 3. Null Keys and Values
- **`Hashtable`**: Does not allow null keys or values. Any attempt to insert null will result in a `NullPointerException`.
- **`ConcurrentHashMap`**: Also does not allow null keys or values for similar reasons; null entries are disallowed to avoid ambiguity in concurrent contexts (e.g., a null could indicate either the absence of a key or a value).

### 4. Iteration Behavior
- **`Hashtable`**: Provides fail-fast iterators, which throw a `ConcurrentModificationException` if the map is modified during iteration by another thread. This behavior requires explicit external synchronization for safe iteration.
- **`ConcurrentHashMap`**: Provides a **weakly consistent** iterator, meaning it will not throw an exception if the map is modified during iteration. Instead, it reflects the state of the map at some point during or after the iteration began, making it safer for concurrent access without external synchronization.

### 5. Concurrency Level
- **`Hashtable`**: Has a single concurrency level, meaning it locks the entire table. There is no way to control the level of concurrency.
- **`ConcurrentHashMap`**: Allows specifying the concurrency level at the time of instantiation (the default is 16), which defines the number of segments. This flexibility allows better performance tuning based on expected concurrent access patterns.

### 6. Default Synchronization
- **`Hashtable`**: Every method is synchronized by default, even if synchronization may not be needed. This can lead to overhead when access does not require synchronization.
- **`ConcurrentHashMap`**: Only segments are locked as needed, leading to lower overhead, particularly for read-heavy workloads.

### When to Use Which?
- **Use `Hashtable`** if you need a simpler thread-safe map and do not require high concurrency or flexibility. Its single-lock approach makes it suitable for scenarios with minimal concurrent access.
- **Use `ConcurrentHashMap`** for applications with high concurrency demands. Its better segmentation, weakly consistent iteration, and performance optimizations make it a preferred choice in modern, multithreaded environments. 

In summary, `ConcurrentHashMap` is generally preferred in concurrent applications due to its better scalability and design. `Hashtable` is considered outdated in most contexts where concurrency is a key requirement.

In Java, when you create a `HashMap`, it initializes an internal array (also called a "table") with a default capacity of 16. This array holds entries (key-value pairs), and the `HashMap` dynamically grows its size as needed when more elements are added. Here's a closer look at how and when the table size grows:

### 1. Initial Capacity and Load Factor
- **Initial Capacity**: By default, the initial capacity of a `HashMap` is 16, but you can specify a different initial capacity when creating the `HashMap`.
- **Load Factor**: The load factor is a measure of how full the hash table is allowed to get before it resizes. By default, the load factor is 0.75, which means the `HashMap` will resize (double its capacity) when it reaches 75% of its current capacity.

### 2. When Does Resizing Happen?
The `HashMap` resizes (doubles its capacity) when the number of entries in the map exceeds the **threshold**, which is calculated as:
\[
\text{Threshold} = \text{Initial Capacity} \times \text{Load Factor}
\]

For a `HashMap` with a default initial capacity of 16 and a load factor of 0.75, the threshold is:
\[
16 \times 0.75 = 12
\]

This means that when the number of entries reaches 12, the `HashMap` will resize by doubling its capacity to 32.

### 3. Resizing Process
When the `HashMap` grows:
1. A new internal array with double the current capacity is created.
2. Existing entries are rehashed (recalculated for the new array positions) and moved to the new table.
3. This resizing process is costly in terms of performance because it involves rehashing and relocating all existing entries.

### 4. Why Resizing?
Resizing helps maintain a balanced distribution of entries across the array, which keeps lookup, insertion, and deletion operations efficient. If the `HashMap` grows too dense (too many entries per bucket), it leads to longer linked lists or tree structures, reducing performance.

### Example
Here's an example of how resizing works with the default settings:
1. Initialize `HashMap` with capacity 16, load factor 0.75.
2. Insert entries until the size reaches 12 (the threshold).
3. On inserting the 13th entry, the `HashMap` resizes to a capacity of 32.
4. The new threshold becomes \( 32 \times 0.75 = 24 \), so resizing will happen again when there are 24 entries. 

By allowing the map to expand dynamically, `HashMap` ensures it stays efficient as it grows.