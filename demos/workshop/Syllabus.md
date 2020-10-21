Syllabus
--------


## Section 100

*Introduction* : 30 min. : **Training goals and introductions**

*Session 1* : 60 min. : **Introduction to Ansible for Networking**

In this lecture, we introduce the base components of Ansible and how it can be used for configuration management of network infrastructure, as well as compute, storage and cloud.

*Session 2* : 60 min. : **Automating ACI with Ansible**

In this lecture, we focus on the specific components of Ansible key to managing the Cisco ACI fabric. We examine Signature-based authentication in Ansible modules and best practices for using the ACI Content collections. 

*Session 3* : 15 min. : **Overview of the training environment, topology and goals**

This section highlights the components of the training environment, which includes cloud services GitHub / GitLab, Ansible Galaxy and the Cisco DevNet sandbox and the WWT Advanced Technology Center.

*Session 4* : 30 min. : **Interactive walk-through of setting up and connecting to resources**

In this session the students will verify connectivity to the lab resources from their laptop.

## Section 110

*Session 1* : 45 min. : **Introduction to Git for Network Engineers**
Develop an understanding of the importance of the incorporating revision control for network configuration files. Provide resources to enable the student with a hands-on experience using Git and GitHub.

*Session 2* : 45 min. : **Data Serialization Formats**
Overview of data serialization formats; YAML, JSON, XML and CSV. Examine how they relate to Python variables and Ansible Playbooks.
<!---    https://www.ciscolive.com/c/dam/r/ciscolive/apjc/docs/2018/pdf/DEVNET-3611.pdf -->

## Section 120

*Session 1*  :  45 min. : **Infrastructure as Code**

Programmable Infrastructure (Infrastructure as Code), is a foundational DevOps component enabling The First Way, the fast flow of work through the system. For every network device configuration, there must be one Source of Truth, which is programmatically accessible via an API or file URL. Historically the Source of Truth was the configuration stored on the device itself. Network managers must eliminate the device configuration as the Source of Truth, rather using version controlled configuration artifacts and configuration data sources such as IP address management (IPAM) systems.

Attendees would benefit by listening to the podcast *Gitlab Courseware as Code with Ben Allison* https://softwareengineeringdaily.com/2020/08/17/gitlab-courseware-as-code-with-ben-allison/ hosted by Software Engineering Daily prior to this session.

*Lab 1*  :  45 min. : **Infrastructure as Code**

This lab demonstrates the concepts of Infrastructure as Code by using version controlled (Git) device configuration files.

## Section 200

*Lab 1* : 60 min. : **Manage Tenants, VRFs and fabric policies**

This lab demonstrates how to manage (create, delete or update) Tenants, VRFs and fabric policies using a YAML input file. The student will clone this repository to their Ansible control node using the Visual Studio Code Remote Development Extension Pack, modify the input file, examine and execute the sample playbook provided. The sample playbook demonstrates the Cisco Ansible ACI content collection.

*Lab 2* : 60 min. : **Incorporate using Excel (CSV) files as a Source of Truth to configure the fabric**

This lab demonstrates using Excel files as a source of truth for playbooks managing an ACI fabric. The students will also see the extensibility of Ansible Engine through the use of WWT developed software to optimize and incorporate spreadsheets into the workflow.

*Lab 3* : 60 min. : **Configure and execute your playbook using Ansible Tower**

This lab provides experience configuring Ansible Tower to execute the playbooks from Lab 1 and 2. This lab reinforces the concept of promoting workflow from the network programmability developer's desktop to production deployment by using version control and Ansible Tower.

## Section 210
*Lab 1* : 30 min. : **Cisco DNA Center Platform**
This lab uses a Juypter Notebook exercise to authenticate with DNA Center and show Python code using the Cisco DNA center APIs to retrieve and display wireless health information.

*Lab 2* : 30 min. : **Execute a playbook to manage VLANs using Ansible Tower**
This lab provides experience configuring Ansible Tower to execute a playbook to manage a VLAN on  IOS-XE devices.

## Section 300

*Session 1* : 45 min. : **GitLab CI/CD Pipeline** 

This lecture introduces the concept of CI/CD (continuous integration, continuous delivery, and continuous deployment) and demonstrates implementing a GitLab CI/CD Pipeline to implement Ansible Lint for Static Code Analysis of playbooks used in the labs and lectures. Also explained is creating and publishing an image to DockerHub to implement the pipeline.

*Lab 1*  : 45 min. : **GitLab CI/CD Pipeline**

This lab provides the students with hands-on experience with creating their own Docker image, publishing the image, and using the image to create a GitLab CI/CD Pipeline in their workshop repository.