---
layout: default
---




# The Cloud

For the purpose of extending and scaling the project a cloud service was very much essential. Through the cloud platform the following were achieved
- Leverage Cloud platform services
- Faster Access using virtual environment
- scalibility of existing resources 

which also had the following advantages 
- Quick Colloboration 
- Lessen the data vuinerability, than on the local 
- Flexibility 

To achieve the purpose we experimemted on using different cloud services like AWS, Azure and Google cloud. We finally settled on using  Google cloud platform as per our requirement, we chose GCP over AWS because of cost factors and also as we had credits for GCP.

Cloud  Requirement Analysis for the Dry Run
The Analysis of cloud and its servies was analysed based on the following features:- 
1 Cloud SQL Instance for Database
 
<br> SQL Standard 
<br> M/C type : db-n1-standard-4
<br> RAM(GB) : 15
<br> Max Storage Capacity: 10,230 GB
<br> Max Connections: 4000

To accomplish the task the following services were to be setup in the cloud environment

### IAM Role

In this service the responsibilities were assigned to the users based on the roles and the the purpose the user is accessing the cloud service. There are three types of roles which are a collection of permissions we  can impose on a user. The following are the roles  
- Primitive roles: Owner, Editor, Viewer
- Predifined roles: Pub/ Sub Publisher
- Custom roles


### Cloud Storage

In the Google cloud platform, buckets are created to hold objects. To meet the requirement of this project, a bucket named Capsule-networks-project was created to store all the images which were created using Maya. The bucket has folders which the category and subcategory in the way they are stored in the drive

[Drive Link](https://drive.google.com/drive/u/1/folders/1c7wjh__WL8cVYCPE3ebdM8oSq1riKts6)

![Octocat](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/image2.png)

![Octocat](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/image5.png)

### Google SQL

For integrating the sql in the cloud a sql servivce was established which is also used for the website.
 
We will require Cloud type as : My SQL and Second Generation.

For creating an instance the following code is used:
<br> Under GCP> Click on SQL
<br> Create Instance> Choose first generation
<br> Choose same region> Instance ID> Click on create
<br> Go to Console or MAMP
<br> Open Webstart page > Click on the DB > Export> Custom > SQL > Go> Uncheck enclose table & column names> Go
<br> Copy the code & save it as DB.sql file
<br> Click to the DB Instance> Import (SQL or CSV)
<br> In the meanwhile save the DB(exported one) into the storage> Select the DB
<br> And the MySQL DB gets imported to SQL instance

![Branching](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/image8.png)

![Branching](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/image10.png)

![Branching](https://raw.githubusercontent.com/Preethamalladu/DMDD-Presentation/master/image11.png)

### Google Compute Engine

The compute engine is used for running the neural networks which will reduce the run time. The requirement of the engine is as follows
 
<br>M/C type: n1-standard-8
<br>Virtual CPU: 8
<br>Memory: 30 GB

![Branching](https://raw.githubusercontent.com/Sindhurakolli/DMDD_portfolio/master/VM_instance.JPG)

For setting up the engine We didn’t had sufficient credits to spin up the instance with NVIDIA GPU’s and also it is not efficient in terms of the computational time.

### VPC Network

Firewall rules and External IP address were set up for using the services externally like Jupiter notebook 

## Contributions in Cloud
Nikunj Doshi 


Please click on below link to go to Website part and Overall references and Citations and Licence part:

* * *

[The Website Part and References and Licence](./website.html)

* * *


[Back](./)

