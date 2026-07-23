Example logs to test

Low severity (should auto resolve, no pause):
"User jsmith logged in successfully from 10.0.1.15, same location as last 30 days"
Expected: benign / low

Medium severity (should auto resolve):
"GET /search?q=test HTTP/1.1 from 192.0.2.5, status 404"
Expected: web_attack or benign / low to medium, single rejected probe

High severity (should escalate and pause for analyst input):
"Failed password for invalid user admin from 203.0.113.44 port 51422 ssh2 (14 attempts in 60s)"
Expected: credential_access / high

Critical severity (should escalate and pause for analyst input):
"Scheduled task created: 'WindowsUpdateCheck' running vssadmin delete shadows /all /quiet"
Expected: malware_c2 / critical, ransomware staging pattern
