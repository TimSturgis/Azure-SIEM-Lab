<h1>Azure SIEM Lab</h1>



<h2>Description</h2>
In this lab, I create a resource group in Azure containing a Windows virtual machine, setup Microsoft Sentinel (SIEM) and create a Log Analytics Workspace to capture event logs from the Windows endpoint. I then configure alerts to be triggered when login attempts are successful and demonstrate the incident.
<br />


<h2>Services and Utilities Used</h2>


- <b>Microsoft Sentinel</b>
- <b>Azure Virtual Machine</b> 
- <b>Log Analytics Workspace</b> 


<h2>Project walk-through:</h2>

<p align="center">




First I created a resource group in Azure and spun up a virtual machine (VM) running Windows 11 Pro to act as an endpoint.  <br/>
![VM created](https://github.com/user-attachments/assets/8580d614-533b-44cb-99d6-fbaef23d9a9a)

<br />
<br />
Network settings were configured to allow remote desktop protocol (RDP) on port 3389 to remain open. This would usually not be a secure setting but is done here for test and demonstration purposes. 

![RDP open](https://github.com/user-attachments/assets/dad24538-8958-4bf9-bf7f-2d458ff1a877)

<br/>

<br />
<br />
I then deployed Microsoft Sentinel as a cloud native security information and event management (SIEM) tool and added it to the Log Analytics Workspace.  
<br/>

![Sentinal spun up](https://github.com/user-attachments/assets/9becb130-f0e0-40f6-992b-bea7db395ca1)

<br/>

![Add Sentinal to LAW](https://github.com/user-attachments/assets/51137ce9-4101-4130-86c1-e4f74cba7555)
<br />


<br />
Next, I needed to send the Windows event logs from the VM endpoint to Log Analytics Workspace to be able to manage the log data in Sentinel. To do this, I set up a data connector (Azure Monitor Agent).
<br/>

![install Azure Monitor Agent from content hub](https://github.com/user-attachments/assets/e5a792fe-1336-47cb-a5df-7deb6c78214c)

<br/>

<br/>

![connected and adding events to sentinal](https://github.com/user-attachments/assets/a38318a6-55a7-47d2-9106-4e8360d35f82)

<br />
<br />
I then configured a data connection rule. The rule uses a simple query to the log data that filters for activity that contains “success” to show all successful logins.  
<br/>

![Query that shows all sign ins](https://github.com/user-attachments/assets/1eb9a45e-77bc-4105-bfc5-2fad2a7d76da)

</p>

The query was then modified to still contain “system” but not contain “system”, this is to avoid logging system account logins.
 <br/>

![query for events that do not contain system account log ins](https://github.com/user-attachments/assets/52397812-1c13-4b67-9d33-69f8bce73797)

</p>

The data connection rule was then used to generate alerts when successful login events occur. 
<br/>

![creating security alert based on the query](https://github.com/user-attachments/assets/6e23b40a-a885-47a3-acbd-064f0b87eb30)

![agan - creating the alert](https://github.com/user-attachments/assets/aae54fa8-ba27-4e19-8217-333f47f8ace3)

</p>

Now, the successful login rule has been created and appears in the Analytics tab.
<br/>

![now analytics shows the new rule-successful log ins](https://github.com/user-attachments/assets/fc5ed3af-1a59-402f-90f4-93ac859f12c9)

</p>

To test and demonstrate the alert, I signed in to the endpoint using Remote Desktop Connection (RDP) to trigger an alert.

![RDP sign in 1](https://github.com/user-attachments/assets/790fc633-38d4-49f5-bb8a-4bfc20ade2d7)

![RDP sign in](https://github.com/user-attachments/assets/e396984d-8efa-448e-a552-1258c420fbbc)

</p>

Here the alert was generated as an incident showing there was a successful login to the endpoint.  <br/>

![the incident](https://github.com/user-attachments/assets/392d9f1e-afb4-4955-84c9-002824f493ca)

</p>
