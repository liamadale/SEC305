# Step 1: Install and configure PRTG Network Monitor on PC-A
1. Download PRTG Network Monitor Free Edition from https://www.paessler.com on PC-A. The free edition allows monitoring of up to 100 sensors, which is sufficient for this lab.
2. Install PRTG Network Monitor following the installation wizard. Accept the default installation settings.
3. After installation, launch PRTG and complete the initial setup wizard. You may need to create a local account or use the default credentials.
4. Navigate to the syslog sensor configuration in PRTG to enable syslog reception on PC-A.
5. Configure PRTG to listen for syslog messages on UDP port 514 (the default syslog port).
# Step 2: Configure R1 to log messages to the syslog server
1. Verify that you have connectivity between R1 and PC-A by pinging the R1 G0/0/1 interface IP address 192.168.1.1 from PC-A. If it is not successful, troubleshoot as necessary before continuing.
2. NTP was configured in Part 4 to synchronize the time on the network. Verify that the timestamp service for logging is enabled on the router using the show run command. Use the following command if the timestamp service is not enabled.
```
R1(config)# service timestamps log datetime msec
```
3. Configure the syslog service on the router to send syslog messages to PC-A.
```
R1(config)# logging host 192.168.1.3
```
# Step 3: Configure the logging severity level on R1
Logging traps can be set to support the logging function. A trap is a threshold that when reached, triggers a log message. The level of logging messages can be adjusted to allow the administrator to determine what kinds of messages are sent to the syslog server.
1. Use the logging trap command to determine the options for the command and the various trap levels available.
```
R1(config)# logging trap ?
```
2. Use the logging trap command to set the severity level for R1 to warnings (level 4).
```
R1(config)# logging trap warnings
R1(config)# exit
```

# Step 4: Display the current status of logging for R1
1. Use the show logging command to see the type and level of logging enabled.

```
R1(config)# service timestamps log datetime msec
R1(config)# logging host 192.168.1.3
R1(config)# logging trap ?
R1(config)# logging trap warnings
R1(config)# exit
```

# Step 5: Make changes to the router and monitor syslog results using PRTG Network Monitor
1. To verify that PRTG is receiving syslog messages from R1, disable and enable R1's G0/0/0 interface. 
```
R1# configure terminal
R1(config)# interface g0/0/0
R1(config-if)# shutdown
```
    Wait for the interface state messages to appear.
```
R1(config-if)# no shutdown
R1(config-if)# exit
R1(config)# exit
```
2. Navigate to PC-A and view the syslog messages in PRTG Network Monitor. You should see messages related to the interface status changes, including OSPF neighbor adjacency messages.
3. Explore additional PRTG features such as creating custom sensors, setting up alerts, and generating reports based on syslog data.