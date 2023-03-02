# Amazon Elastic ompute Cloud(Amazon EC2)

Amazon Elastic Compute Cloud ( Amazon EC2 ) is a web service that providesresizable compute capacity in the cloud . It is designed to make web-scale cloudcomputing easier for developers

Amazon Ec2's simple web service interface allows you to obtain and configurecapacity with minimal friction . It provides you with complete control of yourcomputing resources and lets you run on Amazons proven computing environmentAmazon EC2 reduces the time required to obtain and boot new server instances tominutes , allowing you to quickly scale capacity , both up and down , as yourcomputing requirements change

Amazon EC2 changes the economics of computing by allowing you to pay only forcapacity that you actually use Amazon EC2 provides developers the tools to buildailure resilient applications and isolate themselves from common failurescenarios


# Topics covered

By the end of this lab , you will be able to

 * Launch a web server with termination protection enabled

 * Monitor Your EC2 instance 

 * Modify the security group that your web server is using to allow HTTP access 

 * Resize your Amazon EC2 instance to scalexplore EC2 limits

 * Test termination protection

 * Terminate your EC2 Instance


# Task 1 : Launch Your Amazon EC2nstance

In this task , you will launch an Amazon EC2 instance with terminationprotection Termination protection prevents you from accidentalterminating an EC2 instance . Your instance will include a User Data scriptthat will install a simple web server

3. From the `AWS Management Console` , use the `AWS search bar` to searchfor `EC2` and then choose the service from the list of results

4. At the top left of the screen , if you see `New EC2 Experience` in the top-leftof the screen , ensure `New EC2 Experience` is selected

5. Choose the `Launch instance`  drop down menu and choose `Launchinstance`

6. In the `Name and tags` section enter `Web Server` in the Name box

7. In the `Application and OS Images ( Amazon Machine Image )` section ,select `Amazon Linux` on the `Quickstart` tab

An Amazon Machine Image ( AMD ) provides the information required toaunch an instance which is a virtual server in the cloud An AMI includes


 * A template for the root volume for the instance ( for example , an operatingsystem or an application server with applications

 * Launch permissions that control which AWS accounts can use the AMI toaunch instances

 * A block device mapping that specifies the volumes to attach to theinstance when it is launched

 The `Quick Start` list contains the most commonly-used AMIS . You can alsocreate your own AMI or select an AMI from the AWS Marketplace , an onlinestore where you can sell or buy software that runs on AWS

 8. In the Instance Type section , choose the Instance type drop down menuand choose t3 micro


Amazon EC2 provides a wide selection of instance types optimized to fitdifferent use cases . Instance types comprise varying combinations of CPU ,memory , storage , and networking capacity and give you the flexibility tochoose the appropriate mix of resources for your applications . Each instancetype includes one or more instance sizes , allowing you to scale youresources to the requirements of your target workload


A t3 micro instance type has 2 virtual CPUS and 1 GIB of memory

9. In the Key pair (login) section , locate the Key pair name drop down menuand choose Proceed without a key pair (Not recommended)

Amazon EC2 uses public-key cryptography to encrypt and decrypt loginnformation . To log in to your instance , you must create a key pair , specify thename of the key pair when you launch the instance , and provide the privatekey when you connect to the instance.


10. In the `Network settings` section , choose the `Edit` button . Make thefollowing selections


11. VPC : Choose the VPC with the name that contains `Lab VPC`

12. Subnet : Choose the Subnet with the name that containsPublic Subnet 1

The Network indicates which Virtual Private Cloud ( VPC ) you wish toaunch the instance into . You can have multiple networks , such as differentones for development , testing and production.


The Lab VPC was created using a Cloudformation template during the setupprocess of your lab This VPC includes two public subnets in two differentAvailability Zones


11. In the `Firewall ( security groups )` section , chooseo `Create security group`


 * Security group name = `Web Server security group`
 * Description = `Security group for my web serve`

 A security group acts as a virtual firewall that controls the traffic for one ormore instances When you launch an instance , you associate one or moresecurity groups with the instance You add rules to each security group thallow traffic to or from its associated instances . You can modify the rules fora security group at any time ; the new rules are automatically applied to alIstances that are associated with the security group

In this lab , you will not log into your instance using SSH . Removing SSHaccess will improve the security of the instance


12. Choose the `Remove` button to remove the existing SSH rule . Youshould have no security group rules

13. The Configure storage section default choices can be left alone



Amazon EC2 stores data on a network-attached virtual disk called ElasticBlock Store


You will launch the Amazon EC2 instance using a default 8 GIB disk volumehis will be your root volume ( also known as a boot volume )

14. Expand the `Advanced details` section . Scroll down to the `Termination protection` drop down menu and set to `Enable`

When an Amazon EC2 instance is no longer required, it can be terminatedwhich means that the instance is stopped and its resources are released. Aerminated instance cannot be started again. If you want to prevent thenstance from being accidentally terminated, you can enable terminationprotection for the instance, which prevents it from being terminated


15. Scroll all the way to the bottom until you see a field for User data

When you launch an instance , you can pass user data to the instance thatcan be used to perform common automated configuration tasks and evenrun scripts after the instance starts


16. Your instance is running Amazon Linux , so you will provide a shell scriptthatwill run when the instance starts

```
#!/bin/bash
yum -y install httpd
systemctl enable httpd
systemctl start httpd
echo '<html><h1>Hello From Your Web Server!</h1></html>' > /var/www/html/index.html
```


The script will:

 * Install an Apache web server(http)
 * dConfigure the web server to automatically start on boot
 * Activate the Web server
 * Create a simple web page

17. Choose Launch instance

✅ Expected output :

 ✅ Success

 18. Choose Instances from the collapsible menu on the left pane . You mayneed to expand the menu to see this option

 The instance might appear in a pending state , which means it is beingaunched It will then change to running which indicates that the instancehas started booting . When creating a new instance , there will usually be ashort time before you can access the instance

 19. Wait for your instance to display the following:

  * Instance state : ✅ Running.
  * Status check : ✅ 02/2 checks passed.

Periodically refresh the page if you don't see a change in the Instance stateor Status check values

Select the ✔️ for your newly-created `Web Server` and the `Details` tab displaysdetailed information about your instance


To view more information in the Details tab , drag the window dividerupwards


Review the information displayed in the `Details` tabcludes informationabout the instance type , security settings , network settings , and more . Thestance receives a Pub / ic / PV4 DNS name that you can use to communicateith the instance from the Internet


# Task 2 : Monitor Your Instance

Monitoring is an important part of maintaining the reliability , availability , andperformance of your Amazon Elastic Compute Cloud ( Amazon EC2 )nstances and your AWS solutions

20. Select the `Status checks`tab

with instance status monitoring , you can quickly determine whetherAmazon EC2 has detected any problems that might prevent your instancefrom running applications . Amazon EC2 performs automated checks onevery running EC2 instance to identify hardware and software issues


Notice that both the System reachability and Instance reachability checkshave passed


21. Select the `Monitoring` tab

This tab displays Cloud Watch metrics for your instance . Currently , there arenot many metrics to display because the instance was recently launched


You can choose a graph to see an expanded view

Amazon EC2 sends metrics to Amazon Cloudwatch for your EC2stances . Basic ( Five-minute ) monitoring is enabled by default . You canenable detailed ( one-minute ) monitoring

22. Select the actions menu (in the upper right of the console) choose `Monitor and troubleshoot`, and select Get system log.


✅ Expected output：


Note: If you do not see a system log wait a couple of minutes and refreshthe log screen until it appears.


The System Log displays the console output of the instance , which is avaluable tool for problem diagnosis. It is especially useful for troubleshootingkernel problems and service configuration issues that could cause alstance to terminate or become unreachable before its SSH daemon can bestarted



23. Scroll through theout put and note that the httpd package was installed from the user data that you added when you created the instance

24. Scroll down to the bottom of the browser window and select Cancel

25. Select the ✅ for `Web Server` , then select the `Actions - menu` , choose `Monitor and troubleshoot `) and select `Get instance screenshot`

✅ Expected output：

This shows you what your Amazon EC2 instance console would look like if ascreen were attached to it.

If you are unable to reach your instance via SSH or RDP , you can capture ascreenshot of your instance and view it as an image . This provides visibilityas to the status of the instance , and allows for quicker troubleshooting


26. Scroll down to the bottom of the browser window and select Cancel


Congratulations! You have explored several ways to monitor yourstance.


# Task 3 : Update Your Security Groupand Access the Web Server

When you launched the EC2 instance , you provided a script that installed aweb server and created a simple web page . In this task , you will accesscontent from the web server.


27. Select the ✅ for `Web Server `, then choose the `Details` tab

28. Copy the `Public Ipv4 address` of your instance to your clipboard

29. Open a new tab in your web browser , paste the IP address you justcopied , then press `Enter`

Consider:

Are you able to access your web server ? Why not ?

You are not currently able to access your web server because the securitygroupisnotpermittinginboundtrafficonport80,WhichisusedforHTTPweb requests . This is a demonstration of using a security group as a firewalto restrict the network traffic that is allowed in and out of an instance

To correct this , you will now update the security group to permit web trafficon port 80


30. Keep the browser tab open , but return to the `EC2 Management Console`


31. In the left navigation pane , select `Security Groups`

32. Select the ✅ for the `Security group ID` with the Security group name `Web Server security group`

The security group currently has no rules.


33. Choose the Inbound rules tab

34. Choose `Edit inbound rules`

35. Choose `Add rule `then configure:

  * Type: `HTTP`
  * Source : `Anywhere-IPv4 `


36. choose `save rules`

The new Inbound HTTP rule will create anentry for both IPV4 IP address(0.0.0.0/0) as well as IPV6 IP address (::/0)


note: using "Anywhere" , or more specifically using 0.0.0.0/0 or a is not a recommended best practice for production workloads.

37. Return to the web server tab that you previously opened and refresh - sthe paqe


Expected output:


You should see the message Hello From Your Web Server


Congratulations! You have successfully modified your security group topermit HTTP traffic into your Amazon EC2 Instance

# Task 4 : Resize Your Instance : Instanceype andBS Volume


As your needs change , you might find that your instance is over-utilized ( toosmall ) or under-utilized ( too large ) . If so , you can change the instance typeFor example , if a t3 micro instance is too small for its workload , you carchange it to an t3 . small instance . Similarly , you can change the size of adisk


Stop Your Instance

Before you can resize an instance , you must stop it

When you stop an instance , it is shut down . There is no charge for astopped EC2 instance , but the storage charge for attached Amazon EBSvolumes remains

38. On the `EC2 Management Console` in the left navigation pane , chooses `Instances`

39. if it is not already selected , select the M for `Web Server`

40. Select `Instance state` , then Stop instance

41. Choose `Stop`

Your instance will perform a normal shutdown and then will stop runningThis may take a couple minutes

42. Wait for the `Instance State ` to display : Stopped


Change The Instance Type

43. If it is not already selected , select the ✅ for `Web Server`

44. Select the Actions v menu , select `Instance settings` and `Change instance type `, then configure

  * Instance type: t3.Small

45. Choose `Apply`


When the instance is started again it will be a t3 smal , which has twice asmuch memory as a t3 micro instance


Resize the EBS Volume


46. In the left navigation pane , select `Volumes` from the `Elastic Block Storesection`


47. Select ✅ the volume there

48. In the `Actions` menu , select `Modify volume`

The disk volume currently has a size of 8 GIB . You will now increase the sizeof this disk

49. Change the `size (GIB)` to : `10`

50. Choose `Modify`

51. Choose `Modify` to confirm and increase the size of the volume


Start the Resized Instance


You will now start the instance again , which will now have more memoryand more disk space

52. In left navigation pane , select `Instances`

53. Select ✔️ the Web Server

54. Select `Instance state ` and then `Start instance`

Note An EBS volume being modified goes through a sequence of statesModifying , Optimizing , and finally Complete

Congratulations ! You have successfully resized your Amazon EC2Instance . In this task you changed your instance type from t3 micro to at3 . small . You also modified your root disk volume from 8 GIB to 10 GIB

# Task 5 : Explore EC2 Limits

Amazon EC2 provides different resources that you can use These resourcesnclude images , instances , volumes and snapshots When you create anAWS account , there are default limits on these resources on a per-regionbasis

55. In the left navigation pane , select `Limits`


56. Change the All limits v drop down filter to `Running instances`

Notice that there is a limit for the instance type based on the number ofVCPUS required . For example , Running On-demand All Standard ( A , C , DM , R , T , Z ) instances , has a limit of 1152 VCPUS . A t3 micro instancerequires 2 VCPUS . Therefore , in this region , you could launch 576 t3 microstances in this region

You can request an increase for many of these limits in your own AWSaccount

# Task 6 : Test Termination Protection

You can delete your instance when you no longer need it . This is referred toas terminating your instance . You cannot connect to or restart an instanceafter it has been terminated

In this task , you will learn how to use termination protection

57. In left navigation pane , select Instances

58. Select ✔️ the Web Server

59. Note that in the Instance state w , the Terminate instance optioneyed out

This is a safeguard to prevent the accidental termination of an instanceyou really want to terminate the instance , you will need to disable thetermination protection

60. Select Actions v choose `Instance settings` , and `Change terminationprotection`

61. Unselect Enable

62. Choose `Save`

You can now terminate the instance

63. Refresh [] the instance console screen

64. Select ✔️ the Web Server

65. Choose Instance state v , and Terminate instance

66. Choose Terminate

✅ Expercted output:

The Instance state of the Web Server instance should change to Terminatedafter about 30 seconds . You may have to refresh the page a few times

# Additional Resources

 * [Introduction to Amazon EC2](https://amazon.qwiklabs.com/focuses/57537?catalog_rank=%7B%22rank%22%3A3%2C%22num_filters%22%3A0%2C%22has_search%22%3Atrue%7D&parent=catalog&search_id=22581955)
 * [https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/LaunchingAndUsingInstances.html]










































































































