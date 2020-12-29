---
title: "Step 4. Modify Security Groups of RDS and EC2 instance"
chapter: false
weight: 30
---

### 4. Modify Security Groups of RDS and EC2 instance

* Visit [**Security Groups**](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#SecurityGroups:) page, select `public-instance-sg`, 
  * Click **Edit inbound rules** button
  * Click **Add rule**, For **Type**, select **MYSQL/Aurora**, 
  * For **Source**, select custom and find the `db-sg` , and click **Save rules**
  * Click **Add rule**, For **Type**, select **HTTP**, 
  * For **Source**, select **My IP** , and click **Save rules**

* Visit [**Security Groups**](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#SecurityGroups:) page, select `db-sg` , 
  * Click **Edit inbound rules** button
  * Click **Add rule**, For **Type**, select **MYSQL/Aurora**, 
  * For **Source**, select custom and find the `public-instance-sg` , and click **Save rules**

