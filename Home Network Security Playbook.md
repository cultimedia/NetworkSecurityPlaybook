# Home Network Security Playbook

***Objective**:* Ensure the integrity, confidentiality, and availability of the home network by identifying vulnerabilities, monitoring network traffic, and ensuring all devices are compliant with security best practices.

***Scope**:* All devices connected to the home network managed through the TP-Link Omada router, controller, and switch.

## **Tools Used**:

- **Netdiscover**: For scanning and identifying devices connected to the network.
- **Nmap**: To scan for open ports and detect running services on each device.
- **OpenVAS**: For vulnerability assessment to identify potential security weaknesses.

## Playbook Stages

**0. Updating the Kali Linux System** Ensure your system is secure and up-to-date by regularly performing these updates. See the detailed steps in the prelude to begin your network security operations with the latest software defenses.

**1. Network Mapping** Identify all devices connected to your network using Netdiscover. This foundational step ensures you know exactly what devices are on your network and how they are connected.

**2. Port and Service Scanning** Use Nmap to scan for open ports and running services. This step helps you understand what services are exposed on your network and could potentially be accessed by unauthorized users.

**3. Vulnerability Assessment** Conduct a thorough security check with OpenVAS to pinpoint vulnerabilities and weak spots in your network’s armor. This assessment will guide your efforts to fortify your network against attacks.

**4. Monitoring and Maintenance** Continuously monitor the network for new devices, unexpected connections, or any anomalies. Use a combination of the tools and regular checks to maintain the security integrity of your network.

## Stage 0: Updating Kali Linux System

**Objective**: Keep the system secure and up-to-date with the latest patches, software updates, and security fixes to prevent vulnerabilities and improve system stability.

**Frequency**: This step should be performed before any major scanning or network mapping activity, ideally at least once a week.

### Steps to Update Kali Linux

1. **Open Terminal**: Boot up your terminal, the digital gate through which all commands shall pass.

2. **Update Package Lists**:
    - **Command**: Update the list of available packages and their versions, but do not install or upgrade any packages.
      ```bash
      sudo apt update
      ```
    - **Purpose**: Ensures you have the latest listings of available package updates.

3. **Upgrade Installed Packages**:
    - **Command**: Upgrade all installed packages to their newest available versions.
      ```bash
      sudo apt full-upgrade -y
      ```
    - **Purpose**: Applies the latest updates and security patches to all your installed software, minimizing vulnerabilities.

4. **Remove Unused Dependencies**:
    - **Command**: After upgrading, some packages may become unnecessary.
      ```bash
      sudo apt autoremove -y
      ```
    - **Purpose**: Cleans up the system by removing packages that were installed as dependencies for other packages and are no longer needed.

5. **Restart the System** (if required):
    - **Command**:
      ```bash
      sudo reboot
      ```
    - **Purpose**: Ensures that all updates are properly applied and that the system is running the latest kernel and software versions.

### Stage 1: Network Mapping

**Objective**: Identify and catalog all devices connected to each segmented subnet of the home network. This precision mapping provides clear visibility into the hardware, personal devices, IoT gadgets, and guest devices, allowing for tailored security measures.

#### Steps for Each Subnet:

- **192.168.0.0 - Network Hardware**
  - **Tool**: Netdiscover
  - **Command**:
    ```bash
    sudo netdiscover -r 192.168.0.0/24
    ```
  - **Purpose**: Scan this subnet to identify routers, switches, and other network infrastructure devices. This helps in pinpointing how your network is structured and identifying potential gateways and choke points.

- **192.168.10.0 - Computers, Phones, iPads**
  - **Tool**: Netdiscover
  - **Command**:
    ```bash
    sudo netdiscover -r 192.168.10.0/24
    ```
  - **Purpose**: Focus on personal and frequently used devices. This scan ensures that all personal devices are accounted for and evaluated for potential vulnerabilities.

- **192.168.20.0 - IoT Devices**
  - **Tool**: Netdiscover
  - **Command**:
    ```bash
    sudo netdiscover -r 192.168.20.0/24
    ```
  - **Purpose**: IoT devices often present unique security challenges due to their diverse nature and firmware limitations. Identifying them distinctly helps in applying appropriate security policies and updates.

- **192.168.30.0 - Guest Network**
  - **Tool**: Netdiscover
  - **Command**:
    ```bash
    sudo netdiscover -r 192.168.30.0/24
    ```
  - **Purpose**: Guest networks should be isolated from your main networks. Scanning this subnet helps ensure that guest devices do not cross into more sensitive network zones.

### Stage 2: Port and Service Scanning

**Objective**: Identify open ports and services running on each device within the segmented subnets. This detailed examination helps to understand potential vulnerabilities and ensure that only necessary ports are open, according to the security policies.

#### Steps for Each Subnet:

- **192.168.0.0 - Network Hardware**
  - **Tool**: Nmap
  - **Command**:
    ```bash
    sudo nmap -sV -T4 192.168.0.0/24
    ```
  - **Purpose**: Targets network infrastructure devices to ascertain what management interfaces or protocols are exposed.

- **192.168.10.0 - Computers, Phones, iPads**
  - **Tool**: Nmap
  - **Command**:
    ```bash
    sudo nmap -sV -T4 192.168.10.0/24
    ```
  - **Purpose**: Scans personal devices to check for open ports and services that might not be necessary and could compromise security, such as remote desktop ports, FTP servers, or unnecessary web servers.

- **192.168.20.0 - IoT Devices**
  - **Tool**: Nmap
  - **Command**:
    ```bash
    sudo nmap -sV -T4 192.168.20.0/24
    ```
  - **Purpose**: IoT devices are particularly vulnerable and often are not regularly updated. This scan helps identify risky services that shouldn’t be exposed to the network.

- **192.168.30.0 - Guest Network**
  - **Tool**: Nmap
  - **Command**:
    ```bash
    sudo nmap -sV -T4 192.168.30.0/24
    ```
  - **Purpose**: Ensures that guests do not have access to services that could jeopardize the main network. This scan checks for isolation compliance and identifies any misconfigurations.

### Stage 3: Vulnerability Assessment

**Objective**: Conduct a thorough assessment of potential security vulnerabilities within each subnet of your network to identify and mitigate risks effectively.

**Prelude to Vulnerability Scanning**:
- **Update OpenVAS**: Before beginning your scans, make sure your vulnerability scanner is up-to-date to leverage the latest security databases and scanning technology.
  ```bash
  sudo gvm-feed-update
  ```
- **Upgrade OpenVAS**:
  ```bash
  sudo gvm-setup
  sudo gvm-start
  ```

### Performing the Scans:

While it's technically feasible to scan all subnets at once, doing so might not be the most efficient or gentle on your network's performance. Scanning each segment individually allows you to manage the load and focus on analyzing the results more meticulously, which can be crucial when dealing with different types of devices and their specific vulnerabilities.

#### Steps for Each Subnet:

1. **192.168.0.0 - Network Hardware**
   - **Purpose**: Focus on critical infrastructure vulnerabilities.
   - **Scan Setup**: Configure your scan to target only the IP range of 192.168.0.0/24.

2. **192.168.10.0 - Computers, Phones, iPads**
   - **Purpose**: Look for vulnerabilities in personal devices that could be exploited.
   - **Scan Setup**: Target the scan specifically for the 192.168.10.0/24 subnet.

3. **192.168.20.0 - IoT Devices**
   - **Purpose**: IoT devices often have unique vulnerabilities, making them frequent targets for attacks.
   - **Scan Setup**: Conduct a dedicated scan for the 192.168.20.0/24 subnet.

4. **192.168.30.0 - Guest Network**
   - **Purpose**: Ensure guest devices do not compromise the network.
   - **Scan Setup**: A focused scan on 192.168.30.0/24 helps verify isolation and security controls.

### Additional Considerations:

- **Scheduling**: Depending on the size of your network and the number of devices, consider scheduling scans during off-peak hours to minimize impact on network performance.
- **Regular Scanning**: Set up a regular schedule for scanning each subnet. This not only helps in maintaining security but also tracks the progress of remediation efforts.

### Stage 4: Monitoring and Maintenance

**Objective**: Continuously monitor the network to detect any unusual activities, changes, or emerging vulnerabilities and maintain the security posture over time through regular updates and patching.

#### Key Components to Automate:

1. **Continuous Scanning**:
   - **Tools**: Use cron jobs in Linux to schedule regular scans with Nmap and OpenVAS.
   - **Nmap Example**:
     ```bash
     0 2 * * * sudo nmap -sV -T4 192.168.0.0/24 >> /var/log/nmap/network-hardware.log
     ```
     This command runs a scan every night at 2 AM and logs the results, adjusting the subnet as necessary for other network segments.
   - **OpenVAS Example**:
     Automate scanning in OpenVAS using GVM command-line tools or by setting schedules directly in the GVM web interface.

2. **Log Monitoring**:
   - **Tools**: Use tools like Logwatch, Graylog, or ELK Stack to analyze logs generated by your network devices, firewall, and intrusion detection systems.
   - **Setup**:
     Configure these tools to parse and alert based on specific events or anomalies detected in the logs.

3. **Performance Monitoring**:
   - **Tools**: Utilize network monitoring tools like Nagios, Zabbix, or PRTG.
   - **Purpose**:
     Monitor network health, bandwidth usage, device status, and other performance metrics to catch issues before they become critical.

4. **Update Checks**:
   - **Automation**:
     Set up scripts to check for updates for your network devices’ firmware, software on your systems, and applications.
     ```bash
     0 4 * * SUN sudo apt update && sudo apt full-upgrade -y
     ```
     This cron job, for example, runs weekly to update your Kali Linux system.

5. **Incident Response**:
   - **Tools**: Integrate your monitoring tools with incident response platforms or scripts that can automatically take action based on specific triggers.
   - **Example**:
     Automatically isolate a device from the network if suspicious activity is detected.

#### Documentation and Reporting:
- **Regular Reviews**: Schedule monthly or quarterly security reviews to go over the logs, the outcomes of the scans, and any incidents.
- **Updates**: Keep a change log to track any updates or modifications made to the network or policies.

