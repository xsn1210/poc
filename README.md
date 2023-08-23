Vulnerability submitted by Shuning Xu
Supplier: https://www.atlassian.com/zh/software/jira
Software download link: https://www.atlassian.com/software/jira/update
Affected software :jira software
Vulnerability plug-in name: timetrackers - Time Tracking & Reporting for Jira
Vulnerability plugin Version: Version 5.2.0•Jira Data Center 9.0.0-9.10.1
Description :jira software download selected HOSTING TYPES to download the latest version of data center 9.10.1 locally to install the Timetracker plug-in. The Timetracker plug-in has XSS vulnerability, that is, cross-site scripting attack. The attacker inserts malicious JavaScript code into the web page (input form, URL, message board, etc.), causing the administrator/user to access the trigger, thus achieving the purpose of the attack. The essential reason is that the server is not strict in filtering the data submitted by the user, resulting in the browser treating the user's input as JS code and directly returning it to the client for execution. What appears here is reflected XSS. Attackers using reflected cross-site scripting attacks can steal cookies from users such as administrators, steal clipboard content, change web content (such as download links), and so on.
