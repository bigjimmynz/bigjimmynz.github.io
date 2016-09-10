---
layout: post
title: AWS, Ansible, and Assumed Roles.
date: '2016-08-31T19:27:00.000-07:00'
author: James Morgan
tags:
- devops
- Ansible
- automation
- AWS
modified_time: '2016-08-31T19:27:20.929-07:00'
---

One thing that has become common in AWS usage is Assumed Roles. As an IAM user (or even other services), you are able to switch (or assume) roles in the AWS account, allowing you to change to a different set of access privileges. This becomes very useful when you are running multiple accounts. All IAM users can be setup in one account, but have the ability to assume roles in the other account(s). This means user credentials only need to be managed in one place, making overall admin more simple. Much of this is better explained in [Amazon Docs](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) or [Other Blogs](https://blog.stitchdata.com/role-playing-with-aws-c9eaebcc6c98#.uhyeh2iyv)

### Problem in Existing Automation:
Recently my company created some new AWS accounts and decided to move the lab environment to a new one. This meant we would no longer access the lab with direct credentials, but via an assumed role, with the included complexity of requiring MFA. I have a large repository of Ansible scripts, and unfortunately, they were designed to use default credentials setup in my AWS CLI. I have been working on solving this problem, and enabling my scripts to work with or without an assumed role based on variables I configure in files.

### Setting up AWS CLI:
The first step is getting profiles setup in your AWS CLI config. This allows you to have multiple credentials ready for use in different accounts, but also allows profiles for assumed roles. In this example, my default profile is the main account where my user is (know as the jump account), and the lab profile is the account I will be assuming a role into (the destination account). The access key and secret key would be configured in the ~/.aws/credentials file under the profile [default]. The ~/.aws/config file should look like this:

<script class="gist" src="https://gist.github.com/darknessnz/e6ddf3ff23227b84dac54b4b4d9cfc99.js"></script>

The &lt;assumed role&gt; is the name of the role you need to be in the destination account. You or your AWS admin should have already set it up with the required permissions. The mfa_serial line is the value for the MFA device you have configured. It can be found in your user page in the AWS console. Now to test if that works. Grab a shell, and run (Enter an MFA token if you have that setup):

<pre><code class="bash">$ aws ec2 describe-instances --profile lab
</code></pre>

You should get a json blob as a result of any ec2 instances running, or a response of "Reservations":[] if nothing exists there.

### Ansible and using the Security Token Service:
The [AWS Security Token Service (STS)](http://docs.aws.amazon.com/STS/latest/APIReference/Welcome.html)  allows you to request temporary credentials to perform actions through the AWS API. We will need this to grab credentials for the lab account to create resources there.

We are going to create a basic playbook to create an EC2 Security Group that is capable of running directly or via STS credentials for a different account. The first thing we need to do is create a vars/default.yml file in our ansible directory, this will store some basic vars for this test playbook:

<script src="https://gist.github.com/darknessnz/9c09f93688eb36a7bb16a7e539f7565c.js"></script>

The next step is to create a vars/sts.yml. This will store vars needed to enable/disable the STS functionality, and the details needed to authenticate you. The values needed here are the same as what you will have placed in ~/.aws/config in the AWS CLI setup above.

<script src="https://gist.github.com/darknessnz/6a264047bc4f42e055682f1bb094b4d7.js"></script>

Onto the playbook. It runs on the local machine where the playbook executes. And includes the 2 var files we just defined. Here's a look before I describe it:

<script src="https://gist.github.com/darknessnz/e9a598db845e7fb61d0b07874a9026b0.js"></script>

The first task uses sts_assume_role. This takes the default AWS credentials and variables, and requests temp credentials for the specified account/roles. Which it registers in variable assumed_role. The "when" command will only execute this task when the sts variable is true from the vars/sts.yml file.

The set_fact task takes the previously registered var, and retrieves the new credentials from the object. These values are then assigned to local vars for use in subsequent tasks and roles.

The last task is the actual creation of AWS resources. It could be any of the modules, but we will use ec2_group as an example. Here we take the newly set facts and use them to define aws_access_key, aws_secret_key and security_token. If we were to leave those lines out ansible would take our default credentials and attempt the resource creation with them. Likely that will result in authorisation, or missing resource problems. So we override the defaults with the new facts, forcing the task to connect to the API with the correct credentials. In this current form you would run the playbook like this:

<pre><code class="Bash">$ ansible-playbook test.yml -e sts_mfatoken=123456 -vvvv
</code></pre>

Which should result in the last couple of output lines looking like this, with a new security group in AWS:

<pre><code>PLAY RECAP *****************************************************
localhost : ok=3  changed=2  unreachable=0  failed=0
</code></pre>

### To MFA or not MFA:
Now MFA is always a good idea. And should be enabled on your IAM user account. But depending on your AWS accounts and trust configuration, you may or may not need MFA to assume roles. In my case MFA was required, so what you see in the examples above is what is needed to make the playbook work. This required the addition of the sts_mfa and sts_mfatoken vars. And the inclusion of "-e sts_mfatoken=123456" at the playbook runtime (with the number being the token from your mfa device). Otherwise you will end up with an authorisation error. If MFA is not a requirement, here are the modifications:
* remove mfa_serial from ~/.aws/config
* remove sts_mfa from vars/sts.yml
* run the ansible playbook command without "-e sts_mfatoken=123456"

### Without STS?
This playbook was also designed to run directly without needing to assume a role. In vars/sts.yml just change <code class="inline-code">sts: true</code> to <code class="inline-code">sts: false</code>. This will cause any tasks with the <code class="inline-code">when: sts</code> clause to be skipped as they do not pass the conditional check.

But what about the credential lines in the ec2_group task? Well, where I specify the variable value, I have added <code class="inline-code">| default(omit)</code>. This means if those variables are undefined, the module execution with omit those arguments. Forcing the execution to pull the default values (which are my default credentials).

#### Thats it for now......
Well that was probably a rough blog post. It was a bit rushed due to the things I was working on. But hopefully it is helpful. I hadn't been able to find any real world examples of sts_assume_role in use. I will be dropping back to some more "Getting Started with Tools" in the future. Let me know if this helped you, or any improvements that could be made. Till next time, Automate all the things...
