# THM-WriteUp-Splunk-SLP
Writeup for TryHackMe Splunk: Exploring SPL Lab - a comprehensive exploration of Splunk's Search Processing Language (SPL), focusing on querying, filtering, and structuring log data to extract actionable insights.

Developed by Ramyar Daneshgar 

### Task 2: Connect with the Lab

**Question**: What is the name of the host in the Data Summary tab?  
- **Approach**: After connecting to the lab environment, I navigated to the Data Summary tab under the Search & Reporting app to review the indexed logs. The host name displayed was `cyber-host`.

**Reasoning**: This step was essential for familiarizing myself with the lab environment and understanding the scope of data available for analysis.

---

### Task 3: Search & Reporting App Overview

**Question 1**: In the search history, what is the 7th search query in the list?  
- **Approach**: I navigated to the Search History panel and reviewed the past search queries. The 7th query was:  
  ```spl
  index=windowslogs | chart count(EventCode) by Image
  ```

**Question 2**: In the left field panel, which Source IP has recorded the maximum events?  
- **Approach**: I used the Field Sidebar to explore the `Source IP` field, which revealed that `172.90.12.11` had the highest count of events.  
- **Reasoning**: Leveraging the sidebar is a quick way to identify patterns in logs without writing detailed queries.

**Question 3**: How many events occurred on `04/15/2022` between `08:05 AM and 08:06 AM`?  
- **Approach**: I applied a time filter in the Search & Reporting app and found that `134` events occurred during this timeframe.

---

### Task 4: SPL Fundamentals and Syntax

**Question 1**: How many events are returned when searching for Event ID `1` AND User `*James*`?  
- **Approach**: Using a boolean operator to refine the search:
  ```spl
  index=windowslogs EventID=1 AND User=*James*
  ```
  The result returned `4` events.

**Question 2**: How many events are observed with Destination IP `172.18.39.6` and Destination Port `135`?  
- **Approach**: I refined the query further:
  ```spl
  index=windowslogs DestinationIP=172.18.39.6 DestinationPort=135
  ```
  This returned `4` events.

**Question 3**: Identify the Source IP with the highest count for this query:
```spl
index=windowslogs Hostname="Salena.Adam" DestinationIp="172.18.38.5"
```
- **Approach**: Running the query and reviewing the results, the Source IP with the highest count was `172.90.12.11`.

**Question 4**: Search for `cyber` and `cyber*`, and compare event counts.  
- **Approach**:
  - For `cyber`, the query returned `0` events.
  - For `cyber*`, the query returned `12,256` events.  
- **Reasoning**: Using `*` as a wildcard in Splunk expands the search scope, capturing all terms starting with "cyber".

---

### Task 5: Filtering Results

**Question 1**: What is the third Event ID in this query?
```spl
index=windowslogs | table _time EventID Hostname SourceName | reverse
```
- **Approach**: Using the `reverse` command, I found that the third Event ID was `4103`.

**Question 2**: Use `dedup` with the query above to find the first unique Hostname.  
- **Approach**: I added `dedup` to remove duplicates:
  ```spl
  index=windowslogs | table _time EventID Hostname SourceName | dedup Hostname | reverse
  ```
  The first Hostname was `Salena.Adam`.

---

### Task 6: Structuring Search Results

**Question 1**: Using `reverse` with this query:
```spl
index=windowslogs | table _time EventID Hostname SourceName
```
What is the first Hostname?  
- **Approach**: The query reversed the results, and the first Hostname was `James.browne`.

**Question 2**: Update the query with `tail` to find the last Event ID.  
- **Approach**:
  ```spl
  index=windowslogs | table _time EventID Hostname SourceName | tail 1
  ```
  The last Event ID was `4103`.

**Question 3**: Sort by `SourceName` and find the top value.  
- **Approach**:
  ```spl
  index=windowslogs | table _time EventID Hostname SourceName | sort SourceName
  ```
  The top SourceName was `Microsoft-Windows-Directory-Services-SAM`.

---

### Task 7: Transformational Commands

**Question 1**: List the top 8 Image processes and find the count for the 6th.  
- **Approach**:
  ```spl
  index=windowslogs | top limit=8 Image
  ```
  The count for the 6th Image was `196`.

**Question 2**: Identify the User with the least activity using `rare`.  
- **Approach**:
  ```spl
  index=windowslogs | rare limit=1 User
  ```
  The least active User was `James`.

**Question 3**: Create a pie chart and find the count for `conhost.exe`.  
- **Approach**:
  ```spl
  index=windowslogs | chart count by Image
  ```
  The count for `conhost.exe` was `70`.

---

### Lessons Learned

1. **Query Refinement**: Using SPL’s operators (e.g., boolean, wildcards) and commands like `table`, `fields`, `sort`, and `dedup` simplifies data analysis by narrowing focus to specific events.
2. **Wildcard Efficiency**: The difference between `cyber` and `cyber*` searches highlights the importance of wildcards in expanding search scope.
3. **Transformational Commands**: Commands like `top`, `rare`, and `chart` are invaluable for identifying patterns and outliers, aiding in visualizing and interpreting data.
4. **Best Practices**: Building queries iteratively and combining commands ensures both efficiency and accuracy, critical skills for log analysis in Splunk.

This lab emphasized hands-on experience with SPL and demonstrated its power in log parsing, filtering, and visualization to efficiently analyze security logs and derive actionable insights.
