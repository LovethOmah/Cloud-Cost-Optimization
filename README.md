Automating EBS Snapshot Cleanup for Cost Optimization


Stale or unused EBS snapshots are one of those hidden costs that can slowly inflate your AWS bill. Cleaning them up manually is possible, but it’s repetitive and error-prone. Instead, you can automate this process with EC2, Lambda, and IAM roles.
Here’s a structured, step-by-step demo of how to set this up.



Step 1: Create an EC2 Instance
We’ll begin with an EC2 instance since its volume will provide the snapshot for our demo.
Open the EC2 Console.


Click Launch instance.

Configure the instance (you can use the default Amazon Linux or Ubuntu image).

Review the attached EBS volume under the Storage tab once the instance is running.




Step 2: Take a Snapshot of the Volume
Snapshots are what we’ll eventually clean up with automation.
In the EC2 Console, go to Volumes.

Select the volume that was automatically attached to your new EC2 instance.

Click Actions → Create snapshot.

Give the snapshot a name and description.

Click Create snapshot to finish.




Step 3: Create a Lambda Function
Next, we’ll use Lambda to run the automation code.
Open the Lambda Console.


Click Create function.

Select Author from scratch, give it a name, and choose Python as the runtime.

Once the function is created, go to the Code tab. paste it into the editor.

Paste your cleanUp script  into the editor and Click Deploy.

Run a test to confirm the code executes.

Go to the Configuration tab → General configuration.

Increase the timeout setting to 10 seconds. (this gives enough execution time)

Save changes.




Step 4: Create an IAM Role and Policy
Lambda needs permissions to work with snapshots. We’ll give it exactly what it needs.
Go to Configuration, click on permissions
Click on the role name. 


In the role’s Permissions tab, click Add permissions → Create inline policy.
(The inline policy is specifically created for that role and automatically attached to it)


Select EC2 as the service.

Add the following actions:


ec2:DescribeSnapshots

ec2:DeleteSnapshot

ec2:DescribeVolumes

ec2:DescribeInstances


For Resources, choose All resources (for demo purposes).

Save the policy.





Step 5: Deploy and Test the Setup
Finally, test the complete setup.
Go back to your Lambda function.


Open the Code tab.

Click Test to run the function.

Verify that Lambda is able to:

List existing snapshots.

Delete snapshots that meet your defined criteria.





Wrapping Up
With this setup in place, you’ve automated a small but effective cost optimization task in AWS. Instead of manually cleaning up snapshots, Lambda can do the work for you. In production, you can schedule this function with CloudWatch Events / EventBridge to run on a schedule, and refine the IAM permissions to follow least privilege best practices.
This approach ensures you keep your EBS storage lean and your AWS bill under control.

