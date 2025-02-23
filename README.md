[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)
# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name: Chen Chufan
### Student Id: 21108764
### Email: cchendd@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) (1) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). (2) Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. (3) Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

    > Your answer goes here.
    >
    > **(1) Measurement Tool**: **Phoronix Test Suite**[https://www.phoronix-test-suite.com/]
    >
    > ​	Install Phoronix
    >
    > ```
    > wget https://phoronix-test-suite.com/releases/repo/pts.debian/files/phoronix-test-suite_10.8.4_all.deb
    > sudo dpkg -i phoronix*.deb
    > ```
    >
    > **(2) Configuration:** 
    >
    > - **CPU Test**: 
    >   - use the `pts/compress-7zip` test, which evaluates CPU performance using the `7zip` compression algorithm.
    >   - Configuration: Default thread count matches the number of `vCPUs` of the instance.
    >   - Reason: Thread count matches the number of vCPUs to fully utilize the instance's computing power.
    > - **Memory Test**: 
    >   - use the `pts/ramspeed` test, which evaluates memory performance through read/write operations.
    >   - Configuration: Set memory block size to 1MB and total operation size to 1GB.
    >   - Reason: Memory block size and total operation size are set moderately to avoid distortion caused by insufficient or excessive resources.
    >   - <img src="C:\Users\86181\AppData\Roaming\Typora\typora-user-images\image-20250223150541470.png" alt="image-20250223150541470" style="zoom:50%;" />
    >
    > **(3) Results Explanation:**
    >
    > - **CPU Test: **The result is represented by the amount of compressed data processed per second (unit: KB/s). Higher values indicate better performance.
    > - **Memory Test:** The result is represented in MB/s. Higher values indicate better performance.
    >
    > 

2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size        | CPU performance | Memory performance |
    | ----------- | --------------- | ------------------ |
    | `t2.micro` | 3613 MIPS(Compression)<br />3071 MIPS(Decompression) | 11083.45 MB/s |
    | `t2.medium`  | 9836 MIPS(Compression)<br />5925 MIPS(Decompression) | 20042.80 MB/s |
    | `c5d.large` | 7813 MIPS(Compression)<br />5265 MIPS(Decompression) | 13899.22 MB/s |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.
    >
    > **Answer:**
    >
    > - ***The performance of EC2 instances improves with the increase in vCPUs and memory resources, but not in a perfectly linear manner.***
    > - ***The performance differences between different instance types are significantly influenced by the instance architecture and optimization direction. For example, `c5d.large` is more suitable for compute-intensive tasks, but in some scenarios (e.g., compression tasks), its performance may be inferior to that of `t2.medium`.***
    >
    > 
    >
    > **Experiment Steps**: 
    >
    > **(1) Create 3 instances `t2.micro`, `t2.medium`, and `c5d.large` Linux instances**
    >
    > - `t2.micro`（1 vCPU，1GB Memory）
    > - `t2.medium`（2 vCPU，4GB Memory）
    > - `c5d.large`（2 vCPU，4GB Memory）
    >
    > **(2) Measure the EC2 CPU and Memory performance using instructions below:**
    
    > ```
    > sudo apt-get update
    > sudo apt --fix-broken install
    > sudo apt-get install php-cli php-xml
    > 
    > 
    > # Step 1: Install Phoronix Test Suite
    > wget https://phoronix-test-suite.com/releases/repo/pts.debian/files/phoronix-test-suite_10.8.4_all.deb
    > sudo dpkg -i phoronix*.deb
    > phoronix-test-suite version（for checking installing）
    > 
    > # Step 2: Run CPU Test
    > sudo apt-get install php-zip
    > phoronix-test-suite run pts/compress-7zip
    > 
    > # Step 3: Run Memory Test
    > phoronix-test-suite run pts/ramspeed
    > ```
    >
    > **(3) Result**
    >
    > ```
    > t2.micro
    > Test: Compression Rating:
    >         3565
    >         3669
    >         3605
    > 
    >     Average: 3613 MIPS
    >     Deviation: 1.45%
    > Test: Decompression Rating:
    >         3053
    >         3084
    >         3076
    > 
    >     Average: 3071 MIPS
    >     Deviation: 0.52%
    > Type: Copy - Benchmark: Integer:
    >         11095.62
    >         11109.48
    >         11045.24
    > 
    >     Average: 11083.45 MB/s
    >     Deviation: 0.31%
    > 
    > t2.medium
    > Test: Compression Rating:
    >         9723
    >         10058
    >         9726
    > 
    >     Average: 9836 MIPS
    >     Deviation: 1.96%
    > Test: Decompression Rating:
    >         5890
    >         5977
    >         5908
    > 
    >     Average: 5925 MIPS
    >     Deviation: 0.78%
    > Type: Copy - Benchmark: Integer:
    >         20079.61
    >         19902.93
    >         20145.87
    > 
    >     Average: 20042.80 MB/s
    >     Deviation: 0.63%
    > 
    > c5d.large
    > 
    >  Test: Compression Rating:
    >         7807
    >         7803
    >         7830
    > 
    >     Average: 7813 MIPS
    >     Deviation: 0.19%
    > Test: Decompression Rating:
    >         5256
    >         5241
    >         5298
    > 
    >     Average: 5265 MIPS
    >     Deviation: 0.56%
    > Type: Copy - Benchmark: Integer:
    >         13864.44
    >         13955.19
    >         13878.03
    > 
    >     Average: 13899.22 MB/s
    >     Deviation: 0.35%
    > ```
    >
    > 

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type(client->server)      | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | `t3.medium` - `t3.medium` | 3.62 Gbits/sec | 0.300 ms |
    | `m5.large` - `m5.large`   | 9.48 Gbits/sec | 0.182 ms |
    | `c5n.large` - `c5n.large` | 4.96 Gbits/sec | 0.130 ms |
    | `t3.medium` - `c5n.large` | 2.35 Gbits/sec | 0.687 ms |
    | `m5.large` - `c5n.large`  | 2.46 Gbits/sec | 0.811 ms |
    | `m5.large` - `t3.medium`  | 3.74 Gbits/sec | 0.294 ms |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.
    >
    > **Answer:**
    >
    > ***The network performance between instances of the same type is better than that between different types; compute-optimized instances perform better in terms of bandwidth and latency.***
    >
    > 
    >
    > **Experiment Steps:**
    >
    > (1) Install
    >
    > ```
    > sudo apt-get update
    > sudo apt-get install iperf
    > ```
    >
    > (2) Test network performance in the same area
    >
    > ```
    > server-client
    > AWS-instances-security-group--inbound rules--All trafic 0.0.0.0
    > 
    > (1) TCP
    > server:		iperf -s -w 256k
    > client:		iperf -c <server private IP> -w 256k
    > iperf -c 172.31.47.197 -w 256k
    > (2)RTT
    > client: 	ping <server private IP>
    > ping 172.31.12.34
    > ```
    >
    > (3) Result
    >
    > ```
    > `t3.medium` - `t3.medium`
    > [  1] local 172.31.39.65 port 46748 connected with 172.31.47.197 port 5001 (icwnd/mss/irtt=87/8949/272)
    > [ ID] Interval       Transfer     Bandwidth
    > [  1] 0.0000-10.0180 sec  4.23 GBytes  3.62 Gbits/sec
    > 
    > `m5.large` - `m5.large`
    > [  1] local 172.31.41.44 port 37752 connected with 172.31.33.173 port 5001
    > [ ID] Interval       Transfer     Bandwidth
    > [  1] 0.0000-10.0134 sec  11.0 GBytes  9.48 Gbits/sec
    > 
    > `c5n.large` - `c5n.large`
    > [  1] local 172.31.12.34 port 5001 connected with 172.31.3.123 port 40880 (icwnd/mss/irtt=87/8949/373)
    > [ ID] Interval       Transfer     Bandwidth
    > [  1] 0.0000-10.0003 sec  5.77 GBytes  4.96 Gbits/sec
    > 
    > `t3.medium` - `c5n.large`
    > [  1] local 172.31.47.197 port 57596 connected with 172.31.12.34 port 5001
    > [ ID] Interval       Transfer     Bandwidth
    > [  1] 0.0000-10.0129 sec  2.74 GBytes  2.35 Gbits/sec
    > 
    > `m5.large` - `c5n.large`
    > [  1] local 172.31.41.44 port 51896 connected with 172.31.12.34 port 5001
    > [ ID] Interval       Transfer     Bandwidth
    > [  1] 0.0000-10.0126 sec  2.87 GBytes  2.46 Gbits/sec
    > 
    > `m5.large` - `t3.medium`
    > [  1] local 172.31.41.44 port 34188 connected with 172.31.47.197 port 5001
    > [ ID] Interval       Transfer     Bandwidth
    > [  1] 0.0000-10.0038 sec  4.35 GBytes  3.74 Gbits/sec
    > ```
    >
    > 

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection(client->server) | TCP b/w (Mbps) | RTT (ms) |
    | -------------------------- | -------------- | -------- |
    | N. Virginia - Oregon       | 33.6 Mbits/sec | 63.6 ms  |
    | N. Virginia - N. Virginia  | 4.18 Gbits/sec | 0.256 ms |
    | Oregon - Oregon            | 4.78 Gbits/sec | 0.134 ms |

    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.
    >
    > **Answer:**
    >
    > ***The network performance between instances in different regions is significantly lower than that of instances within the same region; cross-region connections are limited by internet transmission, while intra-region connections leverage AWS's internal network for high performance.***
    >
    > 
    >
    > (3) Test network performance in different areas
    >
    > ```
    > every region run
    > 
    > sudo apt-get update
    > sudo apt-get install iperf
    > 
    > (1) TCP
    > iperf -s -w 256k
    > iperf -c <public IP> -w 256k
    > iperf -c 54.236.21.181 -w 256k
    > (2) RTT
    > ping 54.236.21.181
    > ```
    >
    > (4) Results
    >
    > ```
    > N. Virginia - Oregon
    > [  1] local 172.31.29.223 port 56542 connected with 35.93.27.10 port 5001
    > [ ID] Interval       Transfer     Bandwidth
    > [  1] 0.0000-10.0822 sec  40.4 MBytes  33.6 Mbits/sec
    > 
    > 
    > N. Virginia - N. Virginia
    > [  1] local 172.31.29.223 port 52518 connected with 54.236.21.181 port 5001
    > [ ID] Interval       Transfer     Bandwidth
    > [  1] 0.0000-10.0037 sec  4.87 GBytes  4.18 Gbits/sec
    > 
    > 
    > Oregon - Oregon
    > [  1] local 172.31.19.85 port 35996 connected with 35.93.27.10 port 5001 (icwnd/mss/irtt=14/1448/131)
    > [ ID] Interval       Transfer     Bandwidth
    > [  1] 0.0000-10.0052 sec  5.57 GBytes  4.78 Gbits/sec
    > ```
    >
    > 



