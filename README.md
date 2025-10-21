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











