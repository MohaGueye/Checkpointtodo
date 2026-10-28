# Operating System Checkpoint Solutions

## Part 1: Process Scheduling

Scenario:
- P1: arrival time = 0, burst = 5
- P2: arrival time = 1, burst = 3
- P3: arrival time = 2, burst = 1

### 1) FCFS (First-Come, First-Served)

Execution order:
- P1 runs from 0 to 5
- P2 runs from 5 to 8
- P3 runs from 8 to 9

Gantt chart:
```text
Time:   0      5      8      9
       |------|------|------|
       P1     P2     P3
```

Completion time:
- P1 = 5
- P2 = 8
- P3 = 9

Waiting time:
- P1 = 5 - 0 - 5 = 0
- P2 = 8 - 1 - 3 = 4
- P3 = 9 - 2 - 1 = 6

Average waiting time:
- (0 + 4 + 6) / 3 = 10 / 3 = 3.33

### 2) Round Robin (Quantum = 2)

Schedule:
- 0-2: P1
- 2-4: P2
- 4-5: P3
- 5-7: P1
- 7-8: P1

Gantt chart:
```text
Time:   0      2      4      5      7      8
       |------|------|------|------|------|
       P1     P2     P3     P1     P1
```

Completion time:
- P1 = 8
- P2 = 4
- P3 = 5

Waiting time:
- P1 = 8 - 0 - 5 = 3
- P2 = 4 - 1 - 3 = 0
- P3 = 5 - 2 - 1 = 2

Average waiting time:
- (3 + 0 + 2) / 3 = 5 / 3 = 1.67

### Which is more responsive?
Round Robin is more responsive because it gives each process a small time slice, reducing the time short jobs wait behind longer ones. FCFS is simpler, but less fair and less responsive under mixed workloads.

---

## Part 2: Process Synchronization

Two processes, P1 and P2, each increment a shared variable called `counter` 100 times.

### What can go wrong without synchronization?
Without synchronization, the statement `counter = counter + 1` is not atomic. Two processes can read the same value before either writes back, causing a lost update.

Example:
```text
Initial counter = 0
P1 reads counter -> 0
P2 reads counter -> 0
P1 writes 1
P2 writes 1
Final counter = 1 (wrong; expected 2)
```

This race condition can lead to incorrect final values.

### Suggested mechanism: Mutex
A mutex ensures only one process can access the critical section at a time.

### Pseudocode with mutual exclusion
```text
shared int counter = 0
mutex m

process P1:
    lock(m)
    for i = 1 to 100:
        counter = counter + 1
    unlock(m)

process P2:
    lock(m)
    for i = 1 to 100:
        counter = counter + 1
    unlock(m)
```

This guarantees that the value of `counter` ends at 200 after both processes finish.

---

## Part 3: Memory Management

Given:
- Number of frames = 3
- Page reference string = 1, 2, 3, 2, 4, 1, 5

### 1) FIFO Page Replacement

Execution:
1. Ref 1 -> fault -> [1]
2. Ref 2 -> fault -> [1, 2]
3. Ref 3 -> fault -> [1, 2, 3]
4. Ref 2 -> hit -> [1, 2, 3]
5. Ref 4 -> fault, replace 1 -> [2, 3, 4]
6. Ref 1 -> fault, replace 2 -> [3, 4, 1]
7. Ref 5 -> fault, replace 3 -> [4, 1, 5]

Page faults = 6

### 2) LRU Page Replacement

Execution:
1. Ref 1 -> fault -> [1]
2. Ref 2 -> fault -> [1, 2]
3. Ref 3 -> fault -> [1, 2, 3]
4. Ref 2 -> hit -> [1, 2, 3]
5. Ref 4 -> fault, replace 1 -> [2, 3, 4]
6. Ref 1 -> fault, replace 3 -> [2, 4, 1]
7. Ref 5 -> fault, replace 2 -> [4, 1, 5]

Page faults = 6

### Comparison
- FIFO faults = 6
- LRU faults = 6

In this case, both algorithms perform the same. LRU often performs better than FIFO in real systems because it keeps the most recently used pages and removes the least recently used ones, which matches common program behavior better.

---

## Part 4: Disk Scheduling

Given:
- Initial head position = 53
- Requests = 98, 183, 37, 122, 14, 124, 65, 67

### 1) FCFS

Order of service:
53 -> 98 -> 183 -> 37 -> 122 -> 14 -> 124 -> 65 -> 67

Head movement:
- 53 to 98 = 45
- 98 to 183 = 85
- 183 to 37 = 146
- 37 to 122 = 85
- 122 to 14 = 108
- 14 to 124 = 110
- 124 to 65 = 59
- 65 to 67 = 2

Total FCFS movement = 45 + 85 + 146 + 85 + 108 + 110 + 59 + 2 = 640

### 2) SSTF

Nearest request selection:
53 -> 65 -> 67 -> 37 -> 14 -> 98 -> 122 -> 124 -> 183

Head movement:
- 53 to 65 = 12
- 65 to 67 = 2
- 67 to 37 = 30
- 37 to 14 = 23
- 14 to 98 = 84
- 98 to 122 = 24
- 122 to 124 = 2
- 124 to 183 = 59

Total SSTF movement = 12 + 2 + 30 + 23 + 84 + 24 + 2 + 59 = 236

### Conclusion
SSTF is more efficient in this case because it has much less total head movement:
- FCFS = 640
- SSTF = 236

SSTF reduces seek time by always servicing the closest request first.

---

## Final Summary
- FCFS waiting time = 3.33
- Round Robin waiting time = 1.67
- Round Robin is more responsive
- Mutex prevents race conditions in shared memory
- FIFO and LRU both cause 6 page faults in this scenario
- SSTF is more efficient than FCFS for disk scheduling

This solution is suitable for a Google Doc or PDF submission and can be expanded with diagrams or charts if needed.
