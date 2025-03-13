Cristobal Benavides

# Project 1: Bitcoin Mining with Apache Spark


## **Question 1: Results**

for this spesific question instead of doing i used “this_is_a_bitcoin_block_of_12074051” instead of this_is_a_bitcoin_block_of_12074051 by mistake for the string

### **Local Execution Results**
| k  | xS (Concatenated Nonce + String) | Hash Value | Time Elapsed | Number of Trials |
|----|----------------------------------|------------|--------------|------------------|
| 2  | 1469925134this_is_a_bitcoin_block_of_12074051 | 001c808eb0354863de2e22c04f159d5648d704eb15eff887a259ab748157d83c | 1s | 200 |
| 3  | 1439277610this_is_a_bitcoin_block_of_12074051 | 000cf496c5c8a95a1dee35000ff216a59e6276c42fea56f8238bc4a6c3cf0aad | 1s | 3000 |
| 5  | 1245823906this_is_a_bitcoin_block_of_12074051 | 00000d600788035d3e0d4f59189e1c656afc1468a46da522d77e08da245f7451 | 1s | 150000 |
| 6  | 631213605this_is_a_bitcoin_block_of_12074051 | 0000005354ee65849570f22fcfbe632449be8764ed3a8b66d360dd20de3ab6c5 | 9s | 20,000,000 |

---

### **Q2 Google Cloud Platform (GCP) Execution Results (k = 7)**

| k  | xS (Concatenated Nonce + String) | Hash Value | Time Elapsed | Number of Trials |
|----|----------------------------------|------------|--------------|------------------|
| 7  | 726766127your_is_a_bitcoin_12074051 | 00000003c4f6f6ce8140ebde6f7e487f0058850b4738738afaf726fbe5730cb4 | 811s | 280,000,000 |

### **Cluster Configuration**
- **Cluster Type:** Single Node (1 master, 0 workers)
- **Machine Type:** `n2-standard-4` (4 vCPUs, 16GB RAM)
 - **Persistent Disk Size:** 100GB
- **Spark Version:** 3.5.4 with Hadoop 3.3
- **Storage Bucket:** `gs://benavicb-csci3390-bucket`

 ### **Trial Estimation for k = 7**
The number of trials required to find a valid nonce was estimated based on probability theory.

#### **Calculation Method:**
The probability of finding a valid nonce is determined by:
\[
\frac{1}{16^k}
\]
where `k` is number of leading zeros required.

For **k = 7**, this gives:
\[
\frac{1}{16^7} = \frac{1}{268435456} \approx 3.7 \times 10^{-9}
\]
hence **one valid nonce is expected every ~268 million trials**.

 **i ran 280,000,000 trials** to slightly overshoot the estimated need.


## Q3. Modifying the Code to Generate Nonce Sequentially**
### **Code Modification**
By default, the code selects **random nonces** within the range `[1, n]`. Instead, we modified the program so that it generates **sequential** nonces from `1` to `n`. 

#### **Modified Code:**
val nonce = sc.range(1, trials + 1) 

this Generates nonces from 1 to n sequentially
This replaces the previous method, which used a random number generator

### **Efficiency Discussion**
#### **Advantages of Sequential Approach:**
- **Deterministic**: Ensures no duplicates and systematically explores the search space.
- **Even Work Distribution**: Partitions are evenly distributed among Spark nodes.
- **Better for Small `k` Values**: For low difficulty levels, sequential search can find solutions faster.

#### **Disadvantages of Sequential Approach:**
- **No Early Stopping**: Unlike random sampling, the search continues through all numbers.
- **Not Efficient for Large `k`**: When `k` is large, random search may reduce time by finding a valid nonce sooner.
