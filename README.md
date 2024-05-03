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
- First, on  virtualbox, ensure all machines' network settings are set to NAT
  ![image](https://github.com/Mutimber/Active-Directory/assets/113706552/87b5fb27-48ad-4306-821e-0dd91c9593de)
- 
