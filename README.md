<p align="center"> <img src="https://user-images.githubusercontent.com/50652676/62349836-882fef80-b51e-11e9-99e3-7b974309c7e3.png" width="100" height="100"></p>


<h1 align="center">
    Terraform AWS EC2
</h1>

<p align="center" style="font-size: 1.2rem;"> 
    Terraform module to create an EC2 resource on AWS with ElasticC IP Addresses and Elastic Block Store.
     </p>

<p align="center">

<a href="https://www.terraform.io">
  <img src="https://img.shields.io/badge/Terraform-v0.13-green" alt="Terraform">
</a>
<a href="LICENSE.md">
  <img src="https://img.shields.io/badge/License-MIT-blue.svg" alt="Licence">
</a>


</p>
<p align="center">

<a href='https://facebook.com/sharer/sharer.php?u=https://github.com/devops4mecode/terraform-aws-ec2'>
  <img title="Share on Facebook" src="https://user-images.githubusercontent.com/50652676/62817743-4f64cb80-bb59-11e9-90c7-b057252ded50.png" />
</a>
<a href='https://www.linkedin.com/shareArticle?mini=true&title=Terraform+AWS+EC2&url=https://github.com/devops4mecode/terraform-aws-ec2'>
  <img title="Share on LinkedIn" src="https://user-images.githubusercontent.com/50652676/62817742-4e339e80-bb59-11e9-87b9-a1f68cae1049.png" />
</a>
<a href='https://twitter.com/intent/tweet/?text=Terraform+AWS+EC2&url=https://github.com/devops4mecode/terraform-aws-ec2'>
  <img title="Share on Twitter" src="https://user-images.githubusercontent.com/50652676/62817740-4c69db00-bb59-11e9-8a79-3580fbbf6d5c.png" />
</a>

</p>
<hr>
## Prerequisites

This module has a few dependencies: 

- [Terraform 0.13](https://learn.hashicorp.com/terraform/getting-started/install.html)
- [Go](https://golang.org/doc/install)
- [github.com/stretchr/testify/assert](https://github.com/stretchr/testify)
- [github.com/gruntwork-io/terratest/modules/terraform](https://github.com/gruntwork-io/terratest)







## Examples


**IMPORTANT:** Since the `master` branch used in `source` varies based on new modifications, we suggest that you use the release versions [here](https://github.com/devops4mecode/terraform-aws-ec2/releases).


Here is examples of how you can use this module in your inventory structure:
### Basic Example
```hcl
    module "ec2" {
      source                      = "devops4mecode/ec2/aws"
      version                     = "1.0.0"
      application                 = "devops4me-ec2"
      environment                 = "test"
      label_order                 = ["environment", "application", "name"]
      instance_count              = 2
      ami                         = "ami-08d658f84a6d84a80"
      instance_type               = "t2.nano"
      monitoring                  = false
      tenancy                     = "default"
      vpc_security_group_ids_list = [module.ssh.security_group_ids, module.http-https.security_group_ids]
      subnet_ids                  = tolist(module.public_subnets.public_subnet_id)
      assign_eip_address          = true
      associate_public_ip_address = true
      instance_profile_enabled    = true
      iam_instance_profile        = module.iam-role.name
      disk_size                   = 8
      ebs_optimized               = false
      ebs_volume_enabled          = true
      ebs_volume_type             = "gp2"
      ebs_volume_size             = 30
      instance_tags               = { "snapshot" = true }
      dns_zone_id                 = "Z1XJD7SSBKXLC1"
      hostname                    = "ec2"
    }
```

### Secure Example
```hcl
    module "ec2" {
      source                      = "devops4mecode/ec2/aws"
      version                     = "1.0.0"
      application                 = "devops4me-ec2"
      environment                 = "test"
      label_order                 = ["environment", "application", "name"]
      instance_count              = 2
      ami                         = "ami-08d658f84a6d84a80"
      instance_type               = "t2.nano"
      monitoring                  = false
      tenancy                     = "default"
      vpc_security_group_ids_list = [module.ssh.security_group_ids, module.http-https.security_group_ids]
      subnet_ids                  = tolist(module.public_subnets.public_subnet_id)
      assign_eip_address          = true
      associate_public_ip_address = true
      instance_profile_enabled    = true
      iam_instance_profile        = module.iam-role.name
      disk_size                   = 8
      ebs_optimized               = false
      ebs_volume_enabled          = true
      ebs_volume_type             = "gp2"
      ebs_volume_size             = 30
      encrypted                   = true
      kms_key_id                  = module.kms_key.key_arn
      instance_tags               = { "snapshot" = true }
      dns_zone_id                 = "Z1XJD7SSBKXLC1"
      hostname                    = "ec2"
    }
```






## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| ami | The AMI to use for the instance. | `string` | n/a | yes |
| application | Application (e.g. `do4m` or `devops4me`). | `string` | `""` | no |
| assign\_eip\_address | Assign an Elastic IP address to the instance. | `bool` | `false` | no |
| associate\_public\_ip\_address | Associate a public IP address with the instance. | `bool` | `true` | no |
| attributes | Additional attributes (e.g. `1`). | `list` | `[]` | no |
| availability\_zone | Availability Zone the instance is launched in. If not set, will be launched in the first AZ of the region. | `list` | `[]` | no |
| cpu\_core\_count | Sets the number of CPU cores for an instance. | `string` | `null` | no |
| cpu\_credits | The credit option for CPU usage. Can be `standard` or `unlimited`. T3 instances are launched as unlimited by default. T2 instances are launched as standard by default. | `string` | `"standard"` | no |
| delimiter | Delimiter to be used between `organization`, `environment`, `name` and `attributes`. | `string` | `"-"` | no |
| disable\_api\_termination | If true, enables EC2 Instance Termination Protection. | `bool` | `false` | no |
| disk\_size | Size of the root volume in gigabytes. | `number` | `8` | no |
| dns\_enabled | Flag to control the dns\_enable. | `bool` | `false` | no |
| dns\_zone\_id | The Zone ID of Route53. | `string` | `""` | no |
| ebs\_block\_device | Additional EBS block devices to attach to the instance. | `list` | `[]` | no |
| ebs\_device\_name | Name of the EBS device to mount. | `list(string)` | <pre>[<br>  "/dev/xvdb",<br>  "/dev/xvdc",<br>  "/dev/xvdd",<br>  "/dev/xvde",<br>  "/dev/xvdf",<br>  "/dev/xvdg",<br>  "/dev/xvdh",<br>  "/dev/xvdi",<br>  "/dev/xvdj",<br>  "/dev/xvdk",<br>  "/dev/xvdl",<br>  "/dev/xvdm",<br>  "/dev/xvdn",<br>  "/dev/xvdo",<br>  "/dev/xvdp",<br>  "/dev/xvdq",<br>  "/dev/xvdr",<br>  "/dev/xvds",<br>  "/dev/xvdt",<br>  "/dev/xvdu",<br>  "/dev/xvdv",<br>  "/dev/xvdw",<br>  "/dev/xvdx",<br>  "/dev/xvdy",<br>  "/dev/xvdz"<br>]</pre> | no |
| ebs\_iops | Amount of provisioned IOPS. This must be set with a volume\_type of io1. | `number` | `0` | no |
| ebs\_optimized | If true, the launched EC2 instance will be EBS-optimized. | `bool` | `false` | no |
| ebs\_volume\_enabled | Flag to control the ebs creation. | `bool` | `false` | no |
| ebs\_volume\_size | Size of the EBS volume in gigabytes. | `number` | `30` | no |
| ebs\_volume\_type | The type of EBS volume. Can be standard, gp2 or io1. | `string` | `"gp2"` | no |
| encrypted | If true, the disk will be encrypted. | `bool` | `false` | no |
| environment | Environment (e.g. `prod`, `dev`, `staging`). | `string` | `""` | no |
| ephemeral\_block\_device | Customize Ephemeral (also known as Instance Store) volumes on the instance. | `list` | `[]` | no |
| host\_id | The Id of a dedicated host that the instance will be assigned to. Use when an instance is to be launched on a specific dedicated host. | `string` | `null` | no |
| hostname | DNS records to create. | `string` | `""` | no |
| iam\_instance\_profile | The IAM Instance Profile to launch the instance with. Specified as the name of the Instance Profile. | `string` | `""` | no |
| instance\_count | Number of instances to launch. | `number` | `1` | no |
| instance\_enabled | Flag to control the instance creation. | `bool` | `true` | no |
| instance\_initiated\_shutdown\_behavior | Shutdown behavior for the instance. | `string` | `""` | no |
| instance\_profile\_enabled | Flag to control the instance profile creation. | `bool` | `false` | no |
| instance\_tags | Instance tags. | `map` | `{}` | no |
| instance\_type | The type of instance to start. Updates to this field will trigger a stop/start of the EC2 instance. | `string` | n/a | yes |
| ipv6\_address\_count | Number of IPv6 addresses to associate with the primary network interface. Amazon EC2 chooses the IPv6 addresses from the range of your subnet. | `number` | `0` | no |
| ipv6\_addresses | List of IPv6 addresses from the range of the subnet to associate with the primary network interface. | `list` | `[]` | no |
| key\_name | The key name to use for the instance. | `string` | `""` | no |
| kms\_key\_id | The ARN for the KMS encryption key. When specifying kms\_key\_id, encrypted needs to be set to true. | `string` | `""` | no |
| label\_order | Label order, e.g. `name`,`application`. | `list` | `[]` | no |
| managedby | ManagedBy, eg 'DevOps4Me' or 'NajibRadzuan'. | `string` | `"najibradzuan@devops4me.com"` | no |
| monitoring | If true, the launched EC2 instance will have detailed monitoring enabled. (Available since v0.6.0). | `bool` | `false` | no |
| name | Name  (e.g. `app` or `cluster`). | `string` | `""` | no |
| network\_interface | Customize network interfaces to be attached at instance boot time. | `list(map(string))` | `[]` | no |
| placement\_group | The Placement Group to start the instance in. | `string` | `""` | no |
| root\_block\_device | Customize details about the root block device of the instance. See Block Devices below for details. | `list` | `[]` | no |
| source\_dest\_check | Controls if traffic is routed to the instance when the destination address does not match the instance. Used for NAT or VPNs. | `bool` | `true` | no |
| subnet | VPC Subnet ID the instance is launched in. | `string` | `null` | no |
| subnet\_ids | A list of VPC Subnet IDs to launch in. | `list(string)` | `[]` | no |
| tags | Additional tags (e.g. map(`BusinessUnit`,`XYZ`). | `map` | `{}` | no |
| tenancy | The tenancy of the instance (if the instance is running in a VPC). An instance with a tenancy of dedicated runs on single-tenant hardware. The host tenancy is not supported for the import-instance command. | `string` | `""` | no |
| ttl | The TTL of the record to add to the DNS zone to complete certificate validation. | `string` | `"300"` | no |
| type | Type of DNS records to create. | `string` | `"CNAME"` | no |
| user\_data | The Base64-encoded user data to provide when launching the instances. | `string` | `""` | no |
| vpc\_security\_group\_ids\_list | A list of security group IDs to associate with. | `list(string)` | `[]` | no |

## Outputs

| Name | Description |
|------|-------------|
| arn | The ARN of the instance. |
| az | The availability zone of the instance. |
| instance\_count | The count of instances. |
| instance\_id | The instance ID. |
| ipv6\_addresses | A list of assigned IPv6 addresses. |
| key\_name | The key name of the instance. |
| placement\_group | The placement group of the instance. |
| private\_ip | Private IP of instance. |
| public\_ip | Public IP of instance (or EIP). |
| subnet\_id | The EC2 subnet ID. |
| vpc\_security\_group\_ids | The associated security groups in non-default VPC. |




## Testing
In this module testing is performed with [terratest](https://github.com/gruntwork-io/terratest) and it creates a small piece of infrastructure, matches the output like ARN, ID and Tags name etc and destroy infrastructure in your AWS account. This testing is written in GO, so you need a [GO environment](https://golang.org/doc/install) in your system. 

You need to run the following command in the testing folder:
```hcl
  go test -run Test
```
