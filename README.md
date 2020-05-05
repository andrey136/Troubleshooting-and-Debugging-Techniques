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



* Tools that can be helpful:



* Helpful pieces of advice and Terms:

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
        1. Do the same thing again until we find the element.

    * Usually when trying to find the root cause of a problem, we'll be looking for one answer in a list of many.

