# -OpenEDR---Endpoint-Detection-Response-in-Lab-environment-

📌 Project Overview
OpenEDR is a free, open-source Endpoint Detection and Response platform that provides continuous monitoring of endpoints for suspicious activities. This project documents the full setup and feature testing of OpenEDR across a multi-OS environment, including:

Windows 11 endpoint enrollment and EDR agent installation
Ubuntu Linux endpoint enrollment via CLI
Remote access via Xcitium Remote Control (RDP)
Bidirectional file transfer between host and endpoint
Network isolation (containment) and release of endpoints
File creation event log analysis via the EDR Security tab


🖥️ Lab Environment
MachineOSRoleHostWindows 11OpenEDR portal access, monitoringEndpoint 1Windows 11Primary monitored endpointEndpoint 2Ubuntu (Linux)Secondary monitored endpoint

🚀 Setup & Deployment
1. Windows Endpoint Enrollment

Log in to the OpenEDR portal at openedr.com from the host machine.
After login, copy the enrollment link from the popup wizard.
Paste the link into the endpoint device's browser.
On the Enrollment Wizard page, click Download Windows Installer.
Right-click the downloaded .exe file → Run as Administrator.
Complete the installation wizard → click Finish → Close.
In the OpenEDR portal, navigate to Endpoint Tab → confirm the device appears.
Select the device → click Install or Manage Packages → Install Additional Xcitium Packages.
Check Install Xcitium Client – EDR → click Install.
Restart the endpoint when prompted.


2. Linux Endpoint Enrollment

In the OpenEDR portal, click Enrol Device.
Select Linux as the operating system.
Choose Enroll and Protect → select Ubuntu (16.x+) / Debian (9.x+) with GUI as the platform.
Click Next → copy the enrollment link.
Paste the link into the Linux device's browser.
Click Download Linux Installer.
Open terminal on the Linux device:

bash# Navigate to Downloads
cd ~/Downloads

# Switch to root
sudo su

# Give execute permission to the installer
chmod +x itsm_ax4qT70k_ccsl_installer.run

# Run the installer
./itsm_ax4qT70k_ccsl_installer.run

The installation installs dependencies (including curl) and registers the device.
Go back to the host machine → click Finish in the Enrollment Wizard.


✅ Features Tested
🔗 Remote Access (Windows)

In the portal, go to Endpoint Tab → select the Windows device.
Click Remote Control → With Xcitium Remote Control.
Download and install the Comodo Remote Control application.
Accept the license agreement → click Install → Launch.
Login with your OpenEDR account credentials — select country: USA (required).
Find the enrolled device → click the monitor icon to start a session.
Wait for the RDP connection to establish — full desktop access is provided.


Note: Linux endpoints support logs and isolation only. Remote access and file transfer are Windows-only features.


📁 File Transfer (Host ↔ Endpoint)

In Xcitium Remote Control, click File Transfer (folder icon).
The split view shows Host machine (left) and Endpoint machine (right).
Drag a file from the host side → drop it to the endpoint destination folder.
Confirm the transfer prompt → monitor the progress bar.
Verify the file appears in C:\Program Files (x86)\ on the endpoint.

Transfer log details captured:

Source path
Destination path
File size
Transfer speed
Completion timestamp


🔒 Network Isolation (Containment)
Isolating an endpoint cuts it off from all network communication — useful during incident response to prevent lateral movement.
Steps to isolate:

Select the endpoint device in the portal.
Click More → Security Options → Isolate.
Click Confirm in the "Isolate from Network" dialog.

Verification:

Open Command Prompt on the endpoint.
Run ping google.com — requests will time out, confirming isolation.

To release:

In the portal, click More → Security Options → Release from Isolation.
Re-run ping google.com on the endpoint — replies confirm the device is back on the network.


📋 File Creation Log Analysis (EDR)
OpenEDR logs all endpoint activity, including file write events.
Steps to view logs:

Create a new file on the Windows endpoint.
In the portal, navigate to Security tab → EDR sub-tab.
Search logs by timestamp to find the file write event.

Sample log fields captured:
FieldValuebase_event_typeWrite FilecomponentEDRevent_groupFILEparent_process_pathC:\Windows\System32\userinit.exeparent_process_logged_on_userharsha@harshaaccess_user_nameharsha05@harshaaccess_verdictSafepathC:\Users\harsha05\AppData\Roaming\...


🛠️ Tools & Technologies
ToolPurposeOpenEDR / XcitiumEDR platform & central portalXcitium Remote Control (Comodo)Remote desktop access to endpointsWindows 11Primary endpoint OSUbuntu LinuxSecondary endpoint OSVMware WorkstationVirtualization of endpoint machinesCommand PromptNetwork isolation verification (ping)

🔍 Key Takeaways

OpenEDR provides centralized, real-time visibility across both Windows and Linux endpoints from a single portal dashboard.
The isolation feature is a critical incident response capability — effectively containing a compromised endpoint in seconds.
EDR logs capture granular file-level telemetry, including parent process details, user context, and file paths — essential for forensic investigation.
Linux endpoint support is more limited than Windows, but still provides valuable log data and containment capability.
The Xcitium Remote Control tool enables full RDP-like access to Windows endpoints directly from the host machine, removing the need for separate RDP configuration.
