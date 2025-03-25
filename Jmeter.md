# **JMeter Performance Test Report**

## **1. Introduction**
This report provides an analysis of the performance test conducted using JMeter on the API endpoint:
> **https://httpbin.org/delay/{seconds_to_delay}**

The `{seconds_to_delay}` value was randomly selected between **1 to 10 seconds** for each request. The test was executed under different load conditions, and the results have been analyzed.

## **2. Test Plan Setup**
### **2.1 JMeter Test Plan Configuration**
- **Thread Group**: Varies across test runs (e.g., 800, 1000, 1200, 1600 threads)
- **Ramp-Up Time**: Varies (e.g., 4, 5 seconds)
- **Loop Count**: 10
- **BeanShell Preprocessor**:  int random_seconds = 1 + (int)(Math.random() * 10);  // Generate random delay (1-10)
    vars.put("random_seconds", Integer.toString(random_seconds));  // Store in JMeter variable
- **API Call**: GET request to `https://httpbin.org/delay/{random_seconds}`
- **Listeners**: Summary Report, Response Time Graph, Hits Per Second, and Threads vs. Time

## **3. Performance Results**

### **3.1 Summary of Performance Metrics**
| Metric | Test Run 1 (800 Threads) | Test Run 2 (1000 Threads) | Test Run 3 (1200 Threads) | Test Run 4 (1600 Threads) |
|--------|----------------|----------------|----------------|----------------|
| **Sample Count** | 800 | 1000 | 1200 | 1600 |
| **Error Count** | 2 | 14 | 8 | 13 |
| **Error %** | 0.25% | 1.4% | 0.67% | 0.81% |
| **Mean Response Time (ms)** | 6721.70 | 6645.22 | 6758.33 | 7196.83 |
| **Median Response Time (ms)** | 6816.5 | 6560.5 | 6621.5 | 7092.5 |
| **Min Response Time (ms)** | 1156 | 365 | 473 | 295 |
| **Max Response Time (ms)** | 14274 | 13853 | 13796 | 19661 |
| **90th Percentile (ms)** | 10529.5 | 10621.8 | 10780.3 | 11297.5 |
| **95th Percentile (ms)** | 11046.2 | 11307.5 | 11319.8 | 12990.7 |
| **99th Percentile (ms)** | 12561.97 | 12694.32 | 12398.44 | 16685.66 |
| **Throughput (req/sec)** | 7.97 | 10.33 | 11.91 | 16.14 |
| **Received KB/sec** | 7.30 | 9.38 | 10.87 | 14.71 |
| **Sent KB/sec** | 3.26 | 4.23 | 4.87 | 6.60 |

## **4. Analysis of Results**
### **4.1 Key Observations**
1. **Response Time**: The mean response time increases as the number of threads increases. The highest response time was observed in the **1600-thread test (7196 ms mean response time)**.
2. **Error Rate**:  502 Bad Gateway errors The highest error rate (**1.4%**) was recorded in the **1000-thread test**, suggesting possible server throttling or timeout issues.
3. **Throughput**: The throughput increases with the number of threads but stabilizes around **16 req/sec** at **1600 threads**.
4. **Max Response Time**: The maximum response time **(19.6 sec)** in the highest load test indicates potential API bottlenecks.
5. **Bad Gateway Error**s: API struggled under concurrent requests, likely due to a server-side bottleneck.

## **5. Performance Graphs**
### **5.1 Response Time Over Time**
- **First case**: 
*![RSOT1](https://github.com/user-attachments/assets/df9e0a9b-53e8-4f59-9eca-4f10ae8563d0)
*
- **Second case**: 
*![RSOT2](https://github.com/user-attachments/assets/8a9d3f96-cc50-473c-ae05-59db939b07f6)
*
- **Third case**: 
*![RSOT3](https://github.com/user-attachments/assets/bb90431f-1b65-48b7-83b8-a34e116b3ea4)
*
- **Fourth case**: 
*![RSOT4](https://github.com/user-attachments/assets/f113a3ee-1fc4-44cd-b63d-a329bb96e2a0)
*

### **5.2 Hits Per Second**
- **First case**: 
*![HSP1](https://github.com/user-attachments/assets/d1b82eae-a28a-44e2-9287-e6d83ee4ef92)
*
- **Second case**: 
*![HSP2](https://github.com/user-attachments/assets/a2aaac2f-12de-47cc-9e3a-7070b43609f0)
*
- **Third case**: 
*![HSP3](https://github.com/user-attachments/assets/e26b6e55-2673-4dff-9388-06e09046d150)
*
- **Fourth case**: 
*![HSP4](https://github.com/user-attachments/assets/e0c979bf-5f00-47dd-a9e8-8853f4a605e7)
*

### **5.3 Time vs. Threads**
- **First case**: 
*![TVT1](https://github.com/user-attachments/assets/0557d97b-16a6-4374-bbd5-d7cf51120f5b)
*
- **Second case**: 
*![TVT2](https://github.com/user-attachments/assets/f1bcb8d9-f564-441a-af38-848ea8b07b05)
*
- **Third case**: 
*![TVT3](https://github.com/user-attachments/assets/f1f9fe66-38c9-4cd5-809a-1277e9425ace)
*
- **Fourth case**: 
*![TVT4](https://github.com/user-attachments/assets/b55e5e51-1320-4c72-a00c-5b6c7dfd082b)
*

## **6. Conclusion & Recommendations**

- **Performance Bottleneck**: Response times increase significantly at higher loads, indicating API scaling limitations.
- **Investigate 502 Errors**: API logs should be analyzed to identify whether the issue is **server overload** or **network failure**.  
- **Error Rate**: Minimal errors (<2%), but still present at higher thread counts, suggesting timeout thresholds or network congestion.
- **Recommendation**:
  - Optimize API response time for high concurrent users.
  - Consider increasing server resources for better scalability.
  - Implement caching or rate-limiting to reduce request strain.

## **7. Summary**
The performance test results show that the API handles **moderate loads well** but struggles at **high concurrent requests** (1600 threads). Further optimization is recommended to ensure scalability under heavy loads.

---
**End of Report**

