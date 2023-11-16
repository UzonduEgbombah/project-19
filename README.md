# Automate Infrastructure With IaC using Terraform. Part 4 - Terraform Cloud

#### What Terraform Cloud is and why use it
By now, you should be pretty comfortable writing Terraform code to provision Cloud infrastructure using Configuration Language (HCL). Terraform is an open-source system, that you installed and ran a Virtual Machine (VM) that you had to create, maintain and keep up to date. In Cloud world it is quite common to provide a managed version of an open-source software. Managed means that you do not have to install, configure and maintain it yourself - you just create an account and use it "as A Service".

Terraform Cloud is a managed service that provides you with Terraform CLI to provision infrastructure, either on demand or in response to various events.
By default, Terraform CLI performs operation on the server whene it is invoked, it is perfectly fine if you have a dedicated role who can launch it, but if you have a team who works with Terraform - you need a consistent remote environment with remote workflow and shared state to run Terraform commands.
Terraform Cloud executes Terraform commands on disposable virtual machines, this remote execution is also called remote operations.

#### Migrate your .tf codes to Terraform Cloud
Let us explore how we can migrate our codes to Terraform Cloud and manage our AWS infrastructure from there:

- Create a Terraform Cloud account
- Create an organization
- Configure a workspace
Before we begin to configure our workspace - watch this part of the video to better understand the difference between version control workflow, CLI-driven workflow and API-driven workflow and other configurations that we are going to implement.

We will use version control workflow as the most common and recommended way to run Terraform commands triggered from our git repository.
Create a new repository in your GitHub and call it terraform-cloud, push your Terraform codes developed in the previous projects to the repository.
Choose version control workflow and you will be promped to connect your GitHub account to your workspace - follow the prompt and add your newly created repository to the workspace.

![](https://github.com/UzonduEgbombah/project-19/assets/137091610/9ab56367-b21c-4532-8a86-befff2925a2f)

on to "Configure settings", provide a description for your workspace and leave all the rest settings default, click "Create workspace".

Configure variables

Terraform Cloud supports two types of variables: environment variables and Terraform variables. Either type can be marked as sensitive, which prevents them from being displayed in the Terraform Cloud web UI and makes them write-only.
Set two environment variables: AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY, set the values that you used in Project 16. These credentials will be used to privision your AWS infrastructure by Terraform Cloud.

![](https://github.com/UzonduEgbombah/project-19/assets/137091610/7476788f-c5ed-4888-8994-3a95399400ac)

After you have set these 2 environment variables - yout Terraform Cloud is all set to apply the codes from GitHub and create all necessary AWS resources.

Now it is time to run our Terrafrom scripts, but in our previous project which was project 18, we talked about using Packer to build our images, and Ansible to configure the infrastructure, so for that we are going to make few changes to  our existing respository from Project 18.

The files that would be Addedd is;

AMI: for building packer images

Ansible: for Ansible scripts to configure the infrastucture

Before you proceed ensure you have the following tools installed on your local machine;

#### AMI

![](https://github.com/UzonduEgbombah/project-19/assets/137091610/8d0204ca-dc15-4e05-beb3-1d2163e09d32)


![](https://github.com/UzonduEgbombah/project-19/assets/137091610/1bdff2ee-21d9-4817-9e4c-b857efeb99da)


![](https://github.com/UzonduEgbombah/project-19/assets/137091610/317ee5e3-82be-4746-9b1d-01699e37e404)


![](https://github.com/UzonduEgbombah/project-19/assets/137091610/17cc08f6-0c13-43db-b7a4-678f5e20b19d)


![](https://github.com/UzonduEgbombah/project-19/assets/137091610/63cb2d7a-12c9-4dbb-b856-6068de865608)


#### packer

![](https://github.com/UzonduEgbombah/project-19/assets/137091610/49cd5216-591b-4b54-857d-cfb46c99ffa3)


#### Ansible

![](https://github.com/UzonduEgbombah/project-19/assets/137091610/9db07df9-9c36-4306-a80f-ddf1610a497e)

#### Refactor your enviroment to meet the new changes 

![](https://github.com/UzonduEgbombah/project-19/assets/137091610/336002b1-6bd6-4b50-9b13-26b3b17afcf8)

 #### Run terraform plan and terraform apply from web console

Switch to "Runs" tab and click on "Queue plan manualy" button. If planning has been successfull, you can proceed and confirm Apply - press "Confirm and apply", provide a comment and "Confirm plan"
Check the logs and verify that everything has run correctly. Note that Terraform Cloud has generated a unique state version that you can open and see the codes applied and the changes made since the last run.

![](https://github.com/UzonduEgbombah/project-19/assets/137091610/21b905f7-5643-4597-b3fa-ea5fdf8cb649)


![](https://github.com/UzonduEgbombah/project-19/assets/137091610/3a175fa4-b890-4a0b-afcc-b901cc8b305e)


![](https://github.com/UzonduEgbombah/project-19/assets/137091610/27ea7c83-5d26-4317-97ce-0de9ce0712e3)


![](https://github.com/UzonduEgbombah/project-19/assets/137091610/acdade38-8a48-4cd1-99e4-01b079fc9b19)


![](https://github.com/UzonduEgbombah/project-19/assets/137091610/05d1c951-e986-48ff-80a2-cd846bbdd137)


![](https://github.com/UzonduEgbombah/project-19/assets/137091610/d0f049c7-3153-4edf-bb1f-7272e2c05229)


![](https://github.com/UzonduEgbombah/project-19/assets/137091610/f464aa51-19e1-4235-9183-8bc09b4db218)


![](https://github.com/UzonduEgbombah/project-19/assets/137091610/d7d249ea-10b0-46f7-9765-eb537688f41e)


![](https://github.com/UzonduEgbombah/project-19/assets/137091610/f1b59d13-ec06-4632-a601-2735b97578b6)


![](https://github.com/UzonduEgbombah/project-19/assets/137091610/78147b98-66ba-4d50-a531-a37c1bf5bb79)


![](https://github.com/UzonduEgbombah/project-19/assets/137091610/2a6efd51-16af-479b-8148-c3993162e0a6)


![](https://github.com/UzonduEgbombah/project-19/assets/137091610/fe786edb-2cb4-407c-8708-c60ac32dee47)


#### Test automated terraform plan


By now, you have tried to launch plan and apply manually from Terraform Cloud web console. But since we have an integration with GitHub, the process can be triggered automatically. Try to change something in any of .tf files and look at "Runs" tab again - plan must be launched automatically, but to apply you still need to approve manually. Since provisioning of new Cloud resources might incur significant costs. Even though you can configure "Auto apply", it is always a good idea to verify your plan results before pushing it to apply to avoid any misconfigurations that can cause 'bill shock'.
