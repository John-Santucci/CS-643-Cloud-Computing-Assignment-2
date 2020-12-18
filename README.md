# CS-643-Cloud-Computing-Assignment-2 

# Create EC2 instances:
Create 4 EC2 instances with Amazon Linux AMI.
For the open ports, select 22 (SSH), 80 (HTTP), and 443 (HTTPS).
For the first instance, create a new permission file and save it to disk.
For the second, third and fourthinstance, use the existing pem file that was just created. 
Create a new security group and attach existing polices for access to S3, SQS, and Wine_Test. Attach these security groups to the 4 EC2 instances.

# Security Credentials setup
The security credentials need to be saved on the machine that is running the projects (the EC2 instances in this case).
The AWS SDK reads from these files to authenticate requests.
For unix systems, create a ".aws/" directory at the home directory (~/).
Create a config file inside the directory to set the default region
- [default]
- region=us-east-1
Create a credentials file inside the directory to place the aws access key id and aws secret access key.
- [default]
- aws_access_key_id=<access-key-id>
- aws_secret_access_key=<secret-key>

# Create the SQS queue
Using the amazon console, create a FIFO SQS with all other settings left as default except for checking the box on "Content-based deduplication".
# Input SQS queue name
Inside Wine_List/SQSReader.java, replace queueUrl with the URL of your FIFO queue.
Inside  Wine_Test/SQSWriter.java, replace queueName with the name of your FIFO queue.

# Build the Java projects into a jar including the dependencies
IntelliJ IDE was used to build the projects into a jar file including the dependencies using the .pom file.

# Move Java projects to EC2 VMs:
## Linux 
scp -i pemFile.pem -r Wine_List/wine_list.jar ec2-user@ec2-54-227-215-164.compute-1.amazonaws.com:~/
scp -i pemFile.pem -r Wine_Test/wine_test.jar ec2-user@ec2-3-88-188-69.compute-1.amazonaws.com:~/
## Windows
Transfer using FileZilla or similar method. 

# Upgrade EC2 instances to run Java 1.8
sudo yum install java-1.8.0-openjdk.x86_64
sudo yum erase java-1.7.0-openjdk

# Connect to instances:
ssh -i pemFile1.pem ec2-user@ec2-54-227-215-164.compute-1.amazonaws.com
ssh -i pemFile2.pem ec2-user@ec2-3-88-188-69.compute-1.amazonaws.com

# Run Java applications
## In EC2A
java -jar Wine_List.jar
## In EC2B
java -jar Wine_Test.jar
## In EC2C
java -jar Wine_Test.jar
## In EC2D
java -jar Wine_Test.jar


# Results
In EC2B,EC2C, and EC2D a file called "results.txt" will be created and includes the results of running the application.
