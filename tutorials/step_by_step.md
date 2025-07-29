Open your Splunk instance.
- Navigate to `Settings > Add Data`.
- Upload `bns_practice.csv` with source type as `csv`.
- Index the data under a new index (e.g., `bns_practice`).

Searching and Filtering:
#Let's search events from a specific user
- index=bns_practice user="Mbua"
  
#Let's get failed Logins per user
- index=bns_practice event_type="login_attempt' login_status="failure" | stats count by user, src_ip

#Let's find known bad tools
- index=bns_practice user_agent IN ("sqlmap", "Nikito", "nmap", "dirbuster")

#List all events with malicious reputation
- index=bns_practice reputaion="Malicious"

#Let's find top countries by event count
- index=bns_practice | stats count by country

#Access from unusual countries
- index=bns_practice country IN ("Russia", "Brazil", "India")

#Combine filters to check for failed login in high risk countries
- index=bns_practice event_type="loginin_attempt" login_status="failure" country IN ("Russia", "Brazil", "India")

#Creating Dashboards: let's visualize which users appear most often in the logs using a bar chart
- index=bns_practice | top user
- click visualization, select bar chart.

#Visualize unauthorized or suspicious attempts made using automated tools
- index=bns_practice user_agent IN ("dirbuster", "sqlmap", "Nikito", "nmap") | stats count by user_agent

#Creating Alerts: let's create alert when a suspicious or known user agent logs in successfully.
- index=bns_practice event_type="login_attempt" login_status="success" user_agent IN ("sqlmap", "dirbuster", "python-requests", "nmap")
- Click "Save as", click Alert, select how you want to receive the alert and save.
