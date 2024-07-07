# Building a Cybersecurity Lab: Active Directory, Splunk, Atomic Red Team, and Kali Linux Integration

I have successfully built a lab environment centered around Active Directory, Splunk, Atomic Red Team, and Kali Linux. The setup includes an Active Directory environment hosted on a Windows Server, with Splunk configured to ingest and analyze event data from both the Active Directory server and a target Windows machine. Additionally, Atomic Red Team is integrated to simulate various attack techniques, and a Kali Linux machine is used to perform a brute-force attack against the Active Directory environment. This documentation provides a detailed guide to setting up and configuring these components, offering practical insights into cybersecurity practices.

## Tools

- **Virtualization:** VirtualBox
- **Client Machines:**
  - Windows Server (Active Directory)
  - Target Windows 10 Machine
  - Kali Linux (Attacking Machine)
- **Security Monitoring:** Splunk (Installed on )
- **Attack Simulation:** Atomic Red Team and Kali linux
- **Telemetry and Logging:**
  - Sysmon (Installed on Windows Server and Target Windows Machine)
  - Splunk (Collecting logs from Windows Server and Target Windows Machine)
 
## Network Design

 ![Topology drawio](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/6f24f624-0419-444c-887f-93e935c331dc)

# Installation Guide

In this section, we will cover the installation steps for each component of our SOC automation setup.

## Installing VirtualBox on Windows

 - Visit the [VirtualBox website](https://www.virtualbox.org/) and download the latest version for Windows.
 - Check the SHA-256 checksum of the downloaded installer using PowerShell and Compare the hash with the one provided on the VirtualBox website to ensure the file integrity hasn't been compromised.

    ```sh
    Get-FileHash VirtualBoxInstallerFileName (replace VirtualBoxInstallerFileName)
    ```
 - Double-click the downloaded installer file and follow the on-screen prompts to complete the installation, including any necessary dependencies like Microsoft Visual C++ 2019. Once installed, VirtualBox will launch automatically.

## Creating a Windows 10 Virtual Machine

1. Navigate to this [link](https://www.microsoft.com/en-us/software-download/windows10) and download the Media Creation Tool to generate the Windows 10 ISO image.
2. Open VirtualBox and click "New" to create a new virtual machine. Select a name for the virtual machine (e.g., Windows 10). Then browse to the ISO image folder, double-click it, and tick "Skip unattended installation." Finally, click "Next."

![3](https://github.com/FrezsSec/Setting-Up-SOC-Automation-with-Wazuh-TheHive-and-Shuffle/assets/173344802/c4cba61b-0cbc-4ed3-8e1f-24ce6ba04608)

3. Assign 4 GB of RAM to your virtual machine, or adjust this value based on your available resources, and then click "Next."
4. For the virtual disk, leave the size as 50 GB and click "Next."
5. Now you will see a summary of your settings. If everything looks good, click "Finish."

![6](https://github.com/FrezsSec/Setting-Up-SOC-Automation-with-Wazuh-TheHive-and-Shuffle/assets/173344802/f89a831c-f26b-4866-975d-ceb05f7b76ab)

7. Start the virtual machine and then click **"Install Now"**. When presented with the **"Activate Windows"** screen, select **"I don't have a product key"** and choose **"Windows 10 Pro"** for the edition. Next, accept the license terms. Then, select **"Custom: Install Windows only (advanced)"** and click **"Next"**. Windows will now begin the installation process.

## Installing Kali Linux
1. Visit the [Kali Linux](https://www.kali.org/get-kali/#kali-platforms) download page and select the Pre-built Virtual Machine.

   ![1](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/1756a760-ed35-4c7a-bb8a-8d39910e8833)
   
2. Download the pre-built VirtualBox image (32-bit or 64-bit).
3. Right-click the downloaded Kali Linux file and extract it.
4. Navigate to the extracted folder, locate the .vbox file, and double-click it to import the VM into VirtualBox.
5. Open VirtualBox, select the imported Kali Linux VM, and click "Start".
6. Use the default credentials (username: **kali**, password: **kali**) to log in.
 
    
