# csc458pa2

## Instructions:

To reproduce the results, run a single shell command "sudo ./run.sh" within the folder. This will generate 3 plots each for the scenario
buffer size = 20 and 100. The average and standard deviation of the webpage fetch time is written to fetch_results.txt.

## Questions:

1. Why do you see a difference in webpage fetch times with small and large router buffers?

From our experiment we can see that larger router buffers corresponds with longer webpage fetch time. In one run of our experiment,
a router with a buffer size of 100 resulted in an average webpage fetch time of 0.110 seconds while a router with a buffer size of 100 
results in just 0.504 seconds. The larger buffer size will cause more parts of the webpage content to be stuck in the buffer, rather 
than being dropped by the network and resent by the server, thus leading to longer webpage fetch time.

2. Bufferbloat can occur in other places such as your network interface card (NIC). Check the output of ifconfig eth0 on your VirtualBox 
VM. What is the (maximum) transmit queue length on the network interface reported by ifconfig? For this queue size and a draining rate 
of 100Mbps, what is the maximum time a packet might wait in the queue before it leaves the NIC?

The maximum transmit queue length on the network interface is 1000. When the queue is full, it will hold 1000 * 1500 bytes of data.
Assuming the queue is drained at 100Mbps, a packet might wait up to (1000 * 1500 * 8) / (100 * 10^6) = 0.12 seconds in the queue.

3. How does the RTT reported by ping vary with the queue size? Write a symbolic equation to describe the relation between the two
(ignore computation overheads in ping that might affect the final result).

The RTT reported by ping is direclty proportional to the queue size. We described the relationship with the following equation: 
RTT = propagation_delay * queue_size
If the propagation delay of the network remains constant, the RTT will grow in proportion with queue size.

4. Identify and describe two ways to mitigate the bufferbloat problem.

As seen from our experiment, we can mitigate the bufferbloat issue by lowering the buffer size to a reasonable amount. Smaller buffer
size will ensure that packets will be quickly dropped when the network is busy and resent when available rather than wait in the buffer
for extended periods of time. We can also modify our router to detect when the buffer is being used significantly and notify the sender.
Recall that the sender will continue to increase their congestion window size as long as the packets sent are not dropped and just 
waiting the buffer. Our messages back to the sender will promptly notify them of the bottleneck and prevent the sender from further
increasing their sending rate.
