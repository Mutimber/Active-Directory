# ActiveDirectory Lab
## Network Diagram
![image](https://github.com/Mutimber/Active-Directory/assets/113706552/38a6abbd-bb32-44ff-a1d8-e9e7e8f1435f)

## Install Virtual machines
- Download virtualbox at https://www.virtualbox.org/
- Checksum SHA256 to ensure correct download.
- Install virtualbox
- Download windows iso image at https://www.microsoft.com/en-ca/software-download/windows10
  - First, download windows media creation tool to help with iso file installation.
![image](https://github.com/Mutimber/Active-Directory/assets/113706552/cb56f32d-52ac-48f2-b721-50a892df4b1a)
  - Follow install instructions
  - Create installation media and select Next
  - Choose iso file and place file anywhere.
  - Add the new windows machine on virtual box, Windows 10
  - start the VM and set it up as Windows 10 prod with the necessary settings for the lab.
- Install prebuilt Kali
  - Download machine based on comp specs at https://www.kali.org/get-kali/#kali-virtual-machines
  ![image](https://github.com/Mutimber/Active-Directory/assets/113706552/e6670281-a76d-42bb-a065-391fc74a0147)
  - Once downloaded, add the vbox image as a VM on the virtualization platform
  - kali has default credentials kali/kali
- Install Windows 2022 server iso - will have Active Directory 
  - Download from https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022
    ![image](https://github.com/Mutimber/Active-Directory/assets/113706552/e96dd20d-aa97-43f8-848c-cce0ea4474a7)
  -  Click Download iso and fill the table on the next page at https://info.microsoft.com/ww-landing-windows-server-2022.html
  -  Download the 64 bit version in the next page. https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022
 ![image](https://github.com/Mutimber/Active-Directory/assets/113706552/75387ac8-b995-4f44-8b8b-754c1a0d6561)
  - Add to Vbox and configure the Windows server accordingly.

- Install Ubuntu  server - our Splunk server
  - Go to ubuntu on the browser and select server: https://ubuntu.com/server
  - Download and configure
  - Enter credentials username, server name & password
  - Reboot when prompted then log in
  - update the machine

        sudo apt-get update && sudo apt-get upgrade
    
## Install and Configure Sysmon and Splunk
### To be installed on Windows target machine (Windows 10) and Windows server (Active Directory)
- First, on  virtualbox, ensure all machines' network settings are set to NAT Network
  ![image](https://github.com/Mutimber/Active-Directory/assets/113706552/19aadd87-dff1-4d99-b67e-24b271e8beb4)
- Change all machines in the lab to this network
  ![image](https://github.com/Mutimber/Active-Directory/assets/113706552/3ff65c2e-0504-4558-9049-1b1532222cca)
- When we check IP of the splunk machine, it gives a different IP from the static one already proposed in the network diagram.
- Change it by editing /etc/netplan/00-installer-config.yaml file

      sudo nano /etc/netplan/00-installer-config.yaml
  ![image](https://github.com/Mutimber/Active-Directory/assets/113706552/f9485904-07bc-4fbe-ad31-14c6c6081968)
- Changes implemented

![image](https://github.com/Mutimber/Active-Directory/assets/113706552/5f5320ee-69d3-453c-9534-69cc418405db)

### Splunk
- On host, go to splunk.com and sign up with a corporate email and verify
- Back to Products,choose Free Trial and Downloads
- On Splunk Enterprise, select Get My Free Trial
![image](https://github.com/Mutimber/Active-Directory/assets/113706552/e73422b7-2d91-4848-a2a9-5ad400f0a19c)
- Select Linux machine
- Download deb file
![image](https://github.com/Mutimber/Active-Directory/assets/113706552/f65d0a31-2370-436c-b0a3-ff76f93323e7)
- Back on splunk vm, install guest add-ons for virtualbox

      sudo apt-get install virtualbox-guest-additions-iso

- After install, go to Devices > Shared Folders > Add Folder by clicking on the green + icon on the right
![image](https://github.com/Mutimber/Active-Directory/assets/113706552/940a2402-113c-446e-b470-14a419b9e90e)
- As below

![image](https://github.com/Mutimber/Active-Directory/assets/113706552/e59a5804-7dc2-46f1-93a6-fdddb35c4854)
- Reboot splunk vm
- Now adduser

      sudo adduser robin vboxsf
![image](https://github.com/Mutimber/Active-Directory/assets/113706552/f75452e7-d3b6-4b28-a9f9-96f57c9dbc8f)

- Create new directory called share

      mkdir share
- Mount shared folder onto share directory

      sudo mount -t vboxsf -o uid=1000,gid=1000 F_DRIVE share/
  ![image](https://github.com/Mutimber/Active-Directory/assets/113706552/fc4fab83-5c5a-4a65-9c25-5fc96259ea60)

- Change into share directory to view files and folders using ls -la command. Splunk installer downloaded before is among these as shown below.

 ![image](https://github.com/Mutimber/Active-Directory/assets/113706552/2ae34d9a-72d0-41bb-9771-1a623dfcc7c2)

- Now, install splunk

      sudo dpkg -i splunk-9.2.1-78803f08aabb-linux-2.6-amd64.deb
![image](https://github.com/Mutimber/Active-Directory/assets/113706552/5bd8b9cd-3bbc-4251-8390-2bbc96f43acd)

- To view splunk files. All permissions are splunk
![image](https://github.com/Mutimber/Active-Directory/assets/113706552/73f4afc6-68cc-4a1c-858f-b606cbfa5bf0)
- Enter splunk user

      sudo -u splunk bash
  ![image](https://github.com/Mutimber/Active-Directory/assets/113706552/8ebc9bbf-47f3-4868-a0ee-46be99e84203)

- Change into /bin
- then run the installer

      ./splunk start
- Set up entering credentials, i.e., name and password

      exit
      cd bin
      sudo ./splunk enable boot-start -user splunk
- The last command ensures that whenever the VM boots, splunk runs with the user splunk
![image](https://github.com/Mutimber/Active-Directory/assets/113706552/40816e06-8783-4803-80be-553ca5bc575a)

### Target Machine
- Rename PC to target-PC and restart machine
- Check ip on the command prompt

        ipconfig
![image](https://github.com/Mutimber/Active-Directory/assets/113706552/c7849408-708d-415b-8274-161b73f27586)
- Change Adapter settings
  
![image](https://github.com/Mutimber/Active-Directory/assets/113706552/17d2b5bc-4c90-4c15-b53a-e0733b2a774d)

- On the target machine, login to the splunk machine http://192.168.10.10:8000 and log in using credentials made in the previous section to access the splunk GUI.
- Then, log in to splunk.com using account details and download the Universal Splunk Forwarder.
![image](https://github.com/Mutimber/Active-Directory/assets/113706552/c9773c22-3740-4581-aad2-c74c2f4b2431)
- Choose the 64-bit version
![image](https://github.com/Mutimber/Active-Directory/assets/113706552/f179321b-49a6-45a2-a520-26ec91f71a8e)
- After download, double-click on splunk> universal forwarder, choosing an on-premises Splunk Enterprise instance in the configuration.
- Add credentials and select Generate password
- Skip Deployment server
- For receiving indexer is splunk server IP, that is, 192.168.10.10 on port 9997
- Click install
![image](https://github.com/Mutimber/Active-Directory/assets/113706552/ffd76a8a-88b8-437a-abde-66a96854b23c)
- Login to splunk

![image](https://github.com/Mutimber/Active-Directory/assets/113706552/ce0ea50f-9adc-4f3f-817a-6e5b68f794f7)

- Download sysmon
- Download raw sysmon conf from https://raw.githubusercontent.com/olafhartong/sysmon-modular/master/sysmonconfig.xml
- Configure sysmon
- Configure inputs.conf file to instruct splunk forwarder
- File location C:/Programs Files/SplunkUniversalForwarder/etc/system/default/inputs
  ![image](https://github.com/Mutimber/Active-Directory/assets/113706552/4b02e128-5889-4693-8b30-2603d93dbc7f)
- Restart the Splunk Forwarder service
  ![image](https://github.com/Mutimber/Active-Directory/assets/113706552/2f0b0de4-b93a-475b-bd56-e54346dc913a)
- In splunk, create a new index to capture forwarder details as was the case inputs.conf => endpoint
![image](https://github.com/Mutimber/Active-Directory/assets/113706552/a13fa7c8-4935-4506-85d8-fd8d621afaee)



