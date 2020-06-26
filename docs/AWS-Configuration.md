# RDS Database Creation

1. Go to AWS Management Console and use Find Services to search for RDS
1. Click Create database button
1. Select PostgreSQL
1. Check 'only enable options eligible for RDS Free Usage Tier' and click Next button
1. Scroll down to Settings Form
1. Set DB Instance identifier to multi-docker-postgres
1. Set Master Username to postgres
1. Set Master Password to postgres and confirm
1. Click Next button
1. Make sure VPC is set to Default VPC
1. Scroll down to Database Options
1. Set Database Name to fibvalues
1. Scroll down and click Create Database button

# ElastiCache Redis Creation

1. Go to AWS Management Console and use Find Services to search for ElastiCache
1. Click Redis in sidebar
1. Click the Create button
1. Make sure Redis is set as Cluster Engine
1. In Redis Settings form, set Name to multi-docker-redis
1. Change Node type to 'cache.t2.micro'
1. Change Number of replicas to 0
1. Scroll down to Advanced Redis Settings
1. Subnet Group should say “Create New"
1. Set Name to redis-group
1. VPC should be set to default VPC
1. Tick all subnet’s boxes
1. Scroll down and click Create button

# Creating a Custom Security Group

1. Go to AWS Management Console and use Find Services to search for VPC
1. Click Security Groups in sidebar
1. Click Create Security Group button
1. Set Security group name to multi-docker
1. Set Description to multi-docker
1. Set VPC to default VPC
1. Click Create Button
1. Click Close
1. Manually tick the empty field in the Name column of the new security group and type multi-docker, then click the checkmark icon.
1. Scroll down and click Inbound Rules
1. Click Edit Rules button
1. Click Add Rule
1. Set Port Range to 5432-6379
1. Click in box next to Custom and start typing 'sg' into the box. Select the Security Group you just created, it should look similar to 'sg-…. | multi-docker’
1. Click Save Rules button
1. Click Close

# Applying Security Groups to ElastiCache

1. Go to AWS Management Console and use Find Services to search for ElastiCache
1. Click Redis in Sidebar
1. Check box next to Redis cluster and click Modify
1. Change VPC Security group to the multi-docker group and click Save
1. Click Modify

# Applying Security Groups to RDS

1. Go to AWS Management Console and use Find Services to search for RDS
1. Click Databases in Sidebar and check box next to your instance
1. Click Modify button
1. Scroll down to Network and Security change Security group to multi-docker
1. Scroll down and click Continue button
1. Click Modify DB instance button
1. Applying Security Groups to Elastic Beanstalk
1. Go to AWS Management Console and use Find Services to search for Elastic Beanstalk
1. Click the multi-docker application tile
1. Click Configuration link in Sidebar
1. Click Modify in Instances card
1. Scroll down to EC2 Security Groups and tick box next to multi-docker
1. Click Apply and Click Confirm

# Setting Environment Variables

1. Go to AWS Management Console and use Find Services to search for Elastic Beanstalk
1. Click the multi-docker application tile
1. Click Configuration link in Sidebar
1. Select Modify in the Software tile
1. Scroll down to Environment properties
1. In another tab Open up ElastiCache, click Redis and check the box next to your cluster. Find the Primary Endpoint and copy that value but omit the :6379
1. Set REDIS_HOST key to the primary endpoint listed above, remember to omit :6379
1. Set REDIS_PORT to 6379
1. Set PGUSER to postgres
1. Set PGPASSWORD to postgrespassword
1. In another tab, open up RDS dashboard, click databases in sidebar, click your instance and scroll to Connectivity and Security. Copy the endpoint.
1. Set the PGHOST key to the endpoint value listed above.
1. Set PGDATABASE to fibvalues
1. Set PGPORT to 5432
1. Click Apply button

# IAM Keys for Deployment

1. Go to AWS Management Console and use Find Services to search for IAM
1. Click Users link in the Sidebar
1. Click Add User button
1. Set User name to multi-docker-deployer
1. Set Access-type to Programmatic Access
1. Click Next:Permissions button
1. Select Attach existing polices directly button
1. Search for 'beanstalk' and check all boxes
1. Click Next:Review
1. Add tag if you want and Click Next:Review
1. Click Create User
1. Copy Access key ID and secret access key for use later

# AWS Keys in Travis

1. Open up Travis dashboard and find your multi-docker app
1. Click More Options, and select Settings
1. Scroll to Environment Variables
1. Add AWS_ACCESS_KEY and set to your AWS access key
1. Add AWS_SECRET_KEY and set to your AWS secret key
