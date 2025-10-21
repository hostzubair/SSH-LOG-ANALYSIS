# Objective


The goal of this project is to analyze SSH authentication logs to detect:

1. Successful logins (who connected, from where)   
2. Failed login attempts (possible brute-force or password spraying)  
3. Multiple failed authentication attempts (indicators of brute-force)   
4. Connections without authentication (potential scanning or incomplete sessions)

# Preparation


1. Log in to your Splunk instance (Enterprise or Free). 
2. Go to Apps > Search & Reporting.  
3. Click Add Data → Upload. 
4. Select the provided ssh_log.json file and upload it.   
5. Choose sourcetype = _json so Splunk automatically extracts fields.   
6. Index it under a new index, e.g., ssh_logs.  
7. Review and click on start searching  

![image alt](https://github.com/hostzubair/SSH-LOG-ANALYSIS/blob/3255e068936e1f7346561213efa61d17d46e8f80/Image/Screenshot%20(74).png)

![image alt](https://github.com/hostzubair/SSH-LOG-ANALYSIS/blob/0ec706b83b677753fac0771a341325ad04404a18/Image/Screenshot%20(75).png)



# Step-by-Step Guide

# Task 1: Ingest and Parse Logs


1. Upload ssh_log.json into Splunk.   
2. Ensure the following fields are extracted correctly:   
3. event_type (Successful SSH Login, Failed SSH Login, Multiple Failed Authentication Attempts, Connection Without Authentication)    
4. auth_success (true/false/null)    
5. auth_attempts      
6. id.orig_h (source IP)    
7. id.resp_h (destination host)  
 

<summary><b>Run a validation search:</b></summary> 

Run `index=ssh_log | stats count by event_type`, 

![image alt](https://github.com/hostzubair/SSH-LOG-ANALYSIS/blob/f9bdc83380c703213b71172356d633915d029f0d/Image/Screenshot%20(76).png)

# Task 2: Analyze Failed Login Attempts

<summary><b>Identify all failed login attempts:</b></summary> 

Run `event_type="Failed SSH Login"
| stats count by id.orig_h`, 

![image alt](https://github.com/hostzubair/SSH-LOG-ANALYSIS/blob/f9bdc83380c703213b71172356d633915d029f0d/Image/Screenshot%20(77).png)

![image alt](https://github.com/hostzubair/SSH-LOG-ANALYSIS/blob/f9bdc83380c703213b71172356d633915d029f0d/Image/Screenshot%20(78).png)

# Task 3: Detect Multiple Failed Authentication Attempts (Brute Force)

<summary><b>Search for multiple failed attempts in logs:</b></summary> 

Run `event_type="Multiple Failed Authentication Attempts"
| stats count by id.orig_h, id.resp_h`, 

![image alt](https://github.com/hostzubair/SSH-LOG-ANALYSIS/blob/f9bdc83380c703213b71172356d633915d029f0d/Image/Screenshot%20(79).png)

![image alt](https://github.com/hostzubair/SSH-LOG-ANALYSIS/blob/f9bdc83380c703213b71172356d633915d029f0d/Image/Screenshot%20(80).png)

# Task 4: Track Successful Logins

<summary><b>Search for multiple failed attempts in logs:</b></summary> 

Run `event_type="Successful SSH Login"
| stats count by id.orig_h, id.resp_h`

![image alt](https://github.com/hostzubair/SSH-LOG-ANALYSIS/blob/7753cf6537979face5cad270a2a857177dc42805/Image/Screenshot%20(81).png)

![image alt](https://github.com/hostzubair/SSH-LOG-ANALYSIS/blob/7753cf6537979face5cad270a2a857177dc42805/Image/Screenshot%20(86).png)

# Task 5: Spot Suspicious Connections Without Authentication

<summary><b>Search for unauthenticated SSH connections:</b></summary> 

Run `event_type="Connection Without Authentication"
| stats count by id.orig_h`

![image alt](https://github.com/hostzubair/SSH-LOG-ANALYSIS/blob/7753cf6537979face5cad270a2a857177dc42805/Image/Screenshot%20(82).png)

![image alt](https://github.com/hostzubair/SSH-LOG-ANALYSIS/blob/7753cf6537979face5cad270a2a857177dc42805/Image/Screenshot%20(83).png)


<summary><b>Create a timechart visualization to monitor such events over time:</b></summary> 


Run `event_type="Connection Without Authentication"
| timechart count by id.orig_h`

![image alt](https://github.com/hostzubair/SSH-LOG-ANALYSIS/blob/7753cf6537979face5cad270a2a857177dc42805/Image/Screenshot%20(84).png)

![image alt](https://github.com/hostzubair/SSH-LOG-ANALYSIS/blob/7753cf6537979face5cad270a2a857177dc42805/Image/Screenshot%20(85).png)


# Conclusion

1. Built dashboards to monitor SSH activity.

2. Identified brute-force login attempts and suspicious access attempts.

3. Configured Splunk alerts for high-risk behavior.

4. Learned how to parse, search, visualize, and alert on SSH logs in Splunk.






















