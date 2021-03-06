# Regions, Availability Zones, and Local Zones<a name="using-regions-availability-zones"></a>

Amazon EC2 is hosted in multiple locations world\-wide\. These locations are composed of Regions, Availability Zones, and Local Zones\. Each *Region* is a separate geographic area\. Each Region has multiple, isolated locations known as *Availability Zones*\. Local Zones provide you the ability to place resources, such as compute and storage, in multiple locations closer to your end users\. Resources aren't replicated across Regions unless you specifically choose to do so\.

Amazon operates state\-of\-the\-art, highly available data centers\. Although rare, failures can occur that affect the availability of instances that are in the same location\. If you host all of your instances in a single location that is affected by a failure, none of your instances would be available\.

**Topics**
+ [Region, Availability Zone, and Local Zone Concepts](#concepts-regions-availability-zones)
+ [Available Regions](#concepts-available-regions)
+ [Regions and Endpoints](#using-regions-endpoints)
+ [Describing Your Regions, Availability Zones, and Local Zones](#using-regions-availability-zones-describe)
+ [Specifying the Region for a Resource](#using-regions-availability-zones-setup)
+ [Opting in to a Local Zone](#using-regions-availability-zones-opt-in-local)
+ [Launching Instances in an Availability Zone or Local Zone](#using-regions-availability-zones-launching)
+ [Migrating an Instance to Another Availability Zone](#migrating-instance-availability-zone)

## Region, Availability Zone, and Local Zone Concepts<a name="concepts-regions-availability-zones"></a>

Each Region is completely independent\. Each Availability Zone is isolated, but the Availability Zones in a Region are connected through low\-latency links\. A Local Zone is an AWS infrastructure deployment that places select services closer to your end users\. A Local Zone is an extension of a Region that is in a different location from your Region\. It provides a high\-bandwidth backbone to the AWS infrastructure and is ideal for latency\-sensitive applications, for example machine learning\. The following diagram illustrates the relationship between Regions, Availability Zones, and Local Zones\.

![\[Regions, Availability Zones, and Local Zones\]](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/images/aws_regions.png)

Amazon EC2 resources are one of the following: global, tied to a Region, an Availability Zone, or a Local Zone\. For more information, see [Resource Locations](resources.md)\.

### Regions<a name="concepts-regions"></a>

Each Amazon EC2 Region is designed to be isolated from the other Amazon EC2 Regions\. This achieves the greatest possible fault tolerance and stability\.

When you view your resources, you see only the resources that are tied to the Region that you specified\. This is because Regions are isolated from each other, and we don't automatically replicate resources across Regions\.

When you launch an instance, you must select an AMI that's in the same Region\. If the AMI is in another Region, you can copy the AMI to the Region you're using\. For more information, see [Copying an AMI](CopyingAMIs.md)\.

Note that there is a charge for data transfer between Regions\. For more information, see [Amazon EC2 Pricing \- Data Transfer](https://aws.amazon.com/ec2/pricing/on-demand/#Data_Transfer)\.

### Availability Zones<a name="concepts-availability-zones"></a>

When you launch an instance, you can select an Availability Zone or let us choose one for you\. If you distribute your instances across multiple Availability Zones and one instance fails, you can design your application so that an instance in another Availability Zone can handle requests\.

You can also use Elastic IP addresses to mask the failure of an instance in one Availability Zone by rapidly remapping the address to an instance in another Availability Zone\. For more information, see [Elastic IP Addresses](elastic-ip-addresses-eip.md)\. 

An Availability Zone is represented by a Region code followed by a letter identifier; for example, `us-east-1a`\. To ensure that resources are distributed across the Availability Zones for a Region, we independently map Availability Zones to names for each AWS account\. For example, the Availability Zone `us-east-1a` for your AWS account might not be the same location as `us-east-1a` for another AWS account\.

To coordinate Availability Zones across accounts, you must use the *AZ ID*, which is a unique and consistent identifier for an Availability Zone\. For example, `use1-az1` is an AZ ID for the `us-east-1` Region and it has the same location in every AWS account\.

Viewing AZ IDs enables you to determine the location of resources in one account relative to the resources in another account\. For example, if you share a subnet in the Availability Zone with the AZ ID `use-az2` with another account, this subnet is available to that account in the Availability Zone whose AZ ID is also `use-az2`\. The AZ ID for each VPC and subnet is displayed in the Amazon VPC console\. For more information, see [Working with VPC Sharing](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-sharing.html) in the *Amazon VPC User Guide*\.

As Availability Zones grow over time, our ability to expand them can become constrained\. If this happens, we might restrict you from launching an instance in a constrained Availability Zone unless you already have an instance in that Availability Zone\. Eventually, we might also remove the constrained Availability Zone from the list of Availability Zones for new accounts\. Therefore, your account might have a different number of available Availability Zones in a Region than another account\. 

You can list the Availability Zones that are available to your account\. For more information, see [Describing Your Regions, Availability Zones, and Local Zones](#using-regions-availability-zones-describe)\.

### Local Zones<a name="network-border-group"></a>

When you launch an instance, you can optionally select a Local Zone so that your applications are closer to your end users\. 

Local Zones allow you to seamlessly connect to the full range of services in the AWS Region such as Amazon Simple Storage Service and Amazon DynamoDB through the same APIs and tool sets\.

Local Zones are not available in every Region\. For information about the Regions that support Local Zones, see [Available Regions](#concepts-available-regions)\. 

A Local Zone is represented by a Region code followed by an identifier that indicates the location, for example, `us-west-2-lax-1a`\.

To use Local Zones, you must first opt in\. For more information, see [Opting in to a Local Zone](#using-regions-availability-zones-opt-in-local)\.

After you opt in to a Local Zone, you can launch any of the following resources in the Local Zone:
+ Amazon Virtual Private Cloud subnet
+ Amazon EC2 instances
+ Amazon EBS volumes
+ Amazon FSx file servers
+ Application Load Balancer

You can list the Local Zones that are available to your account\. For more information, see [Describing Your Regions, Availability Zones, and Local Zones](#using-regions-availability-zones-describe)\.

You connect to the public internet by using an internet gateway that resides in the Local Zone\. This configuration provides customers local incoming and outgoing traffic which helps reduce latency\.

Local Zones also support connections to AWS Direct Connect, allowing customers to route their traffic over a private network connection\. 

### Network Border Groups<a name="network-border-group"></a>

A network border group is a unique set of Availability Zones or Local Zones from where AWS advertises IP addresses\. You can allocate the following resources from a network border group:
+ Elastic IPv4 addresses that Amazon provides
+ IPv6 Amazon\-provided VPC addresses

A network border group limits the addresses to the group\. IP addresses cannot move between network border groups\.

## Available Regions<a name="concepts-available-regions"></a>

Your account determines the Regions that are available to you\. For example:
+ An AWS account provides multiple Regions so that you can launch Amazon EC2 instances in locations that meet your requirements\. For example, you might want to launch instances in Europe to be closer to your European customers or to meet legal requirements\.
+ An AWS GovCloud \(US\-West\) account provides access to the AWS GovCloud \(US\-West\) Region only\. For more information, see [AWS GovCloud \(US\-West\) Region](https://aws.amazon.com/govcloud-us/)\.
+ An Amazon AWS \(China\) account provides access to the Beijing and Ningxia Regions only\. For more information, see [AWS in China](https://www.amazonaws.cn/about-aws/china/)\.

The following table lists the Regions provided by an AWS account\. You can't describe or access additional Regions from an AWS account, such as AWS GovCloud \(US\-West\) or the China Regions\. To use a Region introduced after March 20, 2019, you must enable the Region\. For more information, see [Managing AWS Regions](https://docs.aws.amazon.com/general/latest/gr/rande-manage.html) in the *AWS General Reference*\.


| Code | Name | Opt\-in Status | Local Zone | 
| --- | --- | --- | --- | 
|  `us-east-2`  |  US East \(Ohio\)  | Not required | No | 
|  `us-east-1`  |  US East \(N\. Virginia\)  | Not required | No | 
|  `us-west-1`  |  US West \(N\. California\)  | Not required | No | 
|  `us-west-2`  |  US West \(Oregon\)  | Not required | Yes \- us\-west\-2\-lax\-1a You must opt in to the Local Zone\. | 
|  `ap-east-1`  |  Asia Pacific \(Hong Kong\)  | Required | No | 
|  `ap-south-1`  |  Asia Pacific \(Mumbai\)  | Not required | No | 
|  `ap-northeast-3`  |  Asia Pacific \(Osaka\-Local\)  | Not required | No | 
|  `ap-northeast-2`  |  Asia Pacific \(Seoul\)  | Not required | No | 
|  `ap-southeast-1`  |  Asia Pacific \(Singapore\)  | Not required | No | 
|  `ap-southeast-2`  |  Asia Pacific \(Sydney\)  | Not required | No | 
|  `ap-northeast-1`  |  Asia Pacific \(Tokyo\)  | Not required | No | 
|  `ca-central-1`  |  Canada \(Central\)  | Not required | No | 
|  `eu-central-1`  |  Europe \(Frankfurt\)  | Not required | No | 
|  `eu-west-1`  |  Europe \(Ireland\)  | Not required | No | 
|  `eu-west-2`  |  Europe \(London\)  | Not required | No | 
|  `eu-west-3`  |  Europe \(Paris\)  | Not required | No | 
|  `eu-north-1`  |  Europe \(Stockholm\)  | Not required | No | 
|  `me-south-1`  |  Middle East \(Bahrain\)  | Required | No | 
|  `sa-east-1`  |  South America \(São Paulo\)  | Not required | No | 

For more information, see [AWS Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)\.

The number and mapping of Availability Zones per Region may vary between AWS accounts\. To get a list of the Availability Zones that are available to your account, you can use the Amazon EC2 console or the command line interface\. For more information, see [Describing Your Regions, Availability Zones, and Local Zones](#using-regions-availability-zones-describe)\.

## Regions and Endpoints<a name="using-regions-endpoints"></a>

When you work with an instance using the command line interface or API actions, you must specify its Regional endpoint\. For more information about the Regions and endpoints for Amazon EC2, see [Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#ec2_region) in the *Amazon Web Services General Reference*\.

For more information about endpoints and protocols in AWS GovCloud \(US\-West\), see [AWS GovCloud \(US\-West\) Endpoints](https://docs.aws.amazon.com/govcloud-us/latest/UserGuide/using-govcloud-endpoints.html) in the *AWS GovCloud \(US\) User Guide*\.

## Describing Your Regions, Availability Zones, and Local Zones<a name="using-regions-availability-zones-describe"></a>

You can use the Amazon EC2 console or the command line interface to determine which Regions, Availability Zones, and Local Zones are available for your account\. For more information about these command line interfaces, see [Accessing Amazon EC2](concepts.md#access-ec2)\.

**To find your Regions, Availability Zones, and Local Zones using the console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. From the navigation bar, view the options in the Region selector\.  
![\[View your Regions\]](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/images/EC2_select_region.png)

1. On the navigation pane, choose **EC2 Dashboard**\.

1. The Availability Zones and Local Zones are listed under **Service Health**, **Availability Zone Status**\.

**To find your Regions, Availability Zones, and Local Zones using the AWS CLI**

1. Use the [describe\-regions](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-regions.html) command as follows to describe the Regions that are enabled for your account\.

   ```
   aws ec2 describe-regions
   ```

   To describe all Regions, including any Regions that are disabled for your account, add the `--all-regions` option as follows\.

   ```
   aws ec2 describe-regions --all-regions
   ```

1. Use the [describe\-availability\-zones](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-availability-zones.html) command as follows to describe the Availability Zones and Local Zones within the specified Region\.

   ```
   aws ec2 describe-availability-zones --region region-name
   ```

1. Use the [describe\-availability\-zones](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-availability-zones.html) command as follows to describe the Availability Zones and Local Zones regardless of the opt\-in status\.

   ```
   aws ec2 describe-availability-zones --all-availability-zones
   ```

**To find your Regions, Availability Zones, and Local Zones using the AWS Tools for Windows PowerShell**

1. Use the [Get\-EC2Region](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-EC2Region.html) command as follows to describe the Regions for your account\.

   ```
   PS C:\> Get-EC2Region
   ```

1. Use the [Get\-EC2AvailabilityZone](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-EC2AvailabilityZone.html) command as follows to describe the Availability Zones within the specified Region\.

   ```
   PS C:\> Get-EC2AvailabilityZone -Region region-name
   ```

## Specifying the Region for a Resource<a name="using-regions-availability-zones-setup"></a>

Every time you create an Amazon EC2 resource, you can specify the Region for the resource\. You can specify the Region for a resource using the AWS Management Console or the command line\.

**Note**  
Some AWS resources might not be available in all Regions, Availability Zones, and Local Zones\. Ensure that you can create the resources that you need in the desired Regions or Availability Zones before launching an instance in a specific Availability Zone\.

**To specify the Region for a resource using the console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. Use the Region selector in the navigation bar\.  
![\[Use the console Region selector\]](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/images/EC2_select_region.png)

**To specify the default Region using the command line**

You can set the value of an environment variable to the desired Regional endpoint \(for example, `https://ec2.us-east-2.amazonaws.com`\):
+ `AWS_DEFAULT_REGION` \(AWS CLI\)
+ `Set-AWSDefaultRegion` \(AWS Tools for Windows PowerShell\)

Alternatively, you can use the `--region` \(AWS CLI\) or `-Region` \(AWS Tools for Windows PowerShell\) command line option with each individual command\. For example, `--region us-east-2`\.

For more information about the endpoints for Amazon EC2, see [Amazon Elastic Compute Cloud Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#ec2_region)\.

## Opting in to a Local Zone<a name="using-regions-availability-zones-opt-in-local"></a>

Before you specify a Local Zone for a resource, or service, you must opt in to the Local Zone\.

To a Local Zone, go to [AWS Local Zone](https://aws.amazon.com/local-zones/) site, and then make a request\. 

## Launching Instances in an Availability Zone or Local Zone<a name="using-regions-availability-zones-launching"></a>

When you launch an instance, select a Region that puts your instances closer to specific customers, or meets the legal or other requirements that you have\. By launching your instances in separate Availability Zones, you can protect your applications from the failure of a single location\. 

By launching an instance in a Local Zone, you can run latency\-sensitive applications close to your end users while having the benefits of the AWS infrastructure\.

When you launch an instance, you can optionally specify an Availability Zone or Local Zone in the Region that you are using\. If you do not specify an Availability Zone or Local Zone, we select an Availability Zone for you\. When you launch your initial instances, we recommend that you accept the default Availability Zone, because this enables us to select the best Availability Zone for you based on system health and available capacity\. If you launch additional instances, specify an Availability Zone only if your new instances must be close to, or separated from, your running instances\.

## Migrating an Instance to Another Availability Zone<a name="migrating-instance-availability-zone"></a>

If necessary, you can migrate an instance from one Availability Zone to another\. For example, let's say you are trying to modify the instance type of your instance and we can't launch an instance of the new instance type in the current Availability Zone\. In this case, you can migrate the instance to an Availability Zone where we are able to launch an instance of that instance type\.

The migration process involves:
+ Creating an AMI from the original instance
+ Launching an instance in the new Availability Zone
+ Updating the configuration of the new instance, as shown in the following procedure

**To migrate an instance to another Availability Zone**

1. Create an AMI from the instance\. The procedure depends on your operating system and the type of root device volume for the instance\. For more information, see the documentation that corresponds to your operating system and root device volume:
   + [Creating an Amazon EBS\-Backed Linux AMI](creating-an-ami-ebs.md)
   + [Creating an Instance Store\-Backed Linux AMI](creating-an-ami-instance-store.md)
   + [Creating an Amazon EBS\-Backed Windows AMI](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/Creating_EBSbacked_WinAMI.html)

1. If you need to preserve the private IPv4 address of the instance, you must delete the subnet in the current Availability Zone and then create a subnet in the new Availability Zone with the same IPv4 address range as the original subnet\. Note that you must terminate all instances in a subnet before you can delete it\. Therefore, you should create AMIs from all of the instances in your subnet so that you can move all instances from the current subnet to the new subnet\.

1. Launch an instance from the AMI that you just created, specifying the new Availability Zone or subnet\. You can use the same instance type as the original instance, or select a new instance type\. For more information, see [Launching Instances in an Availability Zone or Local Zone](#using-regions-availability-zones-launching)\.

1. If the original instance has an associated Elastic IP address, associate it with the new instance\. For more information, see [Disassociating an Elastic IP Address and Reassociating with a Different Instance](elastic-ip-addresses-eip.md#using-instance-addressing-eips-associating-different)\.

1. If the original instance is a Reserved Instance, change the Availability Zone for your reservation\. \(If you also changed the instance type, you can also change the instance type for your reservation\.\) For more information, see [Submitting Modification Requests](ri-modifying.md#ri-modification-process)\.

1. \(Optional\) Terminate the original instance\. For more information, see [Terminating an Instance](terminating-instances.md#terminating-instances-console)\.