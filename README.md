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

6. Start the virtual machine and then click **"Install Now"**. When presented with the **"Activate Windows"** screen, select **"I don't have a product key"** and choose **"Windows 10 Pro"** for the edition. Next, accept the license terms. Then, select **"Custom: Install Windows only (advanced)"** and click **"Next"**. Windows will now begin the installation process.

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

#### Kali Linux

1. Right-click on the ethernet icon at the top of the screen and select "Edit Connections".

   ![58](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/a522ba5f-3887-4e00-995f-3b67ae755718)


3. Choose the first profile from the list.
4. Click on the cog icon at the bottom left.

   ![56](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/af07cfb8-90c0-41af-aa8a-749432134c86)


6. Select the "IPv4 Settings" tab.
7. Set the method to "Manual" and add the desired IP address.

   ![57](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/548d9acd-163c-4b93-84a8-a9397abd9872)

9. Click "Save".
10. Click on the ethernet icon again, disconnect and then reconnect.
11. Run ifconfig to check the IP address configuration

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

   ![69](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/064f151a-da52-4a42-9259-01131f55508b)


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

9. Verify that Sysmon has been installed correctly by checking the Services application.

### Windows Server Installation

- Follow the same steps as outlined above for the Windows virtual machine to install Sysmon on the Windows Server.

## Install Active Directory on Windows Server

1. Go to **Server Manager**.
2. In the top right corner, click **Manage**.
3. Select **Add Roles and Features**, then hit **Next**.

    ![42](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/87159e14-0bb4-4baa-981c-9aabb0647821)


4. Make sure **Role-based or feature-based installation** is checked.Then select 'Next'.

   ![41](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/462d28e0-1f3b-4a82-976b-3495f5968f72)

5. For the roles, select **Active Directory Domain Services**, then click on **Add Features**.

   ![43](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/1fa947ff-2896-4238-8a2b-3e00f60a0aed)

6. Continue clicking **Next** until you reach the **Install** button.

   ![45](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/985e6654-3413-4a3a-bae8-fce37da08de4)

7. Click on the flag icon beside **Manage**, then click on **Promote this server to a domain controller**.

   ![46](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/fcddd69c-0bee-451b-8792-87b87d6ba510)

8. Select **Add a new forest** and provide a domain name (e.g., example.local). Then hit 'Next'.

   ![47](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/25f3fe01-7214-4f94-af45-cd97f7f5e12a)

9. Set a password.
10. Continue clicking **Next** until you reach the **Install** button.

Once the installation is complete, the server will automatically restart. After the restart, log back into the server. You will see the domain name on the login page, indicating that we have successfully installed Active Directory Domain Services (ADDS) and promoted our server to a domain controller.

   ![48](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/6aadbe5d-122b-4cea-a18a-59b6c8ac2d4e)

### Creating Users

1. In Server Manager, click on "Tools" in the top right corner, then select **Active Directory Users and Computers**.
2. Right-click on the domain, navigate to **New**, and select **Organizational Unit**. Name it, for example, "IT".

   ![49](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/1610acce-f4f8-4aff-aa48-6e2469a10c5e)

3. Right-click on the "IT" unit, then under "New", select **User**.
4. Create a username and password for the new user.

   ![50](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/44413aa5-061a-4da3-840a-a5d54c2dae38)

5. Create another organizational unit named "HR", and create a new user account within this unit.

### Joining Windows 10 to the Domain

1. On the Windows 10 machine, search for "PC" and go to Properties.
2. Scroll down and select "Advanced system settings".
3. Go to the "Computer Name" tab, select "Change".
4. Select "Domain" and type your domain name.

   ![51](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/c11a5f29-faca-45f5-9e60-34308bb84190)

#### Troubleshooting Domain Connection Issues

If you get an error stating that an Active Directory Domain Controller for the domain could not be contacted, it is likely because your target machine does not know how to resolve the .local domain. To fix this:

    ![52](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/ad661cc2-d03d-4744-a595-9f938ab4e0d2)

 - Open "Network & Internet Settings".
 - Select "Change adapter options".
 - Right-click on the network adapter and select "Properties".
 - Double-click "Internet Protocol Version 4 (TCP/IPv4)".
 - Change the DNS server to the IP address of your domain controller.
 - Hit "OK".

   ![53](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/85f4549c-592d-4a5c-97db-787d615f2ef5)

#### Completing Domain Join

After updating the DNS settings, continue with the steps to join the Windows 10 machine to the domain:

 - Select "Domain" and type your domain name.
 - You will be prompted to provide a username and password. Use the administrator account credentials of the server to log in.
 - Once authenticated, restart the Windows 10 machine to apply the changes and join the domain successfully.

#### Logging in with a Domain User

After restarting the Windows 10 machine:

 - Once you reach the log on screen, select "Other User".
 - Ensure that the "Sign in to" field points to your domain.
 - Log in with the newly created user credentials in the domain.

This will allow you to access the Windows 10 machine as a domain user.


## Installing Brute-Force attack tool

1. Update and Upgrade Kali Linux:

   ```sh
   sudo apt-get update && sudo apt-get upgrade -y
   ```
2. Install Crowbar that is a tool used for brute force attacks:

   ```sh
   sudo apt-get install -y crowbar
   ```
3. Unzip the rockyou Wordlist File:

   ```sh
   sudo gunzip /usr/share/wordlists/rockyou.txt.gz
   ```
4. If you don't want to use all the passwords, you can extract the first 20 lines from rockyou.txt and save them into passwords.txt using the command:

   ```sh
   head -n 20 /usr/share/wordlists/rockyou.txt > passwords.txt.
   ```
5. For the sake of this lab, you can also add the password of one of the users you created to the passwords.txt file.
   
#### Enabling RDP on the Target Windows

  - On the target Windows machine, search for "PC" and go to Properties.
  - Scroll down and click on Advanced system settings.
  - Enter the administrator username and password if prompted.
  - In the System Properties window, go to the Remote tab.
  - Select Allow remote connections to this computer.

     ![59](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/99d2458e-d33f-4bed-953f-8edf7c60fcab)

  - Click on Select Users, then Add, and add the two usernames you previously created.

     ![60](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/c227606a-b6b2-4cc0-bb6a-c66aa89f2dd4)

  - Click OK, then Apply.

Now we proceed with our brute force attack:

5. Launch Crowbar and perform a brute-force attack on the target Windows machine using the RDP protocol.

    ```sh
    sudo crowbar -b rdp -u <username> -C passwords.txt -s <Target_IP>
    ```

   ![61](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/ee51aed8-4d85-44a9-b9a9-77abfa301f5c)

## Analyzing Telemetry Data in Splunk

  - Head over to the Splunk server web portal at http://192.168.10.10:8000 and log in with your credentials.
  - Select "Search & Reporting".
  - Narrow down the search for events related to the user `tsmith` within the `endpoint index`, focusing on the last 15 minutes.

     ![68](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/fe286603-6282-4500-a5a8-130ef06e7161)
  
  - Click on the EventCode on the left. You will see a total count of 20 events with Event ID 4625, indicating 20 failed login attempts for the user tsmith.

     ![64](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/1c7b6bb4-c4df-44f1-8a53-faf0cf98e6c5)

  - We select Event ID 4625, and upon scrolling down, we notice that all the events occurred nearly simultaneously, which is a clear indication of brute-force activity.

     ![65](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/dc60f486-4383-4bb4-b92a-0dbcdfdad045)

- Now let's take a look at Event Code 4624, where we have one event indicating a successful account login.

     ![66](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/92cdf6d2-c0b2-4b0e-92bb-2926ac344f3a)

- We can expand the event and scroll down to see that the workstation is identified as "kali," along with the IP address from which the login attempt was made.

     ![67](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/98fbca16-0b6a-438e-a8c8-f0f54f7b9f2a)

## Installing Atomic Red Team on Windows Target

1. Open PowerShell with administrator privileges on the Windows target. Log in with an administrative account, and run the following command:

   ```sh
   Set-ExecutionPolicy Bypass CurrentUser
   ```
2. Before installing the Atomic Red Team, we are going to set an exclusion for the entire C drive, as Microsoft Defender may detect and remove some of the files. To do this, go to Windows Security → Virus & threat protection → Manage settings. Under Exclusions, add a folder exclusion and select the C drive.
3. To install Atomic Red Team, use the following command:

   ```sh
   IEX (IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1' -UseBasicParsing);
   ```
   ```sh
   Install-AtomicRedTeam -getAtomics -Force
   ```
   ```sh
   Import-Module "C:\AtomicRedTeam\invoke-atomicredteam\Invoke-AtomicRedTeam.psd1" -Force
   ```
4. Once the installation is complete, navigate to the C:\ drive. Go to the `AtomicRedTeam` directory and then click on atomics. You will see a bunch of technique IDs that map back to the [MITRE ATT&CK](https://attack.mitre.org/) framework. You can aslo list them via the command:
   ```sh
   Invoke-AtomicTest All -ShowDetailsBrief
   ```
   ![71](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/259a735a-8654-4c9f-b285-8bf72aa27287)

   
### Testing a Persistence Tactic: Creating a Local Account (T1136.001)

- Open PowerShell with Administrator privileges.
- Run:
  ```sh
  Invoke-AtomicTest T1136.001
  ```
  ![72](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/f73ff9b9-7c1b-4d4d-8b78-9d14d459d111)

#### Verifying Telemetry for Created Local Account

After running the atomic test to create a local account, telemetry data is generated and can be viewed in Splunk. Follow these steps to search for the new user account `NewLocalUser` in Splunk:
- Navigate to the Splunk web interface (e.g., `http://192.168.10.10:8000`).
- In the search bar, enter the following query to find events related to the newly created user `NewLocalUser`:

  ```sh
  index=endpoint NewLocalUser
  ```
   or
  
  ```sh
  index=endpoint EventCode=4720
  ```

   ![73](https://github.com/FrezsSec/Building-a-Cybersecurity-Lab-Active-Directory-Splunk-Atomic-Red-Team-and-Kali-Linux-Integration/assets/173344802/ee360b60-8570-4ab2-adb6-783bc4ce0ef9)

