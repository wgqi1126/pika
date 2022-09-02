#Performance pika vs ssdb

## Environment

Server：

    CPU: 24 Cores, Intel(R) Xeon(R) CPU E5-2630 v2 @ 2.60GHz
    MEM: 198319652 kB
    OS: CentOS release 6.2 (Final)
    NETWORK CARD: Intel Corporation I350 Gigabit Network Connection
 
## Command
set and get

## Solution

16 worker thread in Pika
./redis-benchmark -h ... -p ... -n 1000000000 -t set,get -r 10000000000 -c 120 -d 200

## Diagram
<img src="images/benchmarkVsSSDB01.png" height = "400" width = "480" alt="1">
<img src="images/benchmarkVsSSDB02.png" height = "400" width = "480" alt="10">

## Details

### Set：

	ssdb
        1000000000 requests completed in 25098.81 seconds
		0.52% <= 1 milliseconds 
		96.39% <= 2 milliseconds 
		97.10% <= 3 milliseconds 
		97.34% <= 4 milliseconds 
		97.57% <= 5 milliseconds
		…
		98.87% <= 38 milliseconds
		...
		99.99% <= 137 milliseconds
		100.00% <= 215 milliseconds
		...
		100.00% <= 375 milliseconds
		
		39842.53 requests per second
	

	
	
		
	pika
        1000000000 requests completed in 11890.80 seconds
		18.09% <= 1 milliseconds
		93.32% <= 2 milliseconds
		99.71% <= 3 milliseconds
		99.86% <= 4 milliseconds
		99.92% <= 5 milliseconds
		99.94% <= 6 milliseconds
		99.96% <= 7 milliseconds
		99.97% <= 8 milliseconds
		99.97% <= 9 milliseconds
		99.98% <= 10 milliseconds
		99.98% <= 11 milliseconds
		99.99% <= 12 milliseconds
		...
		100.00% <= 19 milliseconds
		...
		100.00% <= 137 milliseconds
		
		84098.66 requests per second
		
### Get：

	ssdb
		1000000000 requests completed in 12744.41 seconds
        7.32% <= 1 milliseconds
		96.08% <= 2 milliseconds
		99.49% <= 3 milliseconds
		99.97% <= 4 milliseconds
		99.99% <= 5 milliseconds
		100.00% <= 6 millisecondsj
		...
		100.00% <= 229 milliseconds
		
		78465.77 requests per second
	
	
	
	pika
        1000000000 requests completed in 9063.05 seconds
		84.97% <= 1 milliseconds
		99.76% <= 2 milliseconds
		99.99% <= 3 milliseconds
		100.00% <= 4 milliseconds
		...
		100.00% <= 33 milliseconds
		
		110338.10 requests per second
		
## Conclusion

Pika has 2.1 times write throughtput than ssdb（qps：84098 vs 39842）and 1.4 read throughput than ssdb（qps：110338 vs 78465）

Write latency in Pika is better than ssdb

Read latency in Pika is better than ssdb

Pika is better than ssdb

		