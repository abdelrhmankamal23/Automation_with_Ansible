## Task_3:  Play an Ansible Role from your VM (workstation) to an EC2 (Apache Web Server)
This is the improvment Task_2 that we will deploy the play on the ec2  
Here is an overview diagram of the task  

<img src="https://i.ibb.co/hFKNtFsv/Screenshot-8-5-2025-13389-lucid-app.jpg" alt="description" width="650" height="400">


### Ensure security allowance
Launch an ec2 from the AWS management console then in its security group:  
- Add the inbound rule of ssh from the current vm instance that you will run or upload the playbook from.
- Add the custom TCP port 88.  

![task3-img](https://i.ibb.co/RT9JHHgB/Screenshot-8-5-2025-132826-us-east-2-console-aws-amazon-com.jpg)
---

### Modify the inventory
Add the ec2 Public IP of the ec2 instance in the inventory along with the key-pair assigned to it as the following  
`3.149.252.117 ansible_user=ec2-user ansible_ssh_private_key_file=~/.ssh/mainkp.pem`  

Note that after uploading your key to the current VM you have to change the permission to 400 as when you use an EC2 key-pair (typically a .pem file) to connect via SSH, the permissions must be restricted so that only the file's owner can read it — otherwise, SSH will refuse to use it for security reasons. Also make sure that it's placed in the ~/.ssh directory  

### Run the playbook and enjoy the automation 
Copy the public IP of the instance and paste it in your browser with :88 and the end to show your website. 

<img src="https://i.ibb.co/jvq1p2gn/web-test.png" alt="description" width="90%">