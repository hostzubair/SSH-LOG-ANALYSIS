# Objective


The goal of this project is to analyze SSH authentication logs to detect:

Successful logins (who connected, from where)   
Failed login attempts (possible brute-force or password spraying)  
Multiple failed authentication attempts (indicators of brute-force)   
Connections without authentication (potential scanning or incomplete sessions)

# Preparation


Log in to your Splunk instance (Enterprise or Free). 
Go to Apps > Search & Reporting.  
Click Add Data → Upload. 
Select the provided ssh_log.json file and upload it.   
Choose sourcetype = _json so Splunk automatically extracts fields.   
Index it under a new index, e.g., ssh_logs.  
Review and click on start searching  

![image alt](https://github.com/hostzubair/SSH-LOG-ANALYSIS/blob/3255e068936e1f7346561213efa61d17d46e8f80/Image/Screenshot%20(74).png)

![image alt](https://github.com/hostzubair/SSH-LOG-ANALYSIS/blob/0ec706b83b677753fac0771a341325ad04404a18/Image/Screenshot%20(75).png)



# Step-by-Step Guide

# Task 1: Ingest and Parse Logs


Upload ssh_log.json into Splunk.   
Ensure the following fields are extracted correctly:   
event_type (Successful SSH Login, Failed SSH Login, Multiple Failed Authentication Attempts, Connection Without Authentication)    
auth_success (true/false/null)    
auth_attempts      
id.orig_h (source IP)    
id.resp_h (destination host)  
 

<summary><b>Run a validation search:</b></summary> 

Run `index=ssh_log | stats count by event_type`, 

![image alt](https://github.com/hostzubair/SSH-LOG-ANALYSIS/blob/f9bdc83380c703213b71172356d633915d029f0d/Image/Screenshot%20(76).png)

# Task 2: Analyze Failed Login Attempts

<summary><b>Identify all failed login attempts:</b></summary> 

Run `event_type="Failed SSH Login"
| stats count by id.orig_h`, 

![image alt](https://github.com/hostzubair/SSH-LOG-ANALYSIS/blob/f9bdc83380c703213b71172356d633915d029f0d/Image/Screenshot%20(77).png)

![image alt](https://github.com/hostzubair/SSH-LOG-ANALYSIS/blob/f9bdc83380c703213b71172356d633915d029f0d/Image/Screenshot%20(78).png)











