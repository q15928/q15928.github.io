---
layout:     post
title:      "Terraform At a Glance"
date:       2019-07-07 12:00:00
author:     "Jason Feng"
header-img: "img/railway-track-2049394_960_720.jpg"
excerpt_separator: <!--more-->
tags:
    - Terraform
    - Configuration
    - Infrastructure as Code
---
This is an excerption from **qwiklabs**. It is a quick introduction of Terraform which is an open source Infrastructure as Code tool to create, change and version the infrastructure safely and efficiently.

<!--more-->
### Quick Facts
Company: *[HashiCorp](https://www.hashicorp.com/)*

Configuration: *HCL (HashiCorp Configuration Language)*

Documentation: *[Terraform Docs](https://www.terraform.io/docs/)*

Implementation Language: *golang*

### Configuration Overview
##### Many Files, One Configuration
Terraform allows you to split your configuration into as many files as you wish. Any Terraform file in the current working directory will be loaded and concatenated with the others when you tell Terraform to apply your desired configuration.

##### Local State
Terraform saves the things it has done to a local file, referred to as a "state file". Because state is saved locally, that means that sometimes the local state will differ from what's actually configured on the firewall.

This is actually not a big deal, as many of Terraform's commands do a Read operation to check the actual state against what's saved locally. Any changes that are found are then saved to the local state automatically.

### Example Terraform configuration
Here's an example of a Terraform configuration file. Parts of this config are discussed below.

```terraform
variable "hostname" {
    default = "127.0.0.1"
}

variable "username" {
    default = "admin"
}

variable "password" {
    default = "admin"
}

provider "panos" {
    hostname = "${var.hostname}"
    username = "${var.username}"
    password = "${var.password}"
}

resource "panos_management_profile" "ssh" {
    name = "allow ssh"
    ssh = true
}

resource "panos_ethernet_interface" "eth1" {
    name = "ethernet1/1"
    vsys = "vsys1"
    mode = "layer3"
    enable_dhcp = true
    management_profile = "${panos_management_profile.ssh.name}"
}

resource "panos_zone" "zone1" {
    name = "L3-in"
    mode = "layer3"
    interfaces = ["ethernet1/1"]
    depends_on = ["panos_ethernet_interface.eth1"]
}
```

### Terminology
##### Plan
A Terraform plan is the sum of all Terraform configuration files in a given directory. These files are generally written in HCL.

##### Provider
A provider can be loosely thought of to be a product (such as the Palo Alto Networks firewall) or a service (such as GCP, Azure, or AWS). The provider understands the underlying API to the product or service, making individual parts of those things available as resources.

Most providers require some kind of configuration in order to use. For the panos provider, this is the authentication credentials of the firewall or Panorama that you want to configure.

Providers are configured in a provider configuration block (e.g. - provider "panos" {...} and a plan can make use of any number of providers, all working together.

##### Resource
A resource is an individual component that a provider supports create/read/update/delete operations.

For the Palo Alto Networks firewall, this would be something like an ethernet interface, service object, or an interface management profile.

##### Data Source
A data source is like a resource, but read-only.

For example, the panos provider has a data source that gives you access to the results of show system info.

##### Attribute
An attribute is a single parameter that exists in either a resource or a data source. Individual attributes are specific to the resource itself, as to what type it is, if it's required or optional, has a default value, or if changing it would require the whole resource to be recreated or not.

Attributes can have a few different types:

String: *"foo", "bar"*
Number: *7, "42" (quoting numbers is fine in HCL)*
List: *["item1", "item2"]*
Boolean: *true, false*
Map: *{"key": "value"}*
Note: some maps may have more complex values

##### Variables
Terraform plans can have variables to allow for more flexibility. These variables come in two flavors: user variables and attribute variables.

Whenever you want to use variables (or any other Terraform interpolation), you'll be enclosing it in curly braces with a leading dollar sign: "${...}"

User variables are variables that are defined in the Terraform plan file with the variable keyword. These can be any of the types of values that attributes can be (default is string) and can also be configured to have default values. When using a user variable in your plan files, they are referenced with var as a prefix: "${var.hostname}". Terraform looks for local variable values in the file terraform.tfvars.

Attribute variables are variables that reference other resources or data sources within the same plan. Specifying a resource attribute using an attribute variable creates an implicit dependency, covered below.

##### Dependencies
There are two ways to tell Terraform that resource "A" needs to be created before resource "B": the universal depends_on resource parameter or an attribute variable. The first way, using depends_on, is performed by adding the universal parameter "depends_on" within the dependent resource. The second way, using attribute variables, is performed by referencing a resource or data source attribute as a variable: "${panos_management_profile.ssh.name}"

##### Common Commands
The Terraform binary has many different CLI arguments that it supports. We'll discuss only a few of them here:

`terraform init` initializes the current directory based on the local plan files, downloading any missing provider binaries.

`terraform plan` refreshes provider/resource states and reports what changes need to take place.

`terraform apply` refreshes provider/resource states and makes any needed changes to the resources.

`terraform destroy` refreshes provider/resource states and removes all resources that Terraform created.

*Note:* This is originated from [qwiklabs](https://google.qwiklabs.com/quests/62).