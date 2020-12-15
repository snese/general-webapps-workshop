---
title: "IAM Lab"
chapter: false
weight: 30
tags:
  - IAM
  - EC2
  - IAM Role
  - IAM User
  - IAM Group
---

|  | |
| --- | --- |
| 步驟一 | 下載 Amazon CloudFormation 範本 |
| 步驟二 | 透過 Amazon CloudFormation 建立資源 |
| 步驟三 | 現有 IAM 使用者及規則驗證及操作 |
| 步驟四 | 透過 IAM Group 授權 |
| 步驟五 | 透過 IAM Role 授權 |
| 步驟六 | 刪除資源 |

### 步驟一

下載 Amazon CloudFormation 範本 IAMLAB.yaml

[https://github.com/hhh2012aa/IAM-LAB/blob/main/IAMLAB.yaml](https://github.com/hhh2012aa/IAM-LAB/blob/main/IAMLAB.yaml)

### 步驟二

透過 Amazon CloudFormation 建立以下資源

1. AWS IAM Users:
    * Sally
    * Anne
    * John

2. AWS IAM Group:
    * Contractors

3. AWS custom policy:
    * departmental-ec2-access
    * contractorsroleassumptionpolicy

4. AWS Role:
    * ec2poweruser

5. Amazon EC2:
    * hr_instance
    * finance_instance

2.1

開啟 CloudFormation 管理頁面

[https://console.aws.amazon.com/cloudformation/home?region=us-east-1](https://console.aws.amazon.com/cloudformation/home?region=us-east-1)

![](https://i.imgur.com/dAvdy2X.png)


2.2

點選 Create stack

![](https://i.imgur.com/WRVSP4p.png)


2.3

![](https://i.imgur.com/9ik6R1A.png)


2.4

![](https://i.imgur.com/XY34kld.png)


2.5

使用預設設定值，點選下一步

![](https://i.imgur.com/4JaNa5w.png)


2.6

勾選確認建立 IAM:Role，點擊 Create stack

![](https://i.imgur.com/YMCyvCH.png)


### 步驟三

現有 IAM 使用者規則驗證及操作

3.1

進入 AWS IAM 管理頁面，取得登入 AWS Console URL

![](https://i.imgur.com/P8WgFEO.png)


![](https://i.imgur.com/LsW2Lnq.png)


3.2

開啟新的無痕瀏覽器頁面，建議使用不同的瀏覽器操作

使用上一步驟得到的 URL 進行登入

範例：[https://540276846281.signin.aws.amazon.com/console](https://540276846281.signin.aws.amazon.com/console)

輸入 User 名稱：Anne

輸入 User 密碼：12345678aA

![](https://i.imgur.com/Bc3VAVk.png)


3.3

嘗試以下操作

1. 進入 AWS EC2 管理畫面，將 EC2(hr\_instance) 開機(START) or 關機(STOP)
2. 進入 AWS EC2 管理畫面，將 EC2(finance\_instance) 開機(START) or 關機(STOP)
3. 進入 AWS EC2 管理畫面，將 EC2(hr\_instance) 刪除(TERMINATE)
4. 進入 AWS IAM 管理頁面

![](https://i.imgur.com/g01H6HU.png)


![](https://i.imgur.com/2221QJn.png)


![](https://i.imgur.com/uai3DH2.png)


![](https://i.imgur.com/hw43FY6.png)

3.4

登出目前使用者，並登入使用者 Sally，重複上述 3.2 及 3.3 操作

輸入 User 名稱：Sally

輸入 User 密碼：12345678aA

### 步驟四

透過 IAM Group 授權

4.1

登出目前使用者，並登入使用者 John，

輸入 User 名稱：John

輸入 User 密碼：12345678aA

嘗試以下操作

1. 進入 AWS EC2 管理畫面
2. 進入 AWS IAM 管理頁面

4.2

切換回到 root 使用者操作視窗，並進入 AWS IAM 管理頁面，點擊左側 Group

執行新增使用者操作，將使用者 John 加入 AWS Group(Contractors)

![](https://i.imgur.com/BO1ItzE.png)


![](https://i.imgur.com/uuJQLRJ.png)


4.3

切換回使用者 John 操作畫面，嘗試以下操作

1. 進入 AWS EC2 管理畫面
2. 進入 AWS IAM 管理頁面

### 步驟五

透過 AssumeRole 授權

5.1

於 AWS IAM Role 管理頁面，取得 AssumeRole 登入 URL

![](https://i.imgur.com/6aSLWES.png)

![](https://i.imgur.com/tnSXfBw.png)


5.2

使用者 John 透過前步驟中 Switch URL 切換至 AssumeRole (ec2poweruser) ，取得 AWS EC2 管理權限

URL 範例：[https://signin.aws.amazon.com/switchrole?roleName=ec2poweruser&amp;account=540276846281](https://signin.aws.amazon.com/switchrole?roleName=ec2poweruser&amp;account=540276846281)

嘗試以下操作

1. 進入 AWS EC2管理畫面，將 EC2(hr\_instance) 開機(START) or 關機(STOP)
2. 進入 AWS EC2管理畫面，將 EC2(finance\_instance) 開機(START) or 關機(STOP)
3. 進入 AWS EC2管理畫面，將 EC2(hr\_instance) 刪除(TERMINATE)
4. 進入 AWS IAM管理頁面

![](https://i.imgur.com/u9dZtPd.png)

![](https://i.imgur.com/HB6Drql.png)

![](https://i.imgur.com/7ddmFh6.png)


5.3

登出目前使用者 John，並嘗試使用使用者 Sally 登入 AssumeRole URL

URL 範例：[https://signin.aws.amazon.com/switchrole?roleName=ec2poweruser&amp;account=540276846281](https://signin.aws.amazon.com/switchrole?roleName=ec2poweruser&amp;account=540276846281)

![](https://i.imgur.com/gNl6NVJ.png)


5.4

切換回到 root 使用者操作視窗，並進入 AWS IAM 管理頁面，點擊左側 User

新增 custom policy(contractorsroleassumptionpolicy) 給使用者 Sally

![](https://i.imgur.com/Ob8dVn6.png)

![](https://i.imgur.com/4w7YEWJ.png)

![](https://i.imgur.com/m80AsY9.png)


5.5

重複操作步驟 5.3，並嘗試以下操作:

* 進入 AWS EC2 管理畫面，將 EC2(finance\_instance) 刪除(TERMINATE)

### 步驟六

刪除資源

6.1

刪除

1. AWS IAM Users:
    * Sally
    * Anne
    * John

2. AWS IAM Group:
    * Contractors

3. AWS custom policy:
    * departmental-ec2-access
    * contractorsroleassumptionpolicy

4. AWS Role:
    * ec2poweruser

5. CloudFormation stack(IAM-Workshop)