Student Resources
-----------------

To make these demonstrations a valuable training experience you will need some software on the personal computer you use for your day-to-day activities. 


## User Accounts
There are several user accounts required to participate in the training. You may already have one or more of these accounts.

### WWT Digital Platform
The ATC is a collaborative ecosystem to design, build, educate, demonstrate and deploy innovative technology products and integrated architectural solutions for World Wide Technology customers.

Create an account on https://www.wwt.com/ using your business email address and complete your profile settings at https://www.wwt.com/my-account#MyProfile.

#### Launch the Lab

Search for `Ansible Student VM` and select the lab. It is at URL [ttps://www.wwt.com/lab/ansible-student-vm](https://www.wwt.com/lab/ansible-student-vm).

Launch the lab. Go to https://www.wwt.com/my-wwt/labs - Or select the labs page from the pop-up windows `Your virtual environment is ready. You can access it on the My Labs page.` Select `Open in ATC Lab Gateway`.  You may be prompted to authenticate with your email address and 14 character password.

Make a note of the IP address under `===Student VM Prblic IP===`. This IP address is the public IP address of your AWS EC2 instance for this lab. 

You will need this IP address to configure the VS Code remote development environment under section `Remote Explorer`.

### Version Control 
Both GitLab (GitLab.com) and GitHub (GitHub.com) provide Git Version Control repository management services over the Internet. If you do not already have an account on both services, sign up for both, ideally using the same handle (username) for your account on each service. For this training, the free account tier is all you need.

### Cisco DevNet
Cisco DevNet is Cisco's developer program to help IT professionals develop integrations with Cisco products. It also hosts learning tracts and sandbox environments. Join DevNet for free at https://developer.cisco.com/

## Install Software
Hands on experience (construction) is a key to learning a new topic. Because our 'hands are one keyboards' most of the day, the students of this training will benefit the most if they learn automation with their own version control accounts and development environment on their laptop. 

In addition to a computer running the macOS, Linux, and Windows 10 operating systems, the following software is required for this training environment.

### Visual Studio Code
VS Code is a free code editor, which runs on the aforementioned operating systems. Download and install it at https://code.visualstudio.com/download. 

While there are a number of suitable editors available for free or for a nominal license fee, this training makes use of the VS Code [Remote Development using SSH](https://code.visualstudio.com/docs/remote/ssh). The Visual Studio Code Remote - SSH extension allows you to open a remote folder on any remote machine, (virtual machine, or container) and terminal window with a running SSH server. 

Install the [Visual Studio Code Remote Development Extension Pack](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack).

As part of the training environment, a remote SSH host will be set up which includes a version of Ansible Engine and other necessary software.

There are a number of on-line tutorials to assist with setting up your laptop. See [Remote Development tutorials](https://code.visualstudio.com/docs/remote/remote-tutorials).

#### SSH client
Your laptop will need a supported SSH client. Refer to [Installing a supported SSH client]https://code.visualstudio.com/docs/remote/troubleshooting#_installing-a-supported-ssh-client).

##### macOS
For macOS, you should not need to install an additional ssh client. 

##### Windows
For Windows, installing Git for Windows is recommended. Refer to https://git-scm.com/download/win. 

Once installed, determine the absolute path of the `ssh` executable. It may be at location `c:\Program Files\Git\usr\bin\ssh.exe`.

Launch Visual Studio Code, in the lower left of the window, right click the gear icon and select `Settings`.  

In the search settings dialog box, type `remote.ssh.path`. In the resulting dialog box, enter the absolute path of your ssh client. For example ` C:/'Program Files'/Git/usr/bin/ssh.exe`. Note the back slash has been substituted by a forward slash and single quotes were added to bound the directory with embedded space. 

If you select the 'gear' icon next to the `remote.ssh.path` select `Copy Settings as JSON`. The copy buffer will contain:

```
"remote.SSH.path": " C:/\"Program Files\"/Git/usr/bin/ssh.exe"
```

Close the Settings window.

#### Remote Explorer
Prior to class you will be provided access to a remote Linux instance. To configure, select the 'Remote Explorer' icon on the left side of the window. Select the 'gear' icon to create a  'custom configuration file'. Provide an absolute path to the configuration file. For example: `C:\Users\kingjoe\.ssh\config`.

When selecting the '+' icon next to the SSH TARGETS, you will be prompted for an 'SSH Connection Command', enter `ssh training@example.net` an press enter to confirm your input.

In the SSH configuration file you will be given the connection information for the remote SSH host. The file entries will look similar to:
```
Host ec2-54-80-193-109.compute-1.amazonaws.com
  User ubuntu
  HostName ec2-54-80-193-109.compute-1.amazonaws.com
  IdentityFile ~/.ssh/devnet_sdk_demo.pem

Host 54.241.44.202
  Hostname 54.241.44.202
  User ubuntu
  IdentityFile ~/.ssh/student-key_Truist_ACI.pem
  ```

You will be provided a SSH private key file (PEM) for the training class, along with the values to specify for the user, host and hostname.

After saving the configuration file, the new host will appear in the SSH TARGETS window. Verify the configuration by '+ window' icon to connect to the host in a new window.


## Lab1

Logon your GitLab account, create a new project, (https://gitlab.com/projects/new) and name it `workshop`.  Set as 'public'.  Select the `Initialize with a README`. Select `Create Project`.

**Note:** Before uploading (pushing) to this repo, make the repo PRIVATE under `Settings -> General -> Visibility, project features, permissions`.

### Clone

Select `Clone` and copy the URL for `Clone with HTTPS`.

Your copy buffer should have something similar to the following, substituted with your username.
```
https://gitlab.com/joelwking/workshop.git
```

** From your terminal window in VS Code. **

#### Set global config values and clone
Use your GitLab username and email address, update the configuration on your remote instance
```
$ git config --global user.name "joelwking"
$ git config --global user.email joel.king@wwt.com
$ git config --list
```
Now clone the respository.
``` 
$ cd
$ git clone https://gitlab.com/joelwking/workshop.git
```
**Note:** So your don't forget make the repo PRIVATE under `Settings -> General -> Visibility, project features, permissions`.

### Download the workshop files
So we have some sample files to work from, do the following:

```
cd workshop
git clone https://gitlab.com/joelwking/cisco_dc_community_of_interest.git
```
To prevent us from accidentially uploading any sensitive data, we will ignore the lab files at this point.
```
echo "cisco_dc_community_of_interest" >>.gitignore
git status
```

### Install the Cisco ACI collection
Install the collection, by default this will install in your `~/.ansible` folder.
```
cd ~
ansible-galaxy collection install cisco.aci
```
From the terminal window, enter the following directory path.

`cd ~/workshop/cisco_dc_community_of_interest/demos/engine/playbooks/`

### Modify the sample playbook and files
Go to the main IDE window and select the `Remote Explorer` icon on the left, and open the `workshop` folder. 

Navigate down to `~/workshop/cisco_dc_community_of_interest/demos/engine/playbooks`

Open the sample.yml playbook. Below the `password` variable in the `aci_login anchor` add the following line.

```
private_key: "{{ lookup('file', '{{ playbook_dir }}/files/{{ apic_username }}.key') }}"
```
Save the file.

#### Create the key file
Under the `files`, create a new file called 'admin.key' and paste in the key provided. You will be given this key via email or another out-of-band method.

#### Null the existing credentials

From your IDE window, look at files/passwords.yml, this is a vaulted file. We aren't going to use this file now, rather I want to emphasis the fact credentials can be encrypted and stored in a repository.

From your terminal window, make a copy of the file and touch the file so the playbook can find an empty file.
```
$ mv files/passwords.yml files/passwords.yml-
$ touch files/passwords.yml
```

### Run the playbook
From the terminal window, in the `~/workshop/cisco_dc_community_of_interest/demos/engine/playbooks/` directory, execute the playbook.

```
$ ansible-playbook -v -i inventory.yml  ./sample.yml -e 'apic_hostname=sandboxapicdc.cisco.com'
```

Congratulation, you have run your first playbook! If all goes well, you have also automated the configuration of the Cisco DevNet always on ACI Sandbox.

In this example, the configuration data came from the `host_vars` directory. Using the IDE editor, review what you see in that directory.

What was the source of the configuration data? 
`host_vars/sandboxapicdc.cisco.com`

### Review

In the sample playbook, we added a `private_key` variable, What would you remove (comment out) in the playbook to eliminate the warning?
```
[WARNING]: When doing ACI signatured-based authentication, providing parameter 'password' is not required
```
You can add a '#' prior to the `password` variable and re-run the playbook


### Modify the playbook to use basis authentication rather than signature-based authentication

Edit `files/passwords.yml` to include:
```yaml
---
#
#  Substitute 'cisco123,' with the password for the DevNet Sandbox APIC
#
  apic_password: cisco123,
```
In the `sample.yml` playbook, comment out the `private_key` variable and specify the variable `password` instead. Execute the playbook again to verify

```
$ ansible-playbook -v -i inventory.yml  ./sample.yml -e 'apic_hostname=sandboxapicdc.cisco.com'
```
This is the end of the first lab. You have updated an ACI fabric using Ansible using multiple authentication methods.

### Save your lab environment
If you wish to save your work, verify your GitLab repo is marked *PRIVATE*, then do the following.

```
cd ~workshop
```
edit the .gitignore file and add a '#' in front of the text, then save the file.

```
# cisco_dc_community_of_interest
```
Now issue these Git commands. You will be prompted for your GitLab username and password at the top of the screen.
```
git add .gitignore
git add cisco_dc_community_of_interest/
git status
git add *
git status
git commit -m 'removed git ignore'
git push origin master
```
## Author
joel.king@wwt.com GitHub/GitLab: @joelwking