# Building a Cybersecurity Lab: Active Directory, Splunk, Atomic Red Team, and Kali Linux Integration

I have successfully built a lab environment centered around Active Directory, Splunk, Atomic Red Team, and Kali Linux. The setup includes an Active Directory environment hosted on a Windows Server, with Splunk configured to ingest and analyze event data from both the Active Directory server and a target Windows machine. Additionally, Atomic Red Team is integrated to simulate various attack techniques, and a Kali Linux machine is used to perform a brute-force attack against the Active Directory environment. This documentation provides a detailed guide to setting up and configuring these components, offering practical insights into cybersecurity practices.

## Tools

- **Virtualization:** VirtualBox
- **Client Machines:**
  - Windows Server 2022 (Active Directory)
  - Target Windows 10 Machine
  - Kali Linux (Attacking Machine)
- **Security Monitoring:** Splunk (Installed on Ubuntu Server )
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

## Installing Windows 10 Virtual Machine

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
 
## Installing Windows Server 2022  

1. Navigate to the [Microsoft Evaluation Center](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022) and download the ISO file.
2. Fill in the required information and select "64-bit Edition" under ISO downloads.
3. Click on the "New" icon in VirtualBox, provide a name for your virtual machine (e.g., "ADDC01"), select the ISO image you downloaded, and choose "Skip unattended installation".

   ![3](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/fa2565ef-2814-454b-a427-893b84d888ea)

4. Assign 4GB to memory, leave the hard disk at 50GB, then select "Finish".

   ![4](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/1544a40c-ad42-4a12-af1a-29dbe5ccd92c)

   ![5](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/6c5c19b0-e2d1-43ca-ac54-0f9fecf092da)

5. Start the virtual machine and then select "Install now."
6. Choose "Windows Server 2022 Standard Evaluation (Desktop Experience)" as the operating system and hit next.

   ![6](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/94c5c869-1196-486a-b734-9c827e8c5eab)

7. Accept the terms and agreements.
8. Select "Custom" for installation and hit next.

   ![8](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/77c4a559-61c7-485b-9e2e-ec6ca2ed5110)

9. After the setup is completed, create a secure password and click on "Finish."
10. Log in using the password you just created.

## Installing Ubuntu Server

1. Go to [ubuntu.com](https://ubuntu.com/), navigate to the 'Products' section, and choose 'Ubuntu Server'. Download version 22.04 from there.
2. The installation process is similar to that of other virtual machines. Set the memory to 8GB, processors to 2, and allocate 100GB for the hard disk.
3. Start the virtual machine.
4. Proceed with the installation. When prompted for profile setup, create a username and password.
5. Once the installer is complete, select "Reboot Now."
    
    ![10](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/e8eb777b-8428-4674-89bc-0f7a1247c433)
6. If you encounter an error unmounting the CD-ROM, simply press Enter.
7. Once Ubuntu finishes rebooting, you will be presented with the login screen. Use the account you created earlier to log in.

   ![11](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/246bb10e-d5e6-4c4b-a609-ff57c0f7197a)

8. After logging in, run the command `sudo apt-get update && sudo apt-get upgrade -y` to update and upgrade your system packages.

## Creating NAT Network

To create a NAT network and configure network settings in VirtualBox for ensuring that virtual machines can be on the same network with internet access, follow these steps:

- Click on "Tools" in VirtualBox, then select Bullet Points.
- Choose the "Network" option.

   ![13](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/b0d1257e-6071-4df5-a48a-8e7e918f2990)

- Select the "NAT Networks" tab.
- Click on "Create".
- Provide a name and an IPv4 prefix for the new NAT network.
- Leave "Enable DHCP" checked.
- Click "Apply" to save the configuration.

   ![12](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/1bb575bf-0e5a-4663-9683-0d44f1ddb674)

- For each virtual machine, navigate to the settings and under the network settings, choose "NAT Network" for Adapter 1.

   ![14](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/5edb66d3-f840-44f5-b352-ea79547de07e)

## Setting up static IPs

#### Splunk Server
    
  - Head over to the Splunk server (Ubuntu Server) and run `sudo nano /etc/netplan/00-installer-config.yaml`.
  - Configure the file as follows:
    
     ![15](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/a786b255-8b4d-4fb4-90f5-9f51c5e5f0a8)


 - Save the changes and then run `sudo netplan apply`.
 - Now, run `ip a` to view the IP assigned to the server.

    ![16](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/ac1877dc-e52e-43f5-81ab-bdbe12c15457)

#### Target Windows 

 - On the target Windows machine go to **Network & Internet Settings**
 - Click on **Change adapter options**.
 - Right-click the network adapter and select **Properties**.
 - Double-click on **Internet Protocol Version 4 (TCP/IPv4)**.
 - Click on **Properties**.
 - Select **Use the following IP address** and set a static IP.
 - Hit **OK**.

   ![27](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/25a65bb4-1300-4fbb-83ae-20a34c6f1a0c)

#### Windows Server

- Follow the same steps as outlined for the Target Windows machine to set a static IP address for the Windows Server.

  ![37](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/79f4d29b-0464-4154-86a6-550a62500934)

  
## Installing Splunk

1. On your host machine, navigate to [splunk.com](https://www.splunk.com/), sign up, and log in.
2. Go to the "Products" section and select "Free Trials & Downloads."
3. Scroll to Splunk Enterprise and select "Get My Free Trial."
4. Select Linux as the operating system and download the .deb file.
5. Go back to the Splunk virtual machine and run the following command to install the Guest Additions for VirtualBox:
  
   ```sh
   sudo apt-get install virtualbox-guest-additions-iso
   sudo apt-get install virtualbox-guest-utils
   ```
6. On the top of the VirtualBox window, click on Devices --> Shared Folders --> Shared Folders Settings.
7. Add the folder where the Splunk installer is located, then check "Read-only," "Auto-mount," and "Make Permanent," and click OK.
   
    ![19](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/3e3c3fc8-66ba-444c-bb27-8567a68064f1)

8. Back on the Splunk virtual machine, run `sudo reboot`.
9. After reboot, add our user to the vboxsf group:

   ```sh
   sudo adduser <username> vboxsf
   ```
10. Create a new directory called "share":

    ```mkdir share```

11. Mount our shared folder to the directory called "share":
  
    ```sh
    sudo mount -t vboxsf -o uid=1000,gid=1000 <shared_folder_name> share/
    ```
   ![20](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/b965796f-aed2-42ef-917a-db566efdaad4)

12. Change the directory to "share" and install Splunk using dpkg.

    ```sh
    sudo dpkg -i splunk*.deb
    ```
   ![23](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/ed4fe38c-3428-4b30-9fff-f8f06526c761)

13. After the installation is complete, navigate to the Splunk installation directory on our server located at `/opt/splunk`.
14. Switch to the Splunk user using the command:

    ```sh
    sudo -u splunk bash
    ```
15. Change to the `bin` directory and then run the command: `./splunk start`, and subsequently agree to the license.
16. Choose an administrator username and password.
17. After completing the installation, exit the Splunk user session by typing 'exit'. Then navigate to the 'bin' directory and run:
   
    ```sh
    sudo ./splunk enable boot-start -user splunk
    ```

    ![24](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/d3b3c068-4d45-42ac-ab40-81f29b721d53)


## Installing Splunk Universal Forwarder 

### On Windows 10

1. On the Windows machine, change the hostname to "target"
2. Go to [splunk.com](https://www.splunk.com) and log in.
3. Navigate to **Products** and select **Free Trials & Downloads**.
4. Scroll down to **Universal Forwarder** and download the forwarder for the Windows operating system.
   
  ![28](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/9d75325b-039c-4b3c-ac71-6eb9b3157e97)

5. Once the download is complete, double-click the `.msi` file.
6. Select an on-premises Splunk Enterprise instance. Go through the installation process. For the username, give "admin", skip the deployment server, and for the receiving indexer, use your Splunk server IP with the default port 9997.

    ![29](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/f6c8d694-bc88-4028-870a-09f16dbd4975)

### Configuring Splunk Universal Forwarder on Windows 10

We need to instruct the Splunk Universal Forwarder on what data to send to our Splunk server. To do this, we must configure a file called `inputs.conf` that is located under `C:\Program Files\SplunkUniversalForwarder\etc\system\default`.
1. Open Notepad with administrative privileges.
2. Copy the content of `inputs.conf` and paste it into Notepad.
3. Modify the contents as follows:

```sh
[WinEventLog://Application]
index = endpoint
disabled = false

[WinEventLog://Security]
index = endpoint
disabled = false

[WinEventLog://System]
index = endpoint
disabled = false

[WinEventLog://Microsoft-Windows-Sysmon/Operational]
index = endpoint
disabled = false
renderXml = true
source = XmlWinEventLog:Microsoft-Windows-Sysmon/Operational
```

4. Save the modified file as `inputs.conf` under `C:\Program Files\SplunkUniversalForwarder\etc\system\local\`.

- **Note:** Do not change the configuration file under the `default` directory to avoid potential issues.

5. After updating the `inputs.conf` file, restart the Universal Forwarder services:

   - Open the Services application with administrative privileges.
   - Search for "SplunkForwarder" in the list of services.
   - Double-click on "SplunkForwarder" to open its Properties window. Go to the Log On tab.
   - Select the "Local System account" option. Click Apply and then OK.
   - Right-click on "splunkforwarder" again in the Services window and select Restart.
   - If you get a warining that windows could not stop the SplunkForwarder, hit OK and start the service.

#### Configuring Index on Splunk Server
  
1. Head over to the Splunk server web portal at 192.168.10.10:8000, and log in with the credentials we created during the Splunk installation on our Ubuntu server.

   ![31](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/59a2b395-d640-4bb3-b6d0-4da966496232)

2. Go to Settings at the top and select Indexes.
3. Click on "New Index" to create an index called "endpoint" because in our inputs.conf file, all events are sent to an index named "endpoint".

   ![33](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/aae98519-545b-419f-8554-78a642c27a2b)

4. Enable the Splunk server to receive the data. Go to Settings, click on "Forwarding and receiving", under "Receive data" click on "Configure receiving", then click on "New Receiving Port", enter "9997", and hit "Save".

   ![34](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/bebcc78a-15f9-42e1-bf83-275ff6c2007d)

5. Click on "Apps" in the top left corner, then select "Search & Reporting". Under the search bar, type `index=endpoint` and click on the search button. Now you can see some events

   ![35](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/2e9158b9-3f85-4a80-97b4-15898585f38f)


### On Windows Server

1. On the Windows Server, change the computer name to ADDC01.
2. Install the Splunk Universal Forwarder and configure the `inputs.conf` file the same way as on the target Windows machine.
3. Then, go to the Splunk server web portal at `192.168.10.10:8000`, click on "Apps" in the top left corner, and select "Search & Reporting." Under the search bar, type `index=endpoint`. This time, you will see two hosts.

   ![39](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/b4cede0a-56d6-47e6-b51c-38793569c36a)


## Installing Sysmon 

### Windows Installation

1. On your Windows virtual machine, open a web browser and navigate to the official Microsoft Sysinternals [website](https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon).
2. Download Sysmon from the website.
3. Navigate to the [GitHub page](https://github.com/olafhartong/sysmon-modular) where `sysmonconfig.xml` is available and download it to your Windows virtual machine.
4. Locate the downloaded Sysmon ZIP file and extract its contents.
5. Open Powershell with administrative privileges.
6. Navigate to the directory where Sysmon is extracted.
7. Place the `sysmonconfig.xml` file in the same directory where Sysmon is extracted.
8. Run the command to install Sysmon with a basic configuration:

``` .\Sysmon64.exe -i .\sysmonconfig.xml ```

![9](https://github.com/FrezsSec/Setting-Up-SOC-Automation-with-Wazuh-TheHive-and-Shuffle/assets/173344802/37251597-a518-4c87-849a-5f4a5795a9d4)

7. Verify that Sysmon has been installed correctly by checking the Services application.

### Windows Server Installation

- Follow the same steps as outlined above for the Windows virtual machine to install Sysmon on the Windows Server.

## Install Active Directory on Windows Server

1. Go to **Server Manager**.
2. In the top right corner, click **Manage**.
3. Select **Add Roles and Features**, then hit **Next**.

    ![40](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/3df4c46b-db34-4185-874d-06190300a9c9)

5. Make sure **Role-based or feature-based installation** is checked.

   ![41](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/462d28e0-1f3b-4a82-976b-3495f5968f72)
