## Solution

``` java

import java.util.*;
//*
class Job {
int start;
int end;
int cpuLoad;

public Job(int start, int end, int cpuLoad) {
	this.start = start;
	this.end = end;
	this.cpuLoad = cpuLoad;
}
};
*//

class Solution {

public static int findMaxCPULoad(List<Job> jobs)
{
	
	// sort the jobs by start time
	Collections.sort(jobs, (a, b) -> Integer.compare(a.start, b.start));

	int maxCPULoad = 0;
	int currentCPULoad = 0;
	PriorityQueue<Job> minHeap = new PriorityQueue<>(jobs.size(), 
													(a, b) -> Integer.compare(a.end, b.end));
	for (Job job : jobs) {
	// remove all jobs that have ended
	while (!minHeap.isEmpty() && job.start > minHeap.peek().end)
		currentCPULoad -= minHeap.poll().cpuLoad;

	// add the current job into the minHeap
	minHeap.offer(job);
	currentCPULoad += job.cpuLoad;
	maxCPULoad = Math.max(maxCPULoad, currentCPULoad);
	}
	return maxCPULoad;
}



```




Here's a breakdown of the algorithm:

1. **Job Class**: Represents a job with attributes `start`, `end`, and `cpuLoad`.

2. **Sorting Jobs**: The jobs are initially sorted by their start time. This is crucial for processing jobs in the order they begin.

3. **Priority Queue (Min Heap)**: A priority queue (min heap) is used to keep track of ongoing jobs. It's sorted by the end time of the jobs, so the job that finishes earliest is at the front of the queue.

4. **Processing Jobs**:
   - For each job in the sorted list:
     - Remove jobs from the priority queue that have ended before the current job starts. This is done by comparing the start time of the current job with the end time of the job at the front of the queue.
     - Update `currentCPULoad` by subtracting the CPU load of jobs removed from the queue.
     - Add the current job to the priority queue and update `currentCPULoad` by adding the CPU load of the current job.
     - Update `maxCPULoad` if `currentCPULoad` is greater than the current `maxCPULoad`.

5. **Result**: The `maxCPULoad` variable holds the maximum CPU load at any point in time across all jobs.

This approach efficiently calculates the maximum CPU load without checking every possible time point. By using a priority queue, it minimizes the work required to determine which job ends next, allowing the algorithm to focus only on times when jobs start or end. This makes the algorithm more efficient, especially when dealing with a large number of jobs.
