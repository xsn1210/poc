Vulnerability submitted by Shuning Xu

Supplier:https://www.atlassian.com/software/jira

Software download link: https://www.atlassian.com/software/jira/update

Affected software :jira software

Vulnerability plugin name: timetrackers - Time Tracking & Reporting for Jira

Vulnerability plugin Version: Version 5.2.0•Jira Data Center 9.0.0-9.10.1

Description :For jira software download, choose HOSTING TYPES as the data center to download the latest version 9.10.1 locally and install the Timetracker plug-in. This plug-in has an XSS vulnerability, that is, a cross-site scripting attack. The attacker will insert malicious JavaScript code into the web page, and the payload is <sCrIpT> alert(1)</sCrIpT>, triggering when the administrator/user visits, so as to achieve the purpose of the attack. The essential reason is that the server does not strictly filter the data submitted by the user, causing the browser to treat the user's input as JS code and directly return it to the client for execution. What's happening here is reflected XSS. The attacker can steal the cookies of users such as administrators, steal clipboard content, change web page content (such as download links) and so on by using reflected cross-site scripting attacks.
