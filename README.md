# starvefree-Reader-writer-problem
# Problem Description
The reader-writer problem is a classical synchronization problem in computer science, which deals with concurrent access to shared resources by multiple processes or threads. The problem involves a shared data structure or file that can be accessed by both readers and writers.

The objective of the reader-writer problem is to ensure that multiple readers can access the shared data structure concurrently, while also ensuring that writers have exclusive access to the shared data structure. Furthermore, the solution must prevent starvation, i.e., it must ensure that both readers and writers get a fair chance to access the shared resource
# Intuition
The shared resources are allocated to writer or reader based on FCFS.We will block the process if the shared resources had been allocated to reader or writer then we block the process and to maintain process order we use a Queue and we have constraint that writer and reader cannot share same resources simultaneously to handle this i used "critical_section_mutex" semaphore.

# Reading Case
If a resource was in the reader access then we can give acess to immediate reader to read the file simultaneously.To maintain count ("read_ct") is used and "readct_mutex" to allow a single reader for upadating the value of read_ct to not to go into race condition and "exe" semaphore to which a process int queue to execute in an order.

# Writing Case
As we cant allow two or more writers in same memory location as they are updating information in shared memory resources so they have to perform in a sequence
# Semaphores Intialization
```
semaphore exe=1;// it is the semaphore to make wait the process till it turn conmes
semaphore readct_mutex=1;// semaphore to allow a single reader for upadating the value of read count
semaphore critical_section_mutex=1;// semaphore to allow reader or writer to critical section but not simultaneously
```
# Reader Pseudocode
```C++
long long int read_ct=0; //intialising count of current readers reading
wait(exe); //execute only when it turn come

wait(readct_mutex); //semaphore to allow a single reader for upadating the value of read count

read_ct++; //count of number of readers currently reading

if(read_ct==1)
{
    wait(critical_section_mutex); //semaphore to allow reader or writer to critical section but not simultaneously
}

signal readct_mutex;


/* Reading process */


wait(readct_mutex);

read_ct--;

if(read_ct==0)
{
    read_ct--;
}

signal(readct_mutex);
```

# writer pseudocode
```C++
wait(exe); //The writer has to wait for the entry semaphore it can get access to it even when readers are reading

wait(critical_section_mutex); // This is to make sure that the writer does not write when readers are reading

signal(exe);// So that the readers or writers can start queing when the writing process is being performed
 
 /*
 Writing Process
 */
 signal(critical_section_mutex)
 ```
 So i applied FIFO Queue to maintain order of sequence of arrival and execited in that order as every process executed in finite time in FCFS type so starvation will not occur
 # END


