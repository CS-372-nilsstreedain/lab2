# Lab 2
1. What is the IP address and TCP port number used by the client computer (source) that is transferring the file to gaia.cs.umass.edu? To answer this question, it’s probably easiest to select an HTTP message and explore the details of the TCP packet used to carry this HTTP message, using the “details of the selected packet header window” (refer to Figure 2 in the “Getting Started with Wireshark” Lab if you’re uncertain about the Wireshark windows.
```
192.168.1.102:1161
```

2. What is the IP address of gaia.cs.umass.edu? On what port number is it sending and receiving TCP segments for this connection?
```
128.119.245.12:80
```

3. What is the IP address and TCP port number used by your client computer (source) to transfer the file to gaia.cs.umass.edu?
```
10.3.8.52
```

4. What is the sequence number of the TCP SYN segment that is used to initiate the TCP connection between the client computer and gaia.cs.umass.edu? What is it in the segment that identifies the segment as a SYN segment?
```
0
```

5. What is the sequence number of the SYNACK segment sent by gaia.cs.umass.edu to the client computer in reply to the SYN? What is the value of the Acknowledgement field in the SYNACK segment? How did gaia.cs.umass.edu determine that value? What is it in the segment that identifies the segment as a SYNACK segment?
```
- 0
- 1
- The acknowledgement number is the next segment being requested, determined by
  increasing the previous sequence number of SYN by 1.
- The SYN & ACK flag are set to 1 aka true, setting the segment to SYNACK.
```

6. What is the sequence number of the TCP segment containing the HTTP POST command? Note that in order to find the POST command, you’ll need to dig into the packet content field at the bottom of the Wireshark window, looking for a segment with a “POST” within its DATA field.
```
Sequence number: 1
```

7. Consider the TCP segment containing the HTTP POST as the first segment in the TCP connection. What are the sequence numbers of the first six segments in the TCP connection (including the segment containing the HTTP POST)? At what time was each segment sent? When was the ACK for each segment received? Given the difference between when each TCP segment was sent, and when its acknowledgement was received, what is the RTT value for each of the six segments? What is the EstimatedRTT value (see Section 3.5.3, page 242 in text) after the receipt of each ACK? Assume that the value of the EstimatedRTT is equal to the measured RTT for the first segment, and then is computed using the EstimatedRTT equation on page 242 for all subsequent segments. Note: Wireshark has a nice feature that allows you to plot the RTT for each of the TCP segments sent. Select a TCP segment in the “listing of captured packets” window that is being sent from the client to the gaia.cs.umass.edu server. Then select: Statistics->TCP Stream Graph- >Round Trip Time Graph.

Segment # |	ACK Segment # |	Sequence # |	Time |	ACK Time |	RTT |	EstimatedRTT
-|-|-|-|-|-|-
4 |	6 |	1 |	0.026477 |	0.053937 |	0.02746 |	0.02746
5 |	9 |	566 |	0.041737 |	0.077294 |	0.035557 |	0.0285
7 |	12 |	2026 |	0.054026 |	0.124085 |	0.070059 |	0.0337
8 |	14 |	3486 |	0.054690 |	0.169118 |	0.11443 |	0.0438
10 |	15 |	4946 |	0.077405 |	0.217299 |	0.13989 |	0.0558
11 |	16 |	6406 |	0.078157 |	0.267802 |	0.18964 |	0.0725

8. What is the length of each of the first six TCP segments?

Segment # |	4 |	5 |	7 |	8 |	10 |	11
-|-|-|-|-|-|-
Length (bytes) |	565 |	1460 |	1460 |	1460 |	1460 |	1460

9. What is the minimum amount of available buffer space advertised at the received for the entire trace? Does the lack of receiver buffer space ever throttle the sender?
```
The minimum amount of available buffer space advertised is 5840 bytes. The
sender is never throttled because of a lack of receiver buffer space.
```

10. Are there any retransmitted segments in the trace file? What did you check for (in the trace) in order to answer this question?
```
No, all the sequence numbers are increasing with respect to time. If there was a
retransmitted segment, the sequence number would be less than the segment before
it.
```

11. How much data does the receiver typically acknowledge in an ACK? Can you identify cases where the receiver is ACKing every other received segment (see Table 3.2 on page 250 in the text).
```
For ACKs (2-6 & 8-12), it is 1460 bytes, therefore typically the acknowledged
data is 1460 bytes. There are cases where the receiver is ACKing every other
received segment, for example segment 80 where the acknowledged data is double
the typical amount.
```

12. What is the throughput (bytes transferred per unit time) for the TCP connection? Explain how you calculated this value.
```
Total bytes = 164091 - 1 = 164090
Total time = 5.455830 - 0.026477 = 5.4294

Throughput = 30,222.49 bytes/sec
		= 30.22 KB/s
```

13. Use the Time-Sequence-Graph(Stevens) plotting tool to view the sequence number versus time plot of segments being sent from the client to the gaia.cs.umass.edu server. Can you identify where TCP’s slowstart phase begins and ends, and where congestion avoidance takes over? Comment on ways in which the measured data differs from the idealized behavior of TCP that we’ve studied in the text.
```
TCP Slow Start takes place at the beginning of the communication when the
segment containing the HTTP POST is sent and ends at roughly 0.1242s. After
that, congestion takes over (shown by the vertical lines). This differs from
what was shown in the text, in that the graph is much less uniform, with more
variability.
```

14. Answer each of two questions above for the trace that you have gathered when you transferred a file from your computer to gaia.cs.umass.edu
```
On my computer, I see a very small exponential increase between t = .87s & .88s.
After that congestion avoidance takes over (shown by there vertical lines).
Specifically with the graph coming from my computer, it is apparent how when
multiple packets are sent during the congestion avoidance intervals, they take
uneven amounts of time and appear skewed on the graph.
```
