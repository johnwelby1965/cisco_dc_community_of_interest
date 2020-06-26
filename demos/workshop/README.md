Ansible Automation : ACI
------------------------

## Student Resources
To make these demonstrations a valuable training experience you will need some software on the personal computer you use for your day-to-day activities. 

### User Accounts
There are several user accounts required to participate in the training. You may already have one or more of these accounts.

#### WWT Digital Platform
The ATC is a collaborative ecosystem to design, build, educate, demonstrate and deploy innovative technology products and integrated architectural solutions for World Wide Technology customers.

Create an account on https://www.wwt.com/ using your business email address and complete your profile settings at https://www.wwt.com/my-account#MyProfile.

##### Launch the Lab
Search for `Ansible Student VM` and select the lab. It is at URL [ttps://www.wwt.com/lab/ansible-student-vm](https://www.wwt.com/lab/ansible-student-vm).

Launch the lab. Go to https://www.wwt.com/my-wwt/labs - Or select the labs page from the pop-up windows `Your virtual environment is ready. You can access it on the My Labs page.` Click `Access Lab`, then select `Open in ATC Lab Gateway`.  You may be prompted to authenticate with your email address and 14 character password.

Once connected, your terminal session is connected to the EC2 instance. 

Make a note of the IP address under `===Student VM Public IP===`. This IP address is the public IP address of your AWS EC2 instance for this lab.

You will need this IP address to configure the VS Code remote development environment under section `Remote Explorer`.

> **Note**: You can test connectivity from your laptop to this instance using a SSH client and private key file (PEM) for the training class. `ssh -i student-key_Truist_ACI.pem ubuntu@<Student VM Public IP>`.

<!--- https://github.com/tylerhatton/ansible-student-lab-tf-template -->  

#### Version Control 
Both GitLab (GitLab.com) and GitHub (GitHub.com) provide Git Version Control repository management services over the Internet. If you do not already have an account on both services, sign up for both, ideally using the same handle (username) for your account on each service. For this training, the free account tier is all you need.

#### Cisco DevNet
Cisco DevNet is a developer program to help IT professionals develop integrations with Cisco products. It also hosts learning tracts and sandbox environments. Join DevNet for free at https://developer.cisco.com/

### Install Software
Hands-on experience (construction) is a key to learning a new topic. Because our 'hands are on keyboards' most of the day, the students of this training will benefit the most if they learn automation with their own version control accounts and development environment on their laptop. 

In addition to a computer running the macOS, Linux, and Windows 10 operating systems, the following software is required for this training environment.

#### Visual Studio Code
VS Code is a free code editor, which runs on the aforementioned operating systems. Download and install it at https://code.visualstudio.com/download. 

While there are a number of suitable editors available for free or for a nominal license fee, this training makes use of the VS Code [Remote Development using SSH](https://code.visualstudio.com/docs/remote/ssh). The Visual Studio Code Remote - SSH extension allows you to open a remote folder on any remote machine, (virtual machine, or container) and terminal window with a running SSH server. 

Install the [Visual Studio Code Remote Development Extension Pack](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack).

As part of the training environment, a remote SSH host will be set up which includes a version of Ansible Engine and other necessary software.

There are a number of on-line tutorials to assist with setting up your laptop. See [Remote Development tutorials](https://code.visualstudio.com/docs/remote/remote-tutorials).

##### SSH client
Your laptop will need a supported SSH client. Refer to [Installing a supported SSH client]https://code.visualstudio.com/docs/remote/troubleshooting#_installing-a-supported-ssh-client).

###### macOS
For macOS, you should not need to install an additional ssh client. 

###### Windows
For Windows, installing Git for Windows is recommended. Refer to https://git-scm.com/download/win. 

Once installed, determine the absolute path of the `ssh` executable. It may be at location `c:\Program Files\Git\usr\bin\ssh.exe`.

Launch Visual Studio Code, in the lower left of the window, right click the gear icon and select `Settings`.  

In the search settings dialog box, type `remote.ssh.path`. In the resulting dialog box, enter the absolute path of your ssh client. For example ` C:/'Program Files'/Git/usr/bin/ssh.exe`. Note the back slash has been substituted by a forward slash and single quotes were added to bound the directory with embedded space. 

If you select the 'gear' icon next to the `remote.ssh.path` select `Copy Settings as JSON`. The copy buffer will contain:

```
"remote.SSH.path": " C:/\"Program Files\"/Git/usr/bin/ssh.exe"
```

Close the Settings window.

##### Remote Explorer
Prior to class you will be provided access to a remote Linux instance. To configure, select the 'Remote Explorer' icon on the left side of the window. Select the 'gear' icon to create a  'custom configuration file'. Provide an absolute path to the configuration file. For example: `C:\Users\kingjoe\.ssh\config`.

When selecting the '+' icon next to the SSH TARGETS, you will be prompted for an 'SSH Connection Command', enter `ssh training@example.net` an press enter to confirm your input.

You will be provided a SSH private key file (PEM) for the training class, along with the values to specify for the user, host and hostname.

You will need to **Add** an SSH entry for your EC2 host in the SSH Configuration File.

Here are the values you will need to create your entry: 

* `Host` == This will be the IP Address you captured above from the ATC Lab Instance
* `User` == The username will be **ubuntu**
* `Hostname` == This will also be the IP Address
* `IdentityFile` == Save the PEM private key to a file named `student-key_Truist_ACI.pem` and reference it

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

After saving the configuration file, the new host will appear in the SSH TARGETS window. Verify the configuration by '+ window' icon to connect to the host in a new window.

Select **Continue** when prompted with **Are you sure you want to continue?** This is adding the certificate to the `known_hosts` file.   


## Lab 1 : Authentication : Cisco APIC Sandbox

Logon your GitLab account, create a new project, (https://gitlab.com/projects/new) and name it `workshop`.  The setting can be  'public' or 'private'.  Select the `Initialize with a README`. Select `Create Project`.

**Note:** Before uploading (pushing) to this repo, make the repo PRIVATE under `Settings -> General -> Visibility, project features, permissions` as there will be clear text credentials present in the exercise.

### Clone

From gitlab.com select `Clone` and copy the URL for `Clone with HTTPS`.

Your copy buffer should have something similar to the following, substituted with your username.
```
https://gitlab.com/joelwking/workshop.git
```

#### Set global config values and clone

Open a  terminal window in VS Code.  This will be the terminal of your EC2 Ubuntu workstation.

Use your GitLab username and email address, update the configuration on your remote instance
```shell
$ git config --global user.name "joelwking"
$ git config --global user.email joel.king@wwt.com
$ git config --list
```
Now clone the repository. If it is marked `private`, at the top of the IDE window, you should be prompted for your GitLab username and password.
```shell 
$ cd ~
$ git clone https://gitlab.com/joelwking/workshop.git
```
> **Note:** So your don't forget make the repo PRIVATE under `Settings -> General -> Visibility, project features, permissions`.  Scroll down and select **SAVE**.

### Download the workshop files
We will clone this repository so we have some sample files and playbooks to work from. Do the following:

```shell
$ cd workshop
$ git clone https://gitlab.com/joelwking/cisco_dc_community_of_interest.git
```
As a precaution from accidentally uploading any sensitive data, we will ignore the lab exercuse files at this point.
```shell
$ echo "cisco_dc_community_of_interest" >>.gitignore
$ git status
```

### Install the Cisco ACI collection
Install the collection, by default it will install in your `~/.ansible` folder.
```shell
$ cd ~
$ ansible-galaxy collection install cisco.aci
```
From the terminal window, enter the following directory path.

```shell
$ cd ~/workshop/cisco_dc_community_of_interest/demos/engine/playbooks/
```

### Modify the sample playbook and files
Go to the main IDE window and select the `Remote Explorer` icon on the left, and open the `workshop` folder. 

Navigate down to `~/workshop/cisco_dc_community_of_interest/demos/engine/playbooks`

Open the `sample.yml` playbook. Below the `password` variable in the `aci_login anchor` add the following line.

```yaml
private_key: "{{ lookup('file', '{{ playbook_dir }}/files/{{ apic_username }}.key') }}"
```
Save the file.

#### Create the key file
Under the `files` directory, create a new file called `admin.key` and paste in the key provided. You will be given this key via email or another out-of-band method prior to class.

#### Null the existing credentials
From your IDE window, look at `files/passwords.yml`, this is a vaulted file. We aren't going to use this file now, rather I want to emphasis the fact credentials can be encrypted and stored in a repository.

>**Note**: You can encrypt and decrypt files with `ansible-vault`. For example `$ ansible-vault encrypt passwords.yml` and enter a passphrase when prompted. You can then decrypt the file by entering `$ ansible-vault encrypt passwords.yml` and the passphrase. When running playbooks `--ask-vault-pass ` can be specified to be prompted for the passphrase.

From your terminal window, make a copy of the file and touch the file so the playbook finds an empty file.
```shell
$ mv files/passwords.yml files/passwords.yml-
$ touch files/passwords.yml
```

### Run the playbook
From the terminal window, in the `~/workshop/cisco_dc_community_of_interest/demos/engine/playbooks/` directory, execute the playbook.

```shell
$ ansible-playbook -v -i inventory.yml  ./sample.yml -e 'apic_hostname=sandboxapicdc.cisco.com'
```

Congratulation, you have run your first playbook! If all goes well, you have also automated the configuration of the Cisco DevNet always on ACI Sandbox.

In this example, the configuration data came from the `host_vars` directory. Using the IDE editor, review what you see in that directory.

What was the source of the configuration data?  Answer: `host_vars/sandboxapicdc.cisco.com`

### Review
In the sample playbook, we added a `private_key` variable, What would you remove (comment out) in the playbook to eliminate the warning?
```shell
[WARNING]: When doing ACI signatured-based authentication, providing parameter 'password' is not required
```
You can add a '#' prior to the `password` variable to comment out that line and re-run the playbook


### Modify the playbook to use basis authentication rather than signature-based authentication

Edit `files/passwords.yml` to include:
```yaml
---
#
#  Substitute '__apicpassword__' with the password for the DevNet Sandbox APIC
#
  apic_password: __apicpassword__
```
In the `sample.yml` playbook, comment out the `private_key` variable and specify the variable `password` instead. Execute the playbook again to verify

```shell
$ ansible-playbook -v -i inventory.yml  ./sample.yml -e 'apic_hostname=sandboxapicdc.cisco.com'
```
This is the end of the first lab. You have updated an ACI fabric using Ansible using multiple authentication methods.

### Save your lab environment
If you wish to save your work, verify your GitLab repository is marked *PRIVATE*, then do the following.

```shell
$ cd ~/workshop
```
Edit the .gitignore file and add a '#' in front of the text, then save the file.

```
# cisco_dc_community_of_interest
```
Now issue these Git commands. You will be prompted for your GitLab username and password at the top of the screen.
```shell
$ git add .gitignore
$ git add cisco_dc_community_of_interest/
$ git status
$ git commit -m 'removed git ignore'
$ git push origin master
```

## Lab 1A : Authentication : ATC Demo Fabric
This lab is an optional extension to Lab 1. We have provided access to a second APIC running in the WWT Advanced Technology Center. This APIC is exposed to the Internet by a combination of NGINX and NGROK. The NGROK connection must be started by the instructor, using a temporary NGROK service. You will be provided the hostname of the public URL and the keyfile to access the demo account.

### Update the `inventory.yml` file

Update the `files/inventory.yml` with the temporary host of the NGROK server. For example, if the ngrok server is `bb130ece7dbb.ngrok.io`, your inventory file should look as follows.

```yaml
    NGROK:
      hosts:
        bb130ece7dbb.ngrok.io:                     # Change to the NGROK https hostname
          apic_hostname: bb130ece7dbb.ngrok.io      # Change to the NGROK https hostname
          apic_username: truist
          apic_use_proxy: no
          apic_validate_certs: no
          apic_password: foo!bar                    # will provide this value as extra-var
```
#### Create the key file
Under the `files`, create a new file called 'truist.key' and paste in the key provided. You will be given this key via email or another out-of-band method.

#### Null the existing credentials
From your IDE window, look at `files/passwords.yml`, this is a vaulted file. We aren't going to use this file now, rather I want to emphasis the fact credentials can be encrypted and stored in a repository.

From your terminal window, make a copy of the file and touch the file so the playbook can find an empty file.
```shell
$ mv files/passwords.yml files/passwords.yml-
$ touch files/passwords.yml
```
### Create a Source of Truth
In Lab 1 the configuration input data is stored in the `host_vars` directory. The `group_vars` and `host_vars` directories may contain group variables and host variables, respectively. The group and host names from the inventory file are used to by Ansible to automatically read input data from these directories as configuration data.

Go to the IDE window and open `host_vars/sandboxapicdc.cisco.com`. Save the file as `files/ngrok.yml`.

Edit the playbook `sample.yml`.  Add an element to the `vars_files` to reference the file you created.

```yaml
vars_files:
    - '{{ playbook_dir }}/files/passwords.yml'
    - '{{ playbook_dir }}/files/ngrok.yml'
```
Remember to save your updates.

### Execute the playbook
From the playbook directory, execute the playbook, substituting the appropriate value for the temporary NGROK host.

```shell
$ ansible-playbook ./sample.yml -v -i inventory.yml -e 'apic_hostname=bb130ece7dbb.ngrok.io'
```

### Push your changes
Push your changes to the GitLab remote. When prompted for username and password, use your GitLab credentials.

```shell
$ cd ~/workshop
$ git add cisco_dc_community_of_interest/
$ git commit -m 'Lab 1A'
$ git push origin master
```
Logon your GitLab account, verify the repository is private, (there should be a lock icon above the Project ID:) and navigate in the GUI to review the changes you have made in this section.

## Lab 2 : Custom modules

Users can extend Ansible's functionality by writing their own modules (typically in Python). In this lab, we will download and use a custom module to read configuration data from a Excel / CSV file.

The custom module for this lab is published on Cisco DevNet Code Exchange. Refer to https://developer.cisco.com/codeexchange/github/repo/joelwking/csv-source-of-truth. The Ansible documentation for adding local modules is at: https://docs.ansible.com/ansible/latest/dev_guide/developing_locally.html

Ansible automatically loads all executable files found in certain directories as modules, one option is the playbook `library` directory. Download the custom module to that directory.

```shell
$ cd ~/workshop/cisco_dc_community_of_interest/demos/engine/playbooks
$ wget https://raw.githubusercontent.com/joelwking/csv-source-of-truth/master/library/csv_to_facts.py -O ./library/csv_to_facts.py
```
To verify the module has been successfully downloaded, review the module documentation.

```shell
$ ansible-doc -M ./library -t module csv_to_facts
```
### Examine the playbook
Examine the playbook `sample_csv.yml`.  This playbook includes a task to invoke the `csv_to_facts` module we downloaded. This module will read the specified input CSV file and return Ansible facts to the playbook. These facts will be the Source of Truth for configuring the APIC.

```yaml
    - name: Get facts from CSV file
      csv_to_facts:
        src: '{{ ifile }}'
        table: spreadsheet
```
The module returns a list variable named `spreadsheet`. Each element in the list is a dictionary using the column header as the key and the column value as the value. The subsequent tasks in the playbook will use this data structure as input to the Ansible ACI module arguments.

### Examine the Source of Truth
From your IDE window, open  `/files/sample_csv_file.csv` which is the input file for the playbook. Note the column headers and the values of each row. 

#### Execute the playbook
From the terminal window, execute the playbook. The playbook uses the basic authentication (userid and password), specify the appropriate APIC password on the command line.

Three extra variables are passed to the playbook, `ifile` is the Source of Truth, and the APIC hostname and credential.

```shell
ansible-playbook ./sample_csv.yml -i inventory.yml -e  'ifile=./files/sample_csv_file.csv apic_hostname=sandboxapicdc.cisco.com apic_password=__apicpassword__'
```

## Lab 3 : Ansible Tower
This lab demonstrates how the playbook and Source of Truth from Lab 2 can be incorporated into Ansible Tower. The collections and custom modules have been installed for you.

We will logon an Ansible Tower instance running in AWS. The host name and credentials will be provided out-of-band.

```
https://ec2-54-81-23-3.compute-1.amazonaws.com/
userid admin
password __changeme123,__
```

### Inventory
We create a host in the demo inventory for the target APIC. Use the `inventory.yml` example from Lab 2. For example, create a host named `sandboxapicdc.cisco.com` with these variables:

```yaml
apic_hostname: sandboxapicdc.cisco.com
apic_username: admin
apic_use_proxy: no
apic_validate_certs: no
```

### Project
The project definition references the GitLab repo. Create a project named `Lab3` with an SCM type of `Git` and the SCM URL as `https://gitlab.com/joelwking/cisco_dc_community_of_interest.git`. Save the project and verify the SCM update is complete.

### Template
Create a template named `Lab3`. From the playbook box select `demos\engine\playbooks\sample_csv.yml`. Select the inventory `Demo Inventory` and job type `run`. In the extra variables dialog box add these variables:

```yaml
ifile: ./files/sample_csv_file.csv 
apic_hostname: sandboxapicdc.cisco.com 
apic_password: __apicpassword__
```
Select the dialog box 'prompt on launch'.

> **Note:** substitute the correct hostname and password.

### Execute the Template
To launch the job template, navigate to the Templates resource and click the Rocket ICON next to the Lab3 line.

### Review the Output
Review the output of the job by selecting `VIEWS-Jobs` from the menu.

## Summary
In these lab exercises, we have created our own GitLab repository, cloned an additional repository which, included sample data and playbooks. We have examined and modified the playbooks and configuration data. We have examined the configuration of both basic authentication and signature-based authentication. We downloaded a custom module that reads a CSV file as a Source of Truth to configure the APIC. Finally, we demonstrate using Ansible Tower to execute a sample playbook using the CSV file as input.

## Author
joel.king@wwt.com GitHub/GitLab: @joelwking