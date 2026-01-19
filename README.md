# MiniProject-5

## Objective

The objective of this project is to perform network traffic analysis on a PCAP file containing simulated malware activity. The goal is to identify key indicators of compromise (IOC), such as the infected machine's IP and hostname, the compromised website, and the domain that delivered the exploit kit and malware. This analysis simulates a real-world scenario where a Security Operations Center (SOC) analyst must trace malware behavior within a network capture.

### Skills Learned

- Network traffic inspection and filtering using Wireshark.
- Identification of infected hosts through HTTP and NBNS traffic.
- MAC address correlation via Ethernet II analysis.
- Threat hunting methodology: identifying compromised websites and exploit servers.
- Understanding and analyzing malware delivery chains.

### Tools Used

Wireshark – Used for in-depth packet analysis and protocol inspection.
PCAP file – Sample provided for threat hunting simulation.


### Case 1 - Log Analysis – File Deletion Event (Wazuh)

1) Delete one of the .exe files where the agent (Wazuh) is installed, and search the Wazuh logs, then provide an explanation for the logs found.

<img width="1037" height="580" alt="image" src="https://github.com/user-attachments/assets/6524dbb9-0c3d-4e19-8f27-cb07ea348666" />
<img width="1017" height="579" alt="image" src="https://github.com/user-attachments/assets/4f8277fc-84fd-4303-969b-fd0b4b3ca29d" />
<img width="1019" height="589" alt="image" src="https://github.com/user-attachments/assets/33ea9d74-29aa-469f-9691-1f52345ea8b3" />
<img width="1033" height="590" alt="image" src="https://github.com/user-attachments/assets/61ebc55c-502a-48f9-ac37-8b5b9d5ffef2" />
<img width="1024" height="588" alt="image" src="https://github.com/user-attachments/assets/79ff4961-10ff-47d8-a75f-89468f4d4876" />
<img width="1024" height="592" alt="image" src="https://github.com/user-attachments/assets/cabaa125-5a6f-42a0-8e65-267e59288da6" />
<img width="1045" height="597" alt="image" src="https://github.com/user-attachments/assets/625ad848-fb6f-4694-9e3b-dc9515f09b45" />

- Rule Description: File deleted
- Timestamp: March 1, 2025 @ 23:48:02.094
- Source IP Address: 10.0.2.15
- User: root (executed by the logged-in user “hafizh”)
- Log Location / File Path: /root/chrome.deb
- Original Log Content: File /root/chrome.deb deleted — Mode: whodata
- Agent: ParrotOS

Explanation : Based on the Wazuh log analysis, a file deletion activity was detected on an endpoint with the agent name ParrotOS. The triggered alert has the rule description “File deleted”, indicating that a monitored file system change was captured using the whodata mechanism.

This event occurred on March 1, 2025 at 23:48:02 and originated from the source IP address 10.0.2.15. The deleted file was /root/chrome.deb, which is located in a privileged directory, suggesting that the action required root-level permissions.

The user field shows that the command was executed as root, while the logged-in user was “hafizh”, indicating the use of sudo to elevate privileges. This confirms that the user had administrative access to the system.

By correlating the log details, it can be concluded that the file was manually deleted using the rm command rather than being removed by an automated process or malware. The activity was successfully recorded by the Wazuh agent through whodata, enabling accurate attribution of the user, file location, and timestamp.

This analysis demonstrates how Wazuh can effectively monitor critical endpoint activities, particularly file modifications or deletions within sensitive directories, which in real-world scenarios may serve as early indicators of suspicious behavior or policy violations.

### Case 2 - Log Analysis – Service Termination via Sudo (Wazuh)

2) Attach a screenshot of the log you found and provide a description below of how you read the log in your language and the version you use to read the log.

<img width="771" height="846" alt="image" src="https://github.com/user-attachments/assets/218b6ed2-0e18-4f13-bbfb-85c67c57e14e" />

- Rule Description: Successful sudo to ROOT executed
- Timestamp: March 2, 2025 at 05:05:23
- Source IP Address: 10.0.2.15
- User: root (executed by a user with sudo privileges)
- Log Location: journald
- Original Log Content: Mar 02 05:05:23 parrot sudo[5056]: root : TTY=pts/1 ; PWD=/home/hafizh/Desktop ; USER=root ; COMMAND=/usr/bin/systemctl stop apache2
- Agent: ParrotOS

Explanation : Based on the Wazuh log analysis, a privileged command execution was detected on an endpoint with the agent name ParrotOS. The triggered alert has the rule description “Successful sudo to ROOT executed”, indicating that a user successfully escalated privileges to execute a command as the root user.

This activity occurred on March 2, 2025 at 05:05:23 UTC and originated from the source IP address 10.0.2.15. The log was generated from journald, which records system-level events and command executions.

The log details show that the command was executed as root by a user with sudo privileges. The working directory at the time of execution was /home/hafizh/Desktop, and the terminal session used was TTY=pts/1. The exact command executed was:

"systemctl stop apache2"

This indicates that the user intentionally stopped the Apache2 service on the ParrotOS system using elevated privileges.

From a security monitoring perspective, this event demonstrates how Wazuh can detect and log administrative actions that impact critical system services. In a real-world SOC environment, such activity could represent legitimate maintenance or, if unauthorized, a potential attempt to disrupt services or evade detection. Therefore, correlating this event with change management records and user activity is essential to determine whether the action is benign or suspicious.



