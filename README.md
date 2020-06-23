# Configure Cisco Switch for Ansible
 Configure Cisco Switch to Manage from Ansible Script
 
 ![Image of Ansible](https://media-exp1.licdn.com/dms/image/C4D12AQFd7Kko8Kywdg/article-inline_image-shrink_1000_1488/0?e=1598486400&v=beta&t=6g69GwM1etObZjtBckVGj8EKkkMczrcy0atnaBx3KRo)
 if you want to Access Cisco 2960 Switch from Ansible 
 first you have to configure Cisco 2960 Below
 
# Enable telnet on Cisco Switch from Web Portal
 
 * Enable Telnet from cisco portal http://Cisco_SwitchIP
 Configure >> Express setup >> Advance Settings >> telnet Access >> Enable >> Submit
 
 ![Image of Ansible](https://media-exp1.licdn.com/dms/image/C4D12AQE1YSD1q9tpZg/article-inline_image-shrink_1000_1488/0?e=1598486400&v=beta&t=DaJSJ2zW2S0HjAN_408UAjwW2qu9xJjPzs4u-iQRqYA)
 
# Configure Cisco Switch from telnet
```bash
Switch# enable 

Switch# Enter Password

Switch# Configure Terminal

Switch# hostname

Switch# ip domain-name <Charusat.org> #Please enter Domain name of your organization

Switch#crypto key generate rsa
enter 2048

Switch# username <kalpesh> priviledge 15 secret <123456789>
instead of 
<kalpesh> Enter User Name  #Created in switch for ssh access used by Ansible
<123456789> Enter password

Switch# aaa new-model

Switch# aaa authentication login default local

Switch# aaa authorization exec default local
Switch# enable secret <123456789>
<123456789> Enter password
Switch# line vty 0 4

Switch# transport input ssh

Switch# end

Switch# copy running-config startup-config

Login with ssh user <kalpesh> user using putty to switch

ssh <kalpesh>@switchip

Switch# enable 

Switch# Enter Password

Switch# Configure Terminal

Switch# priviledge exec level 15 configure terminal
 
Switch# end
````
# Login to Ansible Control Machine 
login to ansible Control Machine to configure ssh key pair with switch
```bash
Switch# ssh-keygen -t rsa 
````
if already generated then no need to generate again
```bash
Switch# ssh-copy-id kalpesh@SwitchIP
````
# Ansible Control Machine configuration are as below

copy below block to /etc/ansible/hosts
```bash
[ciscodevice]
SwitchIP
[ciscodevice:vars]
ansible_ssh_user=<kalpesh>
ansible_ssh_pass=<123456789>
ansible_network_os=ios
````
copy below block to new yml file like vi ping.yml
```bash
---
- name: Test Ping
  hosts: ciscodevice
  gather_facts: no
  connection: local

  tasks:
  - name: Ping To Cisco Switch
    ios_ping:
     dest: <SwitchIP>
````
