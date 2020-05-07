# Troubleshooting and Debugging Techniques

## Troubleshooting Concepts

### Debugging

* Tools that can be helpful:

    * Tools like **tcpdump** and **Wireshark** can show us ongoing network connections, and help us analyze the traffic going over our cables.

    * Tools like **ps**, **top**, or **free** can show us the number and types of resources used in the system.

    * We can use a tool like **strace** to look at the system calls made by a program, or **ltrace** to look at the library calls made by the software.

* Helpful pieces of advice and Terms:

    * **Troubleshooting** is the process of identifying, analyzing, and solving problems.

    * **Debugging** is the process of identifying, analyzing, and removing bugs in a system.

    * **Debuggers** let us follow the code line by line, inspect changes in variable assignments, interrupt the program when a specific condition is met, and more.

### Problem Solving Steps

* The problem solving steps are:

    1. Getting information
    1. Finding the root cause
    1. Performing the necessary remediation

* Helpful pieces of advice and Terms:

    * **Reproduction case** is a clear description of how and when the problem appears.

    * **Immediate remediation** gets the system back to health.

    * **Medium** or **long-term remediation** avoids the problem in the future.

### Silently Crashing Application

* **Commands**:
    * __strace ./program__ - traces a system calls made by the program and tell us what the result of each of these calls was.
    * __strace -o new_file ./program__ - creates a new file with the results of system calls made by the program.
    * __strace ./script.py | less__ - piping the less command allows you to scroll through a lot of text output. 

* Helpful pieces of advice and Terms:

    * **System calls** are the calls that the programs running on our computer make to the running kernel.

### "It Doesn't Work"

* **Common questions**:
    * What were you trying to do? 
    * What steps did you follow? 
    * What was the expected result? 
    * What was the actual result?

* Helpful pieces of advice and Terms:

    * **The load average** on Linux shows how much time a processor is busy in a given minute, with one meaning it was busy for the whole minute. 

    * Normally this number(the load average) shouldn't be above the amount of processors in the computer.

    * A number(the load average) higher than the amount of processors means the computer is overloaded. 

### Creating a Reproduction Case

1. Read the logs available
    * Which logs to read, will depend on the operating system and the application that you're trying to debug.
1. Isolate the conditions that trigger the issue
    * Do other users in the same office also experienced the problem? 
    * Does the same thing happen if the same user logs into a different computer? 
    * Does the problem happen if the applications config directory is moved away?

* Helpful pieces of advice and Terms:

    * **Reproduction case** is a way to verify if the problem is present or not.

### Finding the Root Cause

* Finding the actual root cause of the problem includes the next steps:
    1. Looking at the information we have
    1. Coming up with a hypothesis that could explain the problem
    1. Testing our hypothesis
    1. Go back to the beginning and try different possibility

* Tools that can be helpful:

    * **iotop** is a tool similar to top that lets us see which processes are using the most input and output. 

    * Other related tools are **iostat** and **vmstat**, these tools show statistics on the input/output operations and the virtual memory operations.

    * If the issue is that the process generates too much input or output, we could use a command like **ionice** to make our backup system reduce its priority to access the disk and let the web services use it too.

    * **iftop** is another tool similar to top that shows the current traffic on the network interfaces.

    * The **rsync** command, which is often used for backing up data, includes a **-bwlimit** for checking if the backup software already includes an option to limit the bandwidth.

    * A program **Trickle** can be used to limit the bandwidth being used.

    * The **nice** command reduces the priority of the process and accessing the CPU.

* Helpful pieces of advice and Terms:

    * Understanding the root cause is essential for performing the **long-term remediation**.

    * Whenever possible, we should check our hypothesis in a test environment, instead of the production environment that our users are working with.

### Dealing with Intermittent Issues

* Rebooting a computer or restarting a program changes a bunch of things like:
    * Going back to a clean slate means releasing all allocated memory
    * Deleting temporary files
    * Resetting the running state of programs
    * Re-establishing network connections
    * Closing open files and more

* Depending on what the problem is, you might want to look at different sources of information, like:
    * The load on the computer
    * The processes running at the same time
    * The usage of the network, and so on

* Heisenbugs usually point to these factors:
    * Bad resource management
    * Wrongly allocated memory
    * Not correctly initialized network connections
    * Not properly handled open files

* Helpful pieces of advice and Terms:

    * If a problem goes away by turning it off and on again, there's almost certainly a bug in the software, and the bug probably has to do with not managing resources correctly.

    * Power cycling releases resources stored in cache or memory, which gets rid of the problem.

    * Sometimes, the bug goes away when we *add extra logging information*, or when we *follow the code step by step using a debugger*. This is an especially annoying type of intermittent issue, nicknamed **Heisenbug**, in honor of Werner Heisenberg. He's the scientist that first described the observer effect, where just observing a phenomenon alters the phenomenon.

    * **Observer effect** means that when just observing a phenomenon alters the phenomenon.

* Jokes:

    * There's plenty of jokes related to how, in IT, a lot of what we do to solve problems, is just turn things off and on again.

### Intermittently Failing Script

* Tools that can be helpful:
    * **Zenity** is the application showing the window to select the date, title, and emails.

* Helpful pieces of advice and Terms:
    * In general, when dates are involved in a failure, the problem is due to **how the dates are formatted**.

### What is binary search?

* **Linear search** example: 
    1. Start from the first entry 
    1. Check if the name is the one that we're looking for
    1. If it doesn't match, move to the second element 
    1. Check again
    1. Keep going until we find the employee with the name we're looking for, *or we get to the end of the list*

* **Binary search** example
(*because the list is sorted, we can make decisions about the position of the elements in the list*):

    1. Compare the name that we're looking for with the element in the middle of the list
    1. Check if it's equal, smaller, or bigger
    1. Eliminated half of the list
    1. Do the same thing again until we find the element

* Helpful pieces of advice and Terms:

    * Usually when trying to find the root cause of a problem, we'll be looking for one answer in a list of many.

### Applying Binary Search in Troubleshooting

* Commands:

    *  **git bisect** - finds the commit that broke the program/process and report this as a bug to be fixed.

* Helpful pieces of advice and Terms:

    * **Bisecting** means dividing in two.

    * **Bisect** receives two points in time in the Git history and repeatedly lets us try the code at the middle point between them until we find the commit that caused the breakage.

## Slowness

### Why is my computer slow?

* Diagnosing what's causing your computer to run slow:
    * open one of these tools.
    * check out what's going on.
    * understand which resources the bottleneck and why.
    * plan how you're going to solve the issue.

* The **bottleneck** could be:
    * CPU time 
    * time spent reading data from disk waiting for data transmitted over the network
    * moving data from disk to RAM
    * some other resource that's limiting the overall performance.

* If the problem is:

    * **your program needs more CPU time** - close other running programs that you don't need. 

    * **you don't have enough space on disk** - uninstall, delete or move applications or data you don't need. 

    * **the application needs more network bandwidth** - stop any other processes that are using the network.

    * **the hardware we're using just isn't good enough**  - upgrade the underlying hardware.

* Hardware that needs to be changed can be: 
    * the CPU
    * the memory
    * the disk IO
    * the network connection
    * the graphics card

* Tools that can be helpful:

    * **top** -  lets us see which currently running processes are using the most CPU time on Linux systems.

    * **iotop** and **iftop** help us see which processes are currently using the most disk IO usage or the most network bandwidth. 

    * **Activity Monitor** - lets us see what's using the most CPU, memory, energy, disk, or network on MacOS. 

    * **Resource Monitor** and **Performance Monitor** -  let us analyze what's going on with the different resources on the computer including CPU, memory, disk and network on Windows

* Helpful pieces of advice and Terms:

    * Each application gets a fraction of the CPU time, and then the next application gets a turn.

    * Strategy for addressing slowness is to **identify the bottleneck** for addressing slowness in our device, our script, or our system to run slowly.

    * To tell which piece of hardware needs to be changed we need to **monitor the usage of our resources** to know which of them as being exhausted.

    * Sometimes, we need to figure out what the **software** is doing wrong and where it's spending most of its time to understand how to make it run faster. 

### How Computers Use Resources

* The time spent retrieving data depends on where it's located:
    * CPU
    * Disk
    * Memory
    * Network

* Examples of caches in IT:
    * A web proxy, *stores websites, images, or videos that are accessed often by users behind the proxy*.  * DNS services, *implement a local cache for the websites they resolve*.
    * The operating system, *takes care of some caching for us*.
    * The contents of files or libraries that are accessed often, *even if they aren't in use right now* are **the contents are cached** in memory.

* After running out of RAM:
    * The OS will just remove from RAM anything that's cached, *but not strictly necessary*.
    * The operating system will put the parts of the memory that aren't currently in use onto the hard drive in a space called **swap**.

* The machine is slow because:
    * There are too many open applications
    * The available memory is just too small for the amount that computer is using
    * The running programs may have a memory leak, *causing it to take all the available memory*. 
    
* Helpful pieces of advice and Terms:   
    * A **memory leak** means that memory which is no longer needed is not getting released.
    * **Cache** stores data in a form that's faster to access than its original form.
    * **The swapping implementation's concept** is The information that's not needed right now is removed from RAM and put onto the disk, while the information that's needed now is put into RAM.
    * If a program is using a lot of memory and this stops when you restart the program, it's probably because of a **memory leak**.

### Possible Causes of Slowness

* Slowness:
    * Too many applications configured to start on boot.
    * A program that's keeping some state while running that's causing the computer to slow down.
    * Hardware failures
    * Malicious software

* Helpful pieces of advice and Terms: 

    * If it's slow when starting up, it's probably a sign that **there are too many applications configured to start on boot**.

    * If instead the computer becomes sluggish after days of running just fine, and the problem goes away with a reboot, it means that **there's a program that's keeping some state while running** that's causing the computer to slow down.

    * Solution: schedule a regular restart to mitigate both the slow program and your computer running out of RAM. Reduce the size of the files involved. If the file is a log file, you can use a program like **logrotate**.

    * If your hard drive has errors, the computer might still be able to apply error correction to get the data that it needs, but it will affect the overall performance.

### Slow Web Server

a tool called ab which stands for Apache Benchmark tool to figure out how slow it is. We'll run ab -n 500 to get the average timing of 500 requests, and then pass our site.example.com for the measurement.

the load average on Linux shows how much time the processor is busy at a given minute with one meaning it was busy for the whole minute.

The process priorities in Linux are so that the lower the number, the higher the priority

Typical numbers go from 0 to 19. 
But we can change that using the nice and renice commands

For that, we'll use the pidof command that receives the process name and returns all the process IDs that have that name.

Renice takes the new priority as the first argument, and the process ID to change as the second one. I

for pid in $(pidof ffmpeg); do renice 19 $pid; 

We'll call ps ax which shows us all the running processes on the computer, and we'll connect the output of the command to less, to be able to scroll through it. 
ps ax | less

the locate command to see if we can find them. We'll first exit the less interface with queue and then call locate static/001.webm.

a tool called Daemonize that runs each program separately as if it were a daemon

So we'll use the killall command with the -STOP flag which sends a stop signal but doesn't kill the processes completely

for pid in $(pidof ffmpeg); do while kill - CONT $pid; do sleep 1; done; done

### Writing Efficient Code

we should always start by writing clear code that does what it should and only try to make it faster if we realize that it's not fast enough.

trying to optimize every second out of a script is probably not worth your time.

If we want our code to finish faster, we need to make our computer do less work

to avoid doing work that isn't really needed. How? There's a bunch of different things to do. The most common ones include storing data that was already calculated to avoid calculating it again using the right data structures for the problem and reorganizing the code so that the computer can stay busy while waiting for information from slow sources like disk or over the network.

. A profiler is a tool that measures the resources that our code is using, giving us a better understanding of what's going on

In particular, they help us see how the memory is allocated and how the time spent

So we would use gprof to analyze a C program but use the c-Profile module to analyze a Python program. 

The cProfile module is used to count how many times functions are called, and how long they run.

### Using the Right Data Structures

if you need to access elements by position or will always iterate through all the elements, use a list to store them.

if we need to look up the elements using a key, we'll use a dictionary.

Another thing that we might want to think twice about is creating copies of the structures that we have in memory.

### Expensive Loops

If you do an expensive operation inside a loop, you multiply the time it takes to do the expensive operation by the amount of times you repeat the loop.

how you access the data inside the loop. If the data is stored in a file, your script will need to parse the file to fetch it. 

Whenever you have a loop in your code, make sure to check what actions you're doing, and see if there are operations you can take out of the loop to do them just once
Instead of making one network call for each element, make one call before the loop. Instead of reading from disk for each element, read the whole thing before the loop. 

Make sure that the list of elements that you're iterating through is only as long as you really need it to be. 

Instead, you could modify the service to store the user access info in log files that can be read if necessary and only keep the last five logins in memory

Another thing to remember about loops is to break out of the loop once you found what you were looking for.

### Keeping Local Results

if we have to parse a file, we do it once before we call the loop instead of doing it for each element of the loop. 

If the script gets executed fairly regularly, it's common to create a local cache.

We need to think about how often we're going to update the cache and what happens if the data in the cache is out of date.

you'll want to look for strategies that let you avoid doing expensive operations. First, check if these operations are needed at all. If they are, see if you can store the intermediate results to avoid repeating the expensive operation more than needed.

### Slow Script with Expensive Loop

We'll measure the script speed using the time command

When we call time it runs the command that we pass to it and prints how long it took to execute it. There's three different values. Real, user, and sys. 

Real is the amount of actual time that it took to execute the command

This value is sometimes called wall-clock time because it's how much time a clock hanging on the wall would measure no matter what the computer's doing.

User is the time spent doing operations in the user space. 

Sys is the time spent doing system level operations.

The values of user and sys won't necessarily add up to the value of real because the computer might be busy with other processes.

we'll use the one called pprofile 3. We use the dash f flag to tell it to use the call grind file format and the dash o flag to tell it to store the output in the profile dot out file.

pprofile3 -f callgrind -o profile.out ./program.py

We're going to use kcachegrind to look at the contents, which is a graphical interface for looking into these files.

kcachegrind profile.out

### Parallelizing Operations

 do operations in parallel. That way, while the computer is waiting for the slow IO, other work can take place

 There's actually a whole field of computer science called concurrency, dedicated to how we write programs that do operations in parallel.


Our OS handles the many processes that run on our computer. If a computer has more than one core, the operating system can decide which processes get executed on which core, and no matter the split between cores, all of these processes will be executing in parallel.

Instead, you could split the list of computers into smaller groups and use the OS to call the script many times once for each group. That way, the connections to the different computers can be started in parallel, which minimizes the time but the CPU isn't doing anything.

Another easy thing to do, is to have a good balance of different workloads that you run on a computer.

Threads let us run parallel tasks inside a process.

In Python, we can use the Threading or AsyncIO modules to do this. These modules let us specify which parts of the code we want to run in separate threads or as separate asynchronous events, and how we want the results of each to be combined in the end. 

your script is CPU bound. In this case, you'll definitely want to split your execution across processors

A script is CPU bound if you're running operations in parallel using all available CPU time.

### Slowly Growing in Complexity

store your data in a SQLite file. This is a lightweight database system that lets you query the information stored in the file without needing to run a database server. 

we've gone from hosting the data in a CSV file to having it in a SQLite file then moving it to a database server and finally using a dynamic casher in front of the database server to make it run even faster.

A similar progression can happen on the user facing side of the same project. Initially, we set the Santa service would simply send emails to the people on the list. That's fine if it's a small group and there's one person in charge of the script. But as the project grows more complex, you'd want to have a website for the service to let people do things like check who their assigned person is and create wish lists. Initially, this could just be running on a web server on the same machine as the data. If the website gets used a lot, you might need to add a caching service like Varnish. This would speed up the load of dynamically created pages. And eventually, this still might not be enough. So you need to distribute your service across many different computers and use a load balancer to distribute the requests. You could do this in-house with separate computers hosted at your company, but this means that as the application keeps growing you need to add more and more servers. It might be easier to use virtual machines running in the cloud that can be added or removed as the load sustained by the service changes.

### Dealing with Complex Slow Systems

What do you do if your complex system is slow? As usual, what you want to do is find the bottleneck that's causing your infrastructure to underperform. Is it the generation of dynamic pages on the web server? Is it the queries to the database? Is it doing the calculations for the fulfillment process?

When a database server needs to find data, it can do it much faster if there's an index on the field that you're querying for. On the flip side, if the database has too many indexes, adding or modifying entries can become really slow because all of the indexes need updating.

Now what if when you try to figure out why the service is slow, you see that the CPU on the web serving machine is saturated. The first step is to check if the code of the service can be improved using the techniques that we explained earlier. 

You got it! The local disk I/O latency is causing the application to wait too long for data from disk.

### Using Threads to Make Things Go Faster

importing the futures sub module, which is part of the concurrent module. This gives us a very simple way of using Python threads.

from concurrent import futures

executor = futures.ThreadPoolExecutor()

executor.submit("name of the function", params)

print('Waiting for all threads to finish.')
executor.shutdown

==================

executor = futures.ProcessPoolExecutor()

an executor. This is the process that's in charge of distributing the work among the different workers.

The futures module provides a couple of different executors, one for using threads and another for using processes

So we'll add a message saying that we're waiting for all threads to finish, and then call the shutdown function on the executor. This function waits until all the workers in the pool are done, and only then shuts down the executor.

You nailed it! Memchached is a caching service that keeps most commonly accessed database queries in RAM.

Asyncio is a module that lets you specify parts of the code to run as separate asynchronous events.