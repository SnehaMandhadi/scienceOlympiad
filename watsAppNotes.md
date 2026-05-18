### EC2 instances

1. Ec2 instance types 
2. We generally use general instance
3. Check what is your laptop cpu, memory, ram
4. What instance will you deploy in your org = depending on application type  your application you are deployin

### Route 53
1. Real world DNS names will be mapped to load balancer ip address
Igw(vpc)- route53-loadbalacer(public subnets)-private subnets application
2. Rote53= domain  registration
3. Domain name= you can buy from go daddy Or you can get from AWS itself
4. Ec2 Ipadress can be static or dynamic
5. Request comes from igw- route53
6. Route53 checks in dns records and maps dns name  to its ipadress
7. Buy domain name
8. Register domain registration 
9. We need to put them in hosted zones
10. In hosted zones you keep dns records(table with domain name and ipadress)
11. Hosted zones can be public or private 
12. If your request met in dns records then Route53 will resolve your dns name  to ipadress from there req goes ahead
13. Route53 supports health check  for every 1min or so on web application or web servers  
    **IQ**: Have you implemented public private subnets architecture in AWS
What is the traffic flow

### Difference between Bastion host and NAT gateway

    Bastion host: used for inbound acccess into private subnet  

    Eg: if admin want to ssh into private instances


    NAT Gateway:  used for outboaund access from private subnte to internet  
        -  live in public subnet
        eg: private servers downloading updates,calling external APis

### DC,AZ

1. Dc->AZ->region
2. 1 region will have mulitiple Az,1AZ have mulitple Data centre

### S3
1. Tip: Smart Hackers Save Costly Photos Daily
2. S-scalable(amount of data uploading into S3 bucket is not restricted) H-Highly available S-secure C-cost effective P-performance(loads fast globally) D-durable(11 9's never get lost)
3. Encryption at rest, encryption at transit
4. storage-SSDs,HDDs,
5. If you want reading or write datat into your hardisk is to be quick is very costly. But you are not bothered about reading and writing only I want to store a data =it is cheap


### IAM Policy
1. TIp=vest EAR
2. Version 