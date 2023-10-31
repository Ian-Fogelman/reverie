---
layout: post
title:  Downloading Remote Files with SCP
categories: [AWS, EC2, SCP]
---
Howdy partner, today we look at how to pull files from your EC2 instance to your local machine using SCP.

Sometimes you just need a remote file on your local workstation, perhaps to avoid additional installation or avoid additional provisioning of your remote instance.

This procedure is useful for:
- Pulling log files from a remote EC2 instance.
- Pulling OS specific files from a remote EC2 instance.
- General data transfer needs.

1. Login to AWS console.

2. Spin up a AWS EC2 instance with the Amazon Linux 2023 AMI.

3. Connect via EC2 instance connect with a SSH session.

4. Curl a file into the the EC2 instance with the command:

   ```curl https://gist.githubusercontent.com/Ian-Fogelman/5b84773e5a1ad7c89fa2203132c36aa8/raw/database_cowboys.txt --output database_cowboys.txt ```

   This creates a text file `database_cowboys.txt` in your working directory.

5. Run the command ``pwd`` on your SSH session to get the current working directory.
   This command returns a directory: ``/home/ec2-user/``.

6. On your local workstation use the SCP client to download the file from your EC2 instance:
    - You will need the key pair used when creating the instance to authenticate to your instance typcially a ``key-pair.pem`` file.
    - In your terminal use the command: ``scp -i ec2key.pem ec2-user@12.12.12.12:/home/ec2-user/database_cowboys.txt .``

        - This command uses scp to initiate a connection, the ``-i`` is the identity file flag which points to ``ec2key.pem``.
        - The ``ec2-user`` is the username of your AWS IAM user (defaults to ec2-user).
        - The ``12:12:12:12`` parameter is the public IPAddress. This can be found on the EC2 dashboard [https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Instances](https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Instances) .
        - The path after ``:`` is the working directory and the file name of the file you want to copy, in this cases ``/home/ec2-user/database_cowboys.txt``.
        - The ``.`` character specifies the local directory as the target for the transfer.
