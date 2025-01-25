# SQL style searching 

## Objective


This lab will use SQL-like searches to pick log tables, which items to filter for, and which columns to present, all in one query. OpenSearch will be used to demonstrate how it implements SQL style searching. SQL allows for a smiplified way to search and filter through massive amounts of data stored in tabular format. 

### Skills Learned


- Navigate SIEM data using SQL style search.
- Learn workflow to find tables and dolumn names via SQL search.
- SQL searches of your own data.
- Use SQL aggregation functions to group data and calculation parameters of data in the groups.
- Use UI and CLI version of the search.


### Tools Used


- OpenSearch

## Steps

# - Understanding the available data tables

It is important to know which tables or indexes are available. 
Query Workbench feature of OpenSearch is a web interface that allows you to type SQL-style queries and run them directly from the UI.
1. Show all tables stored in OpenSearch that start with the name Suricata-
   
![Screenshot 2025-01-23 131041](https://github.com/user-attachments/assets/bbadcb1c-4bd0-48e6-9186-91cca6d70b86)
![Screenshot 2025-01-23 131112](https://github.com/user-attachments/assets/959f8373-da07-4202-a251-e3a3db807b85)

We will continue working with the Suricata-Flow table

2. Displaying available columns.

Best way to enumerate columns would be to look horizontal across a log. We will use the following query to search a single log to keep output clean. 
![Screenshot 2025-01-23 132103](https://github.com/user-attachments/assets/0f825ab3-3996-40b0-b9c6-78ad57e6eca0)
![Screenshot 2025-01-23 132135](https://github.com/user-attachments/assets/0c7e5fbb-6edd-4d4a-8775-5a7a8a0500a4)

3. Searching your data with SQL
 - Show a list of source_ip in the suricata-flow, where the application_protocol field was ssh
             - SELECT [column_name(s)]
             - FROM [index_name]
             - WHERE [column_name] [operator];
![Screenshot 2025-01-23 132410](https://github.com/user-attachments/assets/095c2b3c-23c1-450f-81aa-88ab9d330e1b)


![Screenshot 2025-01-23 132636](https://github.com/user-attachments/assets/17e52ca7-1012-45ea-a40d-e364a97938af)

4. Aggregation Functions
   Aggregation Functions for SQL language in OpenSearch:
     AVG : Returns the average of the results
     COUNT : Returns the number of results
     SUM : returns the sum of the results
     MIN : Returns the  minimum of the results
     MAX : Returns the maximum of the results

Aggregation functions would group and calculate something about the data. Results can be used as additional columns or for producing more useful and detailed results.

Find the most common source_ip that is connected to the honeypot. 




![Screenshot 2025-01-23 141410](https://github.com/user-attachments/assets/0b542f74-9374-44c8-a7fe-f2727e6f1a27)
Explanation:
- FROM suricata-flow  -take all the suricata-flow index of logs
- GROUP BY source_ip  -Aggregate all logs from the table on the source_ip field
- COUNT(source_ip) -aggregation function, happens before whole line is evaluated, counts how many logs are in each aggregated group as a result of the previous GROUP BY
- SELECT COUNT(source_ip), source_ip -for each line of output, show results of previously described count in the first column and the source IP associated with that count in the second column(so we have count of IP hits)
- ORDER BY 1 -how to present the data, tells engine to order results in descending order (highest on top) , by first column listed in the select statement (where number 1 comes from), this will result in the most common source_ip
- LIMIT 10  -only shows top 10 groups

  ![Screenshot 2025-01-23 141441](https://github.com/user-attachments/assets/b616a63f-c4c3-4097-850c-c33b28c67f88)


## Additional Question
- Locate the firewall logs data table to see what items are included in it. Produce output showing:
          - Most commmonly used destination ports
          - destinatino port on the left column
          - count in the right column
          - show only top 5 destination ports represented in the logs in descending order


  1.  Show tables that would be named firewall:
     
![Screenshot 2025-01-23 144620](https://github.com/user-attachments/assets/20e6a3ea-67b5-45f4-a804-671da4d6525e)


2. See available data points:

![Screenshot 2025-01-23 145132](https://github.com/user-attachments/assets/8be37b14-188f-45c4-b714-bfdf06a44a9d)

![Screenshot 2025-01-23 145153](https://github.com/user-attachments/assets/ed81f4d8-bad0-45ad-8126-cd5cd7d71182)

3. Filter through data
   SELECT destination_port, COUNT(destination_port)
   FROM firewall
   GROUP BY destination_port
   ORDER BY 2 DESC
   LIMIT 5
   
   
   
![Screenshot 2025-01-23 152848](https://github.com/user-attachments/assets/ce89df25-6798-41ce-a049-dd3794c72772)
![Screenshot 2025-01-23 152911](https://github.com/user-attachments/assets/18af36cc-cc5f-416c-9217-9131b342a193)
