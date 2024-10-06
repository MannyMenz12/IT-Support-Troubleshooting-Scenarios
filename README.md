# IT-Support-Troubleshooting-Scenarios
A collection of common IT support troubleshooting scenarios
### Case Study 1: Network Printer Not Printing for Users Despite Being Connected
1. Introduction/Problem Description:

Issue:
The network printer (Model: HP LaserJet Pro 500) used in the office for multiple departments stopped printing for all users. The printer shows as “connected” on the network, and users can see it in their printer lists, but no documents are printing. Jobs sent to the printer remain stuck in the print queue without producing an error message.

Environment:

	•	Printer connected via Ethernet to the company network.
	•	Printer shared through a Windows Server (Windows Server 2019) acting as the print server.
	•	Users accessing the printer from various devices: Windows 10 and macOS.

2. Approach/Investigation:

Step 1: Check Physical Connections

	•	Tool: Visual Inspection, Device Control Panel
	•	Action: Confirmed that the printer is powered on and properly connected via Ethernet. No error messages or warning lights on the printer’s control panel.
	•	Result: The printer is physically operational.

Step 2: Ping the Printer

	•	Tool: Command Prompt (ping <printer_IP>)
	•	Action: Pinged the printer’s IP address from my workstation to check if the device is reachable on the network.
	•	Result: The printer responded to ping requests, confirming it is connected to the network.

Step 3: Check Print Queue

	•	Tool: Print Management Console on Windows Server (printmanagement.msc)
	•	Action: Logged into the print server and reviewed the print queue. Found multiple stuck jobs from different users. Cleared the print queue, but the jobs reappeared when users tried to print again.
	•	Result: Jobs are stuck without error messages, indicating potential driver or communication issues.

Step 4: Restart Print Spooler

	•	Tool: Windows Services (services.msc)
	•	Action: Restarted the print spooler service on both the print server and the user workstations to clear any possible stuck jobs or spooler errors.
	•	Command Used:

# Restart Print Spooler via Command Prompt (for automation)
net stop spooler && net start spooler


	•	Result: The service restarted successfully, but printing still failed.

Step 5: Check Printer Driver and Firmware

	•	Tool: HP Printer Driver & Firmware Download, Device Manager
	•	Action:
	•	Verified the installed printer driver version on the print server. Found it was outdated compared to the latest release from HP.
	•	Checked the printer’s firmware version through the control panel. The firmware was also out of date.
	•	Result: Both drivers and firmware needed updating.

Step 6: Verify Network Configuration

	•	Tool: Printer Control Panel, Router Configuration Interface
	•	Action:
	•	Checked the printer’s network settings through the control panel.
	•	Verified the printer had a static IP address assigned and checked for IP conflicts in the router’s DHCP lease table.
	•	Result: No IP conflicts, static IP was properly assigned.

Step 7: Review User Permissions

	•	Tool: Active Directory, Print Management Console
	•	Action:
	•	Checked user permissions for accessing the shared printer.
	•	All users had appropriate permissions to print, and no changes had been made recently.
	•	Result: Permissions were properly configured.

3. Solution:

Step 1: Update Printer Drivers and Firmware

	•	Tool: HP Support, Device Manager, Windows Update
	•	Action:
	•	Downloaded the latest HP LaserJet Pro 500 driver from the HP support website and installed it on the print server.
	•	Updated the firmware by connecting to the printer’s web interface (HP Embedded Web Server) and manually uploading the firmware update file.
	•	Result: Driver and firmware updates were applied successfully.

Step 2: Clear the Printer Queue and Spooler Reset

	•	Tool: Print Management Console, Command Line
	•	Action:
	•	Cleared the print queue and restarted the print spooler service once again.
	•	Ran a PowerShell script to automate spooler restart across user devices for future troubleshooting:

# PowerShell Script to Restart Print Spooler on Multiple Machines
$computers = @("Computer1", "Computer2", "Computer3")
foreach ($computer in $computers) {
    Invoke-Command -ComputerName $computer -ScriptBlock {
        Restart-Service -Name "Spooler"
    }
}


	•	Result: After restarting, the print jobs began to process again.

Step 3: Reconfigure Printer’s Network Settings

	•	Tool: Printer Control Panel, Router Configuration
	•	Action:
	•	Reconfigured the printer’s network settings to ensure proper connectivity, assigning a static IP within the correct subnet range.
	•	Rebooted the printer and network switch for a fresh connection.
	•	Result: The network settings were confirmed correct, and the printer reconnected successfully.

Step 4: Test Printing Across Devices

	•	Tool: User Workstations, Print Management Console
	•	Action: Conducted test prints from several devices (Windows 10 and macOS) to confirm that printing was functioning properly.
	•	Result: All test prints were successful, confirming the issue was resolved.

4. Outcome:

After completing the driver and firmware updates, clearing the printer queue, and reconfiguring the network settings, the network printer resumed normal operation. All users were able to print documents successfully. The key issue was the outdated printer drivers, which were incompatible with the latest Windows updates and caused the print jobs to become stuck.
