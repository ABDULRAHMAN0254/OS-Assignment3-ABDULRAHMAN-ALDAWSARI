# Assignment 3 - Complete Documentation

**Student Name**: [ABDULRAHMAN IBRAHIM ALDAWSARI]  
**Student ID**: [445050254 ]  
**Date Submitted**: [ 2026 may 2]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: `[YourStudentID]_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [2026 may 1, 9pm]
**What I implemented**: i forked Repository on GitHub and i made my Repository Public 
and Rename it

**Time spent**: 20 min

---

### Entry 2 - [2026 may 1 , 9:30]
**What I implemented**: i used VScode to clone from github
and i set my studentID and made my firest commit

**Time spent**: 30 min

---

### Entry 3 - [2026 may 1, 10 pm]
**What I implemented**: i Add ReentrantLock to protect counter variables
and commit it

**Testing approach**: 

**Time spent**: 30 min

---

### Entry 4 - [2026 may 1, 10:30 pm]
**What I implemented**: i Add ReentrantLock to protect execution log and commit it

**Time spent**: 30 min

---

### Entry 5 - [2026 may 1, 11 pm ]
**What I implemented**: Add Semaphore to control concurrent CPU access 

**Time spent**: 30 min

---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
Q1- What shared resource is affected?

Q2- Why is concurrent access a problem?

Q3- What incorrect behavior could occur?


**Your Answer**:

[The first race condition is in contextSwitchCount++, which is a shared variable. Concurrent access is a problem because multiple threads may update it at the same time and lose updates. This can cause incorrect context switch count.

The second race condition is in executionLog.add(). The shared resource is the ArrayList. Concurrent access is a problem because ArrayList is not thread-safe, so data can be corrupted or lost.]

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:

[ReentrantLock is used to allow only one thread to access a critical section at a time, and I used it to protect counters and execution log.

Semaphore controls how many threads can access a resource. I used it to control CPU execution so only one process runs at a time.]

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:

[Deadlock is when threads wait forever for each other’s resources.

To prevent it, I used try-finally so locks and semaphores are always released.

This ensures no thread keeps holding a resource.]

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

[I used one lock (coarse-grained) for all three counters.

I chose this because it is simple and easy to manage.

The disadvantage is lower performance because threads must wait even if counters are independent.

Fine-grained locking would allow better concurrency because each counter has its own lock.

Since the counters are independent, fine-grained locking gives better performance because multiple threads can work at the same time.
]

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: 
contextSwitchCount, completedProcessCount, totalWaitingTime
**Why they need protection**: 
hey are shared between threads, so multiple threads can update them at the same time and cause wrong values (race condition).
**Synchronization mechanism used**: 
ReentrantLock (to ensure only one thread updates at a time)

**Code snippet**:
```java

lock.lock();
try {
    contextSwitchCount++;
    completedProcessCount++;
    totalWaitingTime += time;
} finally {
    lock.unlock();
}
```

**Justification**: 
Without locking, two threads may update the same counter together and lose updates.
---

### Critical Section #2: Execution Log

**What resource**: What resource:
executionLog (ArrayList)

**Why it needs protection**:
ArrayList is not thread-safe, so multiple threads adding logs at the same time can corrupt data or lose entries.

**Synchronization mechanism used**: 
ReentrantLock
**Code snippet**:
```java

lock.lock();
try {
    executionLog.add(message);
} finally {
    lock.unlock();
}
```

**Justification**: 
Lock ensures logs are added one by one safely without data corruption.
---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 
To control how many processes can use the CPU at the same time (limit concurrency).

**Number of permits and why**: 
1 permit, because only one process should execute in the CPU at a time.

**Where implemented**: 
Inside run() before execution starts.
**Code snippet**:
```java

cpuSemaphore.acquire();

try {
    // process execution
} finally {
    cpuSemaphore.release();
}
```

**Effect on program behavior**: 

Prevents multiple processes from running simultaneously and avoids CPU conflict.
---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
Run program several times and compare outputs
```bash
# Commands used (run the program at least 5 times)
```

**Results**: 
Final results are mostly the same, but order of execution and logs may change each run.

**Why synchronization is necessary**: 
Because threads share data like counters and logs, and without synchronization they can update them at the same time causing wrong values.

**Conclusion**: 
Synchronization ensures correct and consistent shared data.
---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: 
Run program and check for errors in log updates.
**Results**: 
Without locking, ConcurrentModificationException may happen.
**What this proves**: 
ArrayList is not thread-safe.
---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: 
All processes complete successfully.
**Actual values**: 
Correct and match expected output.
**Analysis**: 
Synchronization prevents lost updates
---

### Test 4: Different Scenarios
**Scenario tested**: [Different number of processes and time quantum.]

**Purpose**: 
Check program stability.
**Results**: 
Program still works correctly.
**What I learned**: 

Synchronization keeps program stable in all cases.
---

## Part 5: Reflection and Learning

### What I learned about synchronization:

[ data in multithreading. Without it, race conditions happen and results become incorrect. I learned that locks protect critical sections so only one thread can update data at a time. Semaphores control how many threads access a resource. try-finally is used to avoid deadlocks by always releasing locks. Overall, synchronization makes programs safe and correct]

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**:  Bank systems (prevent wrong balance updates when many users withdraw/deposit).

**Example 2**:  Online booking systems (avoid two users booking the same seat).

---

### How I would explain synchronization to others:

[It is like a shared bathroom with one key. Only one person can enter at a time, and others must wait. This prevents conflicts and keeps things correct.]

---

## Part 6: GitHub Repository Information

**Repository URL**: https://github.com/ABDULRAHMAN0254/OS-Assignment3-ABDULRAHMAN-ALDAWSARI

**Number of commits**: 4

**Commit messages**: 
1. Set my student ID: 445050254
2. Add ReentrantLock to protect counter variables
3. Add ReentrantLock to protect execution log 
4.  Add Semaphore to control concurrent CPU access

---

## Summary

**Total time spent on assignment**: 5 houres

**Key takeaways**: 
1. Learned race conditions and how to fix them
2. Understood locks and semaphores in Java
3. Improved understanding of multithreading

**Most challenging aspect**: 

**What I'm most proud of**: 
Making the scheduler work with proper synchronization
---

**End of Documentation**
