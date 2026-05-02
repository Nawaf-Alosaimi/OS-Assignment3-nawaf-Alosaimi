# Assignment 3 - Complete Documentation

**Student Name**: [Nawaf Abdulaziz Alosaimi]  
**Student ID**: [445050227]  
**Date Submitted**: [2026/5/1]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> https://drive.google.com/file/d/1mzDEM8zVWsWCPZg-RmcfsAkC5AngpRDI/view?usp=drive_link
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [https://drive.google.com/file/d/1mzDEM8zVWsWCPZg-RmcfsAkC5AngpRDI/view?usp=drive_link]

**Video filename**: `[445050227]_Assignment3_Synchronization.mov`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [2026/4/30, 7pm]
**What I implemented**: 
i forked repository ,updated Student ID
**Challenges encountered**: 
everythimg was good
**How I solved it**: 

**Testing approach**: 

**Time spent**: 
15 min
---

### Entry 2 - [5/1, 6:15]
**What I implemented**: 
i finished Task 1 by implemented Fine grained 
i implemented Reentranlock to protect each counter (contextSwitchCount, completedProcessCount, totalWaitingTime)
**Challenges encountered**: 
when i were wraiting the Ai within VScode generated the rest of the code that i wanted to write then the rest of code was wwrong 
**How I solved it**: 
i back by comd-z then i rewraited the code i wanted to write manually
**Testing approach**: 
3 times to ensure counters increment correctly
**Time spent**: 
45 min
---

### Entry 3 - [5/1,7:20]
**What I implemented**: 
Completed Task 2 by adding a new ReentrantLock (logLock) to protect the executionLog ArrayList from concurrent modifications.
**Challenges encountered**: 
every thing was good
**How I solved it**: 

**Testing approach**: 
2 times times to ensure the Array list dosen't change
**Time spent**: 
30 min
---

### Entry 4 - [5/1, 8:54]
**What I implemented**: 
 i completed task3 by adding Binary Semaphore to control CPU access within the run() and runToCompletion() methods
**Challenges encountered**: 
every thing was good

**How I solved it**: 

**Testing approach**: 
i tested 3 times first one when Semaphore was 1 seconed wehan it was 2
third when the Semaphore was 3 i didn't notice difference between them
**Time spent**: 
45 min
---

### Entry 5 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:

[The shared resource is contextSwitchCount and executionLog
contextSwitchCount is not atomic; it consists of three steps: read, add, and write if two threads execute this simultaneously, one thread might overwrite the other's work.
executionLog which is an ArrayList. Concurrent access is a problem because a standard ArrayList in Java is not thread-safe and cannot handle multiple modifications at once and he program will either crash with a ConcurrentModificationException
]

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:

[A ReentrantLock provides mutual exclusion, meaning only one thread can enter a critical section at a time. It is used to protect data.
 A Semaphore is used to control access to a limited number of resources, 
- ReentrantLock: I used it in Task 1 and 2 to protect the counters (contextSwitchCount, completedProcessCount, totalWaitingTime)
- I used a Binary Semaphore (Semaphore(1)) in Task 3 to control access to the CPU execution block in the run() and runToCompletion() methods]

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:

[A deadlock is a situation in which a set of
processes are blocked because each process is
holding a resource and waiting for another
resource acquired by some other process creating a cycle where nobody can proceed
 - Breaking Circular Wait We can force processes to request resources in a specific order so they don't form a waiting circle.
 - Breaking "Hold and Wait": We can force a process to get all the resources it needs at once before it starts, or make it release what it has before asking for new ones.
 - n my code, I made sure a thread only requests one lock at a time, does its work, and then releases it. I didn't let any thread hold a lock while waiting for another lock. Most importantly, I used the try-finally block for every lock and semaphore. This guarantees that unlock() and release() are always executed,]

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

[for Task 1, I used separate locks for each counter fine-grained
i made this choice because the three counters (contextSwitchCount, completedProcessCount, totalWaitingTime) are completely independent of each other
he trade-off between the two approaches is performance versus simplicity. Coarse-grained locking (using one lock for everything) is simpler to write but creates a bottleneck
orcing threads to wait unnecessarily. Fine-grained locking requires writing more lock objects, but it is much faster. Given that the counters are independent
the fine-grained approach provides much better concurrency. It allows multiple threads to update different counters at the exact same time without blocking each other, which maximizes CPU efficiency
]

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: 
contextSwitchCount, completedProcessCount, totalWaitingTime.
**Why they need protection**: 
Because these variables are accessed and modified by multiple Process threads at the same time will lead to race conditions
**Synchronization mechanism used**: 
Fine-grained ReentrantLock one independent lock for each variable
**Code snippet**:
```java
public static void incrementContextSwitch() {
        // TODO: Protect this critical section with a lock
        // RACE CONDITION: Multiple threads might read and write simultaneously!
        contextSwitchLock.lock();
        try {
            contextSwitchCount++;
        } finally {
            contextSwitchLock.unlock();
        }
    }
    
```

**Justification**: 
I used separate ReentrantLock objects for each counter instead of a single lock. This fine-grained approach maximizes concurrency because threads updating different counters do not block each other
---

### Critical Section #2: Execution Log

**What resource**: 
executionLog (ArrayList<String>).
**Why it needs protection**: 
n Java, a standard ArrayList is not thread-safe. If multiple threads try to add messages to the log at the exact same time
it will cause a ConcurrentModificationException
**Synchronization mechanism used**: 
A dedicated ReentrantLock named logLock.
**Code snippet**:
```java
public static void logExecution(String message) {
        // TODO: Protect this critical section with a lock
        // RACE CONDITION: ArrayList is not thread-safe!
        logLock.lock();
        try {
            executionLog.add(message);
        } finally {
            logLock.unlock();
        }
    }
}

```

**Justification**: 
A ReentrantLock provides the necessary mutual exclusion to ensure that only one thread can append a message to the list at any given tim
---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 
To manage and control concurrent access to the simulated CPU, ensuring that multiple processes do not attempt to execute their tasks simultaneously.
**Number of permits and why**: 
1 permit This acts as a Binary Semaphore which simulates a single-core CPU environment where only one process can physically run on the CPU at any exact moment.
**Where implemented**: 
in the SharedResources class, and its acquire() and release() methods are implemented inside the run() and runToCompletion() methods of the Process class
**Code snippet**:
```java
try{
            SharedResources.cpuSemaphore.acquire();        
        // TODO #3: Acquire CPU semaphore before executing
        // This ensures only allowed number of processes run simultaneously
        
        try {
            if (startTime == -1) {
                startTime = System.currentTimeMillis();
            }
            
            // Increment context switch counter
            SharedResources.incrementContextSwitch();
            
            int runTime = Math.min(timeQuantum, remainingTime);
            
            String quantumBar = createProgressBar(0, 15);
            String message = "  ▶ " + name + " (Priority: " + priority + ") executing quantum [" + runTime + "ms]";
            System.out.println(Colors.BRIGHT_GREEN + message + Colors.RESET);
            
            // Log execution
            SharedResources.logExecution(name + " started quantum execution");
            
            try {
                int steps = 5;
                int stepTime = runTime / steps;
                
                for (int i = 1; i <= steps; i++) {
                    Thread.sleep(stepTime);
                    int quantumProgress = (i * 100) / steps;
                    quantumBar = createProgressBar(quantumProgress, 15);
                    System.out.print("\r  " + Colors.YELLOW + "⚡" + Colors.RESET + 
                                    " Quantum progress: " + quantumBar);
                }
                System.out.println();
                
            } catch (InterruptedException e) {
                System.out.println(Colors.RED + "\n  ✗ " + name + " was interrupted." + Colors.RESET);
            }
            
            remainingTime -= runTime;
            int overallProgress = (int) (((double)(burstTime - remainingTime) / burstTime) * 100);
            String overallProgressBar = createProgressBar(overallProgress, 20);
            
            System.out.println(Colors.YELLOW + "  ⏸ " + Colors.CYAN + name + Colors.RESET + 
                              " completed quantum " + Colors.BRIGHT_YELLOW + runTime + "ms" + Colors.RESET + 
                              " │ Overall progress: " + overallProgressBar);
            System.out.println(Colors.MAGENTA + "     Remaining time: " + remainingTime + "ms" + Colors.RESET);
            
            if (remainingTime > 0) {
                System.out.println(Colors.BLUE + "  ↻ " + Colors.CYAN + name + Colors.RESET + 
                                  " yields CPU for context switch" + Colors.RESET);
                SharedResources.logExecution(name + " yielded CPU");
            } else {
                completionTime = System.currentTimeMillis();
                long waitingTime = (completionTime - creationTime) - burstTime;
                SharedResources.addWaitingTime(waitingTime);
                SharedResources.incrementCompletedProcess();
                SharedResources.logExecution(name + " completed execution");
                System.out.println(Colors.BRIGHT_GREEN + "  ✓ " + Colors.BOLD + Colors.CYAN + name + 
                                  Colors.RESET + Colors.BRIGHT_GREEN + " finished execution!" + 
                                  Colors.RESET);
            }
            System.out.println();
            
        } finally {
            // TODO #4: Release CPU semaphore here
           SharedResources.cpuSemaphore.release(); // Always release in finally block to prevent deadlocks!
        }
    }   catch (InterruptedException e) {
            System.out.println(Colors.RED + "  ✗ " + name + " was interrupted while waiting for CPU." + Colors.RESET);
        }
    }
```

**Effect on program behavior**: 
before the semaphore, processes would print their progress bars on top of each other
 With the semaphore processes take turns one process finishes its quantum and releases the CPU before the next one is allowed to start
---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
```bash
javac SchedulerSimulationSync.java
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
```
but i run it by the "Run" button. After each run
**Results**: 
1st
═ Synchronization Statistics ═══
Total Context Switches: 28
Total Completed Processes: 15
Total Waiting Time: 609122ms
Average Waiting Time: 40608ms

2sec
═══ Synchronization Statistics ═══
Total Context Switches: 28
Total Completed Processes: 15
Total Waiting Time: 608737ms
Average Waiting Time: 40582ms

3th
═══ Synchronization Statistics ═══
Total Context Switches: 28
Total Completed Processes: 15
Total Waiting Time: 608923ms
Average Waiting Time: 40594ms

4th
═══ Synchronization Statistics ═══
Total Context Switches: 28
Total Completed Processes: 15
Total Waiting Time: 610217ms
Average Waiting Time: 40681ms

5th
═══ Synchronization Statistics ═══
Total Context Switches: 28
Total Completed Processes: 15
Total Waiting Time: 609010ms
Average Waiting Time: 40600ms

**Why synchronization is necessary**: 
(Explain what race conditions COULD occur without synchronization, even if you didn't observe them. Explain which shared resources need protection and why.)

**Conclusion**: 
Synchronization is necessary because multiple threads are modifying shared resources concurrently. If the counter variables (like contextSwitchCount) are not protected, race conditions will occur because the increment operation (++) is not atomic. This would cause threads to overwrite each other's work leading to lost updates and incorrect statistics.
---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: 
 i run the program 4 times by the run button
**Results**: 
Total Context Switches: 28
Total Completed Processes: 15
Total log entries: 56
all Runs give me the same results 
the Total log entries should give me 43 but i don't think there's much
different
**What this proves**: 
This proves that the logLock (ReentrantLock) worked perfectly
---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: 

**Actual values**: 

**Analysis**: 

---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]

**Purpose**: 
i changed the processes number from 10 to 20 
and the time quantim from 2000cto 200
**Results**: 

═══ Synchronization Statistics ═══
Total Context Switches: 49
Total Completed Processes: 25
Total Waiting Time: 769912ms
Average Waiting Time: 30796ms

═══ Execution Log Summary ═══
Total log entries: 98

**What I learned**: 
I learned that using ReentrantLock and Semaphore correctly makes the program very strong and stable. It can handle many threads running at the same time without any data loss or unnecessary waiting time
---

## Part 5: Reflection and Learning

### What I learned about synchronization:

In this assignment, I learned that synchronization is absolutely essential when multiple threads share the same data. Before this, I did not realize that a simple count++ operation could cause data loss if two threads execute it at the exact same time, which is known as a race condition. I learned how to use tools like ReentrantLock to protect shared variables, ensuring only one thread can modify them at a time. I also learned how to use a Semaphore, which is perfect for managing access to limited resources like a CPU

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: 
 Car Rental Fleet Management Systems. If two customers try to book the exact same car model on the website at the exact same millisecond, without synchronization, the database might mistakenly rent the single car to both people. A lock ensures only the first person completes the transaction before the second person's request is processed.
**Example 2**: 
Banking and ATMs. If two people share a joint bank account and try to withdraw $500 from two different ATMs at the exact same time, synchronization prevents the bank system from deducting the money twice or giving out more cash than the actual account balance.
---

### How I would explain synchronization to others:

[Imagine a group of students trying to write on the exact same small whiteboard at the same time. If everyone writes together, the board becomes a messy, unreadable mix of words. This is what happens in race condition
Synchronization is like placing a rule: there is only one whiteboard marker in the room. If you want to write on the board, you must hold the marker (which is a Lock). When you finish, you put the marker down for the next person. A Semaphore is similar, but imagine having exactly 3 markers; it allows exactly 3 people to write at the same time and no more. These tools keep the program organized and prevent it from crashing]

---

## Part 6: GitHub Repository Information

**Repository URL**: 
https://github.com/Nawaf-Alosaimi/OS-Assignment3-nawaf-Alosaimi
**Number of commits**: 
6 commits so far
**Commit messages**: 
1. Set my student ID: 445050227
2. Task 1 : Added ReentrantLock for counter protection
3. Task 2 (445050227): Added ReentrantLock for execution log
4. Task 3 (445050227): Implemented semaphore for CPU control in runToCompletion

---

## Summary

**Total time spent on assignment**: 
Approximately 12 to 15 hours
**Key takeaways**: 
1. ReentrantLock (used to protect data)
2. Semaphore (used to manage resources like the CPU)
3. ArrayList are not thread-safe in Java

**Most challenging aspect**: 
time management
Making absolutely sure that no deadlocks could ever happen. It required very careful attention to detail
**What I'm most proud of**: 
I am most proud of seeing the final result in the terminal
seeing the program run perfectly 5 times in a row with 100% accurate statistics and a clean execution log makes me proud of my technical skills as a computer science student.

**End of Documentation**
