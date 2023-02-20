## Network > Peering Gateway > Console User Guide

This guide describes how to use the Peering Gateway service from the console.

## Peering

**Peering** is a feature to connect two different **VPCs**. Normally, VPCs cannot communicate with each other because they are in different network zones. You can connect them using a **floating IP**, but it incurs extra charges depending on your network usage. However, the peering feature allows you to connect two **VPCs** at no additional cost.

* Peering connects two different VPCs. Connecting to another VPC across a VPC is not supported. For example, in the `A <-> B <-> C` connection, `A` and `C` cannot be connected.
* If you want to connect to a different VPC through a peer VPC, configure the **Route** settings provided from Peering so that the packets are delivered via VM instances.
  > [Note] For how to use Route, see "Common Feature > Route" below. 
* If the IP address ranges of the two VPCs overlap, they cannot be used.<br>
  Each IP address range must not be a subset of the other. Otherwise, peering creation will fail.
* In regions other than the Korea regions, communication with subnets not associated with the **Default Routing Table** is not possible.
    * In the Korea regions, separate routes must be configured in the routing tables of both peered VPCs to enable communication.
        * Add the route by entering the IP address range of the counterpart VPC in the route's **Target CIDR**, and selecting the **PEERING** entry with the name of the peering from the gateway list.
        * Communication is possible only with subnets associated with the routing table to which the route has been added.
        * For a routing table other than the default routing table, if the route is added to the routing table, peer communication becomes available on subnets associated with the routing table.
        * If you specify a VPC without a subnet when creating a peering, the peering creation fails.

## Region Peering

**Region peering** is a feature to connect two **VPCs** created in different regions. Peering can be used to connect VPCs in the same region, but it cannot be used to connect VPCs in different regions. However, region peering allows you to connect two VPCs in different regions.

* Region peering connects two VPCs in different regions. Connecting to another VPC across a VPC is not supported. For example, in the `A <-> B <-> C` connection, `A` and `C` cannot be connected.
* If you want to connect to a different VPC through a peer VPC, configure the **Route** settings provided from Peering so that the packets are delivered via VM instances.
  > [Note] For how to use Route, see "Common Feature > Route" below. 
* Region peering is only available for VPCs in the Korea (Pyeongchon) Region and the Korea (Pangyo) Region.
* Only two VPCs of the same account and the same project can be connected.
* When you create a region peering, it is automatically created in the other connected region.
* When you delete a region peering, it is automatically deleted from the other connected region.
* If the IP address ranges of the two VPCs overlap, they cannot be used.
* You cannot create duplicate VPC connections.
* Communication becomes available after configuring additional **routes** in the **routing tables** of both peered VPCs.
    * Add the route by entering the IP address range of the counterpart VPC in the route's **Target CIDR**, and selecting the **INTER_REGION_PEERING** entry with the name of the region peering from the gateway list.
    * Communication is possible only with subnets associated with the routing table to which the route has been added.
    * For a routing table other than the default routing table, if the route is added to the routing table, peer communication becomes available on subnets associated with the routing table.
    * If you specify a VPC without a subnet when creating a region peering, the region peering creation fails.

### Create a Region Peering

1. Go to **Network > Peering Gateway > Region Peering**.
2. Click **Create Region Peering**.
3. Enter **Name**, **Local VPC**, **Peer Region**, and **Peer VPC ID**.</br>
   > [Note] Peer VPC ID<br>
   > For how to check the **Peer VPC ID**, see "Check the VPC ID of the Peer Region" below. 

### Check the VPC ID of the Peer Region

To create a region peering, you need to know the **VPC ID** of the VPC that becomes a target of peering from another region. To check the VPC ID of the peer region, use the following steps:

1. Access the console screen of the peer region.
2. Go to **Network > VPC > Management**.
3. Choose the peering target VPC.
4. Copy the UUID value shown in **Basic Information > VPC Name**.

## Project Peering

**Project peering** is a feature to connect two **VPCs** created in different projects. Peering can be used to connect VPCs in the same project, but it cannot be used to connect VPCs in different project. However, the project peering feature allows you to connect two VPCs in different projects.

* Project peering connects two VPCs in different projects. Connecting to another VPC across a VPC is not supported. For example, in the `A <-> B <-> C` connection, `A` and `C` cannot be connected.
* If you want to connect to a different VPC through a peer VPC, configure the **Route** settings provided from Peering so that the packets are delivered via VM instances.
  > [Note] For how to use Route, see "Common Feature > Route" below.  
* Project peering is only available for VPCs in the Korea (Pyeongchon) Region and the Korea (Pangyo) Region.
* Only two VPCs in different projects in the same region can be connected.
* When you create a project peering, it is automatically created in the other connected project.
* When you delete a project peering, it is automatically deleted from the other connected project.
* If the IP address ranges of the two VPCs overlap, they cannot be used.
* You cannot create duplicate VPC connections.
* Communication becomes available after configuring additional **routes** in the **routing tables** of both peered VPCs.
    * Add the route by entering the IP address range of the counterpart VPC in the route's **Target CIDR**, and selecting the **INTER_PROJECT_PEERING** entry with the name of the project peering from the gateway list.
    * Communication is possible only with subnets associated with the routing table to which the route has been added.
    * For a routing table other than the default routing table, if the route is added to the routing table, peer communication becomes available on subnets associated with the routing table.
    * If you specify a VPC without a subnet when creating a project peering, the project peering creation fails.

### Create a Project Peering

To create a **project peering**, your project's **tenant ID** and **VPC ID** must be allowed in **Allow Project Peering** of the peer project.
Before creating a project peering, you need to pass the **tenant ID** and **VPC ID** to the administrator of the peer project and ask them to register the information in **Allow Project Peering**.
After registration of information in the peer project is completed, you can create a project peering by the following steps.

> [Note] For how to allow project peering, see "Allow Project Peering" below. 

1. Go to **Network > Peering Gateway > Project Peering**.
2. Click **Create Project Peering**.
3. Enter **Name**, **Local VPC**, **Peer Region**, **Peer Tenant ID**, and **Peer VPC ID**.</br>
   > [Note] Peer Tenant ID, Peer VPC ID <br>
   > For information on how to check the peer tenant ID and peer VPC ID, see "Check the Peer Tenant ID of a Peer Project" below.

### Allow Project Peering

The configuration needs to be performed in the project that receives the **project peering** request. Enter the **tenant ID** and **VPC ID** of the counterpart sending the request so that the project peering request sent by the peer can be accepted.

1. Go to **Network > Peering Gateway > Project Peering**.
2. Click **Allow Project Peering**.
3. Enter **Name**, **Peer Tenant ID** and **Peer VPC** and click the Confirm button.

### Check the VPC ID of the Peer Project

To create a **project peering**, you need to know the **VPC ID** of the VPC that becomes a target of peering from the peer project. You can check the **VPC ID** of the peer project with the following steps.

> [Note] If you do not have access to the peer project, obtain a VPC ID from the administrator of the peer project.

1. Access the peer project's console screen.
2. Go to **Network > VPC > Management**.
3. Choose a peering target VPC.
4. Copy the UUID value shown in **Basic Information > VPC Name**.

### Check the Tenant ID of the Peer Project

To create a **project peering**, you need to know the **tenant ID** of the peer project. You can check the **tenant ID** of the peer project with the following steps.

> [Note] If you do not have access to the peer project, obtain a tenant ID from the administrator of the peer project.

1. Access the peer project's console screen.
2. Go to **Network > VPC > Management**.
3. Select the peering target or any entry among VPCs displayed on the screen.
4. Copy the ID value shown in **Basic Information > Tenant ID**.

## Common Feature

This section describes the common feature provided from Peering (Peering, Region peering, and Project peering). 

### Route

The **Route** settings provided from Peering allows you to make a configuration where traffic is delivered to a different VPC via VM instances of a peer VPC. The peering's route allows you to specify and configure a VM instance's port and virtual IP port that processes all incoming traffic from the peering. By deploying Network Virtual Appliance VM in a VM instance that serves as the route's gateway, you can control traffic in the VM instance and deliver it to a different peering. 
* If you want to configure a Hub and Spoke VPC connection through peering and control all traffic with Network Virtual Appliance located in the Hub VPC, you can use the routing feature of Peering.

> [Note] This feature is only available for VPCs in the Korea (Pyeongchon) Region and the Korea (Pangyo) Region.

#### Create Route

1. Select a peering to configure Route
2. Select Route at the bottom tab
3. Select **Change Route** 
   > [Note] For peering, there are two buttons. **Change Peer Route** means adding a route to the **Peer VPC** selected when creating a peering, and **Change Local Route** means the location for **Local VPC**.<br> 
4. Click the **+** button.
5. Enter the target CIDR.
6. Select a gateway.
    > [Note] For gateway, only instances and virtual IPs can be seleted.<br>
7. Click the **Confirm** button.

4. For Peering, select **Gateway Category** (Project peering and Region peering do not exist.)
   > [Note] **Peer Gateway** is to add a route to the **Peer VPC** that you selected when creating the peering, and **Local Gateway** is the **Local VPC** location.<br>
5. Click Confirm after selecting the gateway.
   > [Note] Gateway can only select instances and virtual IPs.<br>

#### Delete Route

1. Select a peering for which you want to delete the route settings.
2. Select a route at the bottom tab.
3. Click the **Change Route** button.
    > [Note] For peering, there are two buttons. **Change Peer Route** means adding a route to the **Peer VPC** selected when creating a peering, and **Change Local Route** means the location for **Local VPC**.<br> 
4. Click the **-** button for the route to delete.
5. Click the **Confirm** button.
