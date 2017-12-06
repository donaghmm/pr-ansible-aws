## Standalone Tomcat Deployment to ubuntu OS in AWS

- Requires Ansible 1.5 or newer
- AWSCLI
- Expects Ubuntu hosts
- Install Openjdk 1.8
- Install Tomcat 8

have your aws access keys ready in ~/.boto or ~/.aws/credentials file or even in the ENV variables.

These playbooks deploy a very basic implementation of Tomcat Application Server,
version 8. Edit the group_vars/tomcat-servers file to set any Tomcat configuration parameters you need.

This playbook contain 9 sections 

1) Check your current IP and keep the value in my_ip var
2) Create the simple security group and whitelist your IP for accessing instances for port 80 and 22
3) Create ELB which is listening on port 80
4) Create Launch Configuration (when we add the instances to ASG we need the launch configuration to create our instances with same configuration) so we need this later
5) Create EC2 instances (currently for my AWS the restrictions are max 2) We can change that in group-vars/pramerica
6) Gather IP's of the instances and store them for later. Also make the hosts to know each other (establishing trust/known hosts)
7) Install and configure tomcat and apache, use apache's ProxyPass to forward port 80 traffic to internal tomcat AJP
8) Adding the instances to ELB
9) Configuring the ASG and Configuring Alarms as per Scaling Properties

Then run the playbook, like this:

ansible-playbook -i inventory/ec2.py pr.yml -e @group_vars/pramerica

When the playbook run completes, you should be able to see the Tomcat
Application Server running on the port 80, on the target machines.
