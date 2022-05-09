## Network > Peering Gateway > Overview

The Peering Gateway service allows you to connect two different virtual private networks. By connecting two different **VPCs**, you can use a private IP assigned to a **VPC** to access resources in the counterpart **VPC** directly.

### Main Features

The peering types provided by the Peering Gateway service are as follows:

* Peering
    * This is a feature to connect two different VPCs in the same region and in the same project.
    * You can communicate with a connected peer VPC by adding a route for the network range of the connected peer VPC.
* Region Peering
    * This is a feature to connect two different VPCs that are in different regions but in the same project.
    * You can communicate with a connected peer VPC by adding a route for the network range of the connected peer VPC in another region.
* Project Peering
    * This is a feature to connect two different VPCs that are in the same region but in different projects.
    * You can communicate with a connected peer VPC by adding a route for the network range of the connected peer VPC in another project.
