### Splunk-SPL
A comprehensive technical analysis and hands-on application of Splunk's Search Processing Language (SPL) to query, filter, and visualize log data for actionable insights in cybersecurity.

---

**Developed by: Ramyar Daneshgar**  

---

### Task 2: Connect with the Lab  

#### **Question:** What is the name of the host in the Data Summary tab?  
**Approach:** After launching the lab environment, I navigated to the **Data Summary** tab in the **Search & Reporting App**. This tab provided a quick overview of indexed logs by host, source, and sourcetype. The host was identified as `cyber-host`.  
**Reasoning:** Understanding the data scope, including host identifiers, sets the context for constructing relevant queries later.

---

### Task 3: Search & Reporting App Overview  

#### **Question 1:** What is the 7th search query in the history list?  
**Approach:** I navigated to **Search History** and reviewed previous queries. The 7th query was:  
```spl
index=windowslogs | chart count(EventCode) by Image
```  
**Reasoning:** Search history enables efficient reuse and refinement of previously executed queries, critical for iterative analysis.  

#### **Question 2:** Which Source IP recorded the maximum events?  
**Approach:** I explored the **Field Sidebar**, focusing on the `Source IP` field, which revealed `172.90.12.11` as the IP with the highest event count.  
**Reasoning:** The Field Sidebar provides a high-level summary, making it easier to identify patterns without manually sifting through logs.

#### **Question 3:** How many events occurred between 08:05 AM and 08:06 AM on 04/15/2022?  
**Approach:** I applied a custom time filter in the search query interface, isolating events within the specified window. The result returned `134` events.  
**Reasoning:** Precise time filtering allows focused analysis of event spikes or anomalies during critical periods.

---

### Task 4: SPL Fundamentals and Syntax  

#### **Question 1:** How many events match Event ID 1 and User *James*?  
**Approach:** I used the following query with boolean operators to refine the results:  
```spl
index=windowslogs EventID=1 AND User=*James*
```  
The query returned `4` events.  
**Reasoning:** Boolean operators in SPL enable compound conditions, ensuring only relevant events are included.  

#### **Question 2:** How many events match Destination IP `172.18.39.6` and Port `135`?  
**Approach:** The query was refined further:  
```spl
index=windowslogs DestinationIP=172.18.39.6 DestinationPort=135
```  
The query returned `4` events.  
**Reasoning:** Using specific field-value pairs narrows results to precise network activities.

#### **Question 3:** What is the Source IP with the highest count for a specific Hostname and Destination IP?  
**Approach:**  
```spl
index=windowslogs Hostname="Salena.Adam" DestinationIp="172.18.38.5"
```  
This query revealed `172.90.12.11` as the most active Source IP.  
**Reasoning:** Combining multiple field filters isolates activity within a specific interaction context, aiding root-cause analysis.  

#### **Question 4:** Compare results for the terms `cyber` and `cyber*`.  
**Approach:**  
- For `cyber`:  
  ```spl
  index=windowslogs cyber
  ```  
  Result: `0` events.  

- For `cyber*`:  
  ```spl
  index=windowslogs cyber*
  ```  
  Result: `12,256` events.  
**Reasoning:** Wildcards (*), when appended to search terms, significantly broaden the scope, capturing all variations of the root term.

---

### Task 5: Filtering Results  

#### **Question 1:** What is the 3rd Event ID in this query?  
**Approach:**  
```spl
index=windowslogs | table _time EventID Hostname SourceName | reverse
```  
The third Event ID was `4103`.  
**Reasoning:** Using `reverse` reorders results, making it easier to explore specific positional entries.  

#### **Question 2:** Deduplicate Hostname and identify the first entry.  
**Approach:**  
```spl
index=windowslogs | table _time EventID Hostname SourceName | dedup Hostname | reverse
```  
The first unique Hostname was `Salena.Adam`.  
**Reasoning:** Deduplication prevents redundant results, focusing on unique data points.  

---

### Task 6: Structuring Search Results  

#### **Question 1:** Reverse the query and find the first Hostname.  
**Approach:**  
```spl
index=windowslogs | table _time EventID Hostname SourceName | reverse
```  
The first Hostname was `James.browne`.  

#### **Question 2:** Tail the query to find the last Event ID.  
**Approach:**  
```spl
index=windowslogs | table _time EventID Hostname SourceName | tail 1
```  
The last Event ID was `4103`.  
**Reasoning:** The `tail` command is invaluable for examining the most recent or relevant entries.  

#### **Question 3:** Sort by SourceName and find the top entry.  
**Approach:**  
```spl
index=windowslogs | table _time EventID Hostname SourceName | sort SourceName
```  
The top SourceName was `Microsoft-Windows-Directory-Services-SAM`.  
**Reasoning:** Sorting alphabetically or numerically provides a structured view of results, streamlining analysis.  

---

### Task 7: Transformational Commands  

#### **Question 1:** List the top 8 Image processes; find the count for the 6th.  
**Approach:**  
```spl
index=windowslogs | top limit=8 Image
```  
The count for the 6th process was `196`.  
**Reasoning:** The `top` command reveals the most frequent occurrences, helping prioritize investigation.  

#### **Question 2:** Identify the least active User with the rare command.  
**Approach:**  
```spl
index=windowslogs | rare limit=1 User
```  
The least active User was `James`.  
**Reasoning:** The `rare` command highlights outliers, often indicative of anomalies or infrequent actions.  

#### **Question 3:** Create a pie chart and find the count for `conhost.exe`.  
**Approach:**  
```spl
index=windowslogs | chart count by Image
```  
The count for `conhost.exe` was `70`.  
**Reasoning:** Visualizations such as pie charts provide intuitive insights into data distribution.  

---

### Lessons Learned  

1. **Query Refinement:** SPL operators like `AND`, `OR`, `NOT`, and wildcards enable efficient data narrowing, essential for pinpointing security events.  
2. **Wildcard Usage:** The stark difference between `cyber` and `cyber*` queries underscores the importance of wildcards in expanding search scope.  
3. **Transformational Commands:** Using commands like `top`, `rare`, and `chart` simplifies data visualization and helps quickly identify key insights.  
4. **Best Practices:** Iterative query building and leveraging Splunk’s rich feature set ensures accurate, meaningful analysis, crucial in cybersecurity contexts.  

This lab provided hands-on experience in log analysis using Splunk SPL, emphasizing its versatility in querying, filtering, and visualizing logs to derive intelligence.
