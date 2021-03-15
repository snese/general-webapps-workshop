## AWS General WebApps Workshop

This workshop is a hands-on tutorial of building website on AWS. In this workshop, you will learn how to build a high availability application on AWS with [**AWS Management Console**](https://aws.amazon.com/console/). In lab 1, we will build single-instance WordPress website using [**Amazon EC2**](https://aws.amazon.com/ec2)and [**Amazon RDS**](https://aws.amazon.com/rds/), then further improve the architecture of by adding [**Auto Scaling Group**](https://docs.aws.amazon.com/autoscaling/ec2/userguide/AutoScalingGroup.html) , Load Balancer ([**Elastic Load Balancing**](https://aws.amazon.com/elasticloadbalancing/?nc=sn&loc=0)) and CDN ([**AWS CloudFront**](https://aws.amazon.com/cloudfront)) in lab 2 to make the website more scalable, secure and reliable.

* Target audience: AWS Beginners
* Level: 100 
* Region: us-east-1

![](/images/lab2-architecture.jpg)

### Cost

The extimated cost (USD) is shown in below:

* Region: us-east-1
* Duration: 2 hours

* EC2: 0.0116 (t2.micro) * 2 (hours) * 3 (instances) = 0.0696 
* RDS: 0.017 * 2 (hours) = 0.034
* S3: 0.023 (storage) + 0.0054 (Requests) = 0.0284
* ALB: (0.0225 (hourly charge) + 0.008 (LCU charge) ) * 2 (hours) = 0.061
* CloudFront: 0.085 (Regional Data Transfer Out to Internet) + 0.0100 (HTTPS requests) = 0.095
* Total: ~ 0.3 (USD)

