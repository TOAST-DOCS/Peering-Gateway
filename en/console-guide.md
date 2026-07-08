<a id="network-peering-gateway-console-user-guide"></a>
## Network > Peering Gateway > Console User Guide { #network-peering-gateway-console-user-guide }

This guide describes how to use the Peering Gateway service from the console.

<a id="peering"></a>
## Peering { #peering }

**Peering** is a feature to connect two different **VPCs**. Normally, VPCs cannot communicate with each other because they are in different network zones. You can connect them using a **floating IP**, but it incurs extra charges depending on your network usage. However, the peering feature allows you to connect two **VPCs** at no additional cost.

* Peering connects two different VPCs. Connecting to another VPC across a VPC is not supported. For example, in the `A <-> B <-> C` connection, `A` and `C` cannot be connected.
* If you want to connect to a different VPC through a peer VPC, configure the **Route** settings provided from Peering so that the packets are delivered via VM instances.

    !!! tip "Note"
            For how to use Route, see "Common Feature > Route" below. 

* If the IP address ranges of the two VPCs overlap, they cannot be used.<br>
  Each IP address range must not be a subset of the other. Otherwise, peering creation will fail.
* In regions other than the Korea regions, communication is only possible with subnets that are connected to the **Default Routing Table**.

    !!! tip "Note"
        When you create a peering, a routing rule is implicitly added to the default routing table that forwards packets destined for the other VPC's IP address range to the peering gateway for routing peering communication. Therefore, you cannot set up another route in the default routing table with the other VPC's IP address range as the CIDR. Also, be aware if you add a route that specifies a portion of the other VPC's IP address range as the CIDR because it has a higher priority than routes intended for peering communication, which may prevent peering communication.

* In the Korea regions, separate routes must be configured in the routing tables of both peered VPCs to enable communication.
    * Add the route by entering the IP address range of the counterpart VPC in the route's **Target CIDR**, and selecting the **PEERING** entry with the name of the peering from the gateway list.
    * Communication is possible only with subnets associated with the routing table to which the route has been added.
    * For a routing table other than the default routing table, if the route is added to the routing table, peer communication becomes available on subnets associated with the routing table.
    * If you specify a VPC without a subnet when creating a peering, the peering creation fails.

<a id="create-a-peering"></a>
### Create a Peering { #create-a-peering }

1. Go to **Network > Peering Gateway > Peering**.
2. Click **Create Peering**.
3. Enter a **Name** and **Description**, and select **Local VPC**, **Peer VPC** to click **Confirm**.

<a id="change-a-peering"></a>
### Change a Peering { #change-a-peering }

1. Go to **Network > Peering Gateway > Peering**.
2. Select a peering to change from the peering list.
3. Click **Change Peering**.
4. Change the peering's **Name** or **Description** and click **Confirm**.

<a id="delete-a-peering"></a>
### Delete a Peering { #delete-a-peering }

1. Go to **Network > Peering Gateway > Peering**.
2. In the peering list, select the peering you want to delete.
3. Click **Delete Peering**.

<a id="other-important-notes-on-retrieving-peering-ports-using-the-api"></a>
### (Other) Important notes on retrieving peering ports using the API { #other-important-notes-on-retrieving-peering-ports-using-the-api }

When retrieving ports associated with peering using the API, the retrieval method differs depending on whether a **peer ID** exists in the peering resource.

!!! tip "Note"
    For information on how to check the peer ID, refer to "Reference > How to check the peer ID" at the bottom of this page.

* If no **peer ID** exists: Retrieve using only the peering ID, as before.
    * GET /v2.0/ports?device_id={peering ID}
* If a **peer ID** exists: You must provide both the peering ID and the peer ID to retrieve ports from both VPCs.
    * GET /v2.0/ports?device_id={peering ID}&device_id={peer ID}

<a id="region-peering"></a>
## Region Peering { #region-peering }

**Region peering** is a feature to connect two **VPCs** created in different regions. Peering can be used to connect VPCs in the same region, but it cannot be used to connect VPCs in different regions. However, region peering allows you to connect two VPCs in different regions.

* Region peering connects two VPCs in different regions. Connecting to another VPC across a VPC is not supported. For example, in the `A <-> B <-> C` connection, `A` and `C` cannot be connected.
* If you want to connect to a different VPC through a peer VPC, configure the **Route** settings provided from Peering so that the packets are delivered via VM instances.

    !!! tip "Note"
           For how to use Route, see "Common Feature > Route" below. 

* You can connect to the same project, different projects, or both.
* When you create a region peering, it is automatically created in the other connected region.
* When you delete a region peering, it is automatically deleted from the other connected region.
* If the IP address ranges of the two VPCs overlap, they cannot be used.
* You cannot create duplicate VPC connections.
* Communication becomes available after configuring additional **routes** in the **routing tables** of both peered VPCs.
    * Add the route by entering the IP address range of the counterpart VPC in the route's **Target CIDR**, and selecting the **INTER_REGION_PEERING** entry with the name of the region peering from the gateway list.
    * Communication is possible only with subnets associated with the routing table to which the route has been added.
    * For a routing table other than the default routing table, if the route is added to the routing table, peer communication becomes available on subnets associated with the routing table.
    * If you specify a VPC without a subnet when creating a region peering, the region peering creation fails.

<a id="create-a-region-peering"></a>
### Create a Region Peering { #create-a-region-peering }

!!! tip "Note"
    To create a region peering between different projects, your project's tenant ID and VPC ID must be allowed in the peer project's peering allowed targets. This is not required when creating a region peering within the same project.
    Before you can create a region peering between different projects, you must send your tenant ID and VPC ID to the administrator of the peer project and request to register the information with the peering allowed targets.
    Once the peer project has registered the information, you can create a project peering according to the following steps.
    For how to use the peering allowed targets, see the "Common Feature > Peering Allowed List" .

1. Go to **Network > Peering Gateway > Region Peering**.
2. Click **Create Region Peering**.
3. Select a **name**, **local VPC**, and **peer region**.
4. Select **a peer tenant**.
    * There is no additional input required when selecting the **same tenant**.
    * When you select a **different tenant**, you must enter the **peer tenant ID**.

    !!! tip "Note"
         For the peer tenant ID, see the "References" section below.

5. Enter  a **peer VPC ID**.

    !!! tip "Note"
        For the peer tenant ID, see the "References" section below.

<a id="delete-a-region-peering"></a>
### Delete a Region Peering { #delete-a-region-peering }

1. Go to **Network > Peering Gateway > Region Peering**.
2. In the peering list, select the region peering you want to delete.
3. Click **Delete Region Peering**.

<a id="project-peering"></a>
## Project Peering { #project-peering }

**Project peering** is a feature to connect two **VPCs** created in different projects. Peering can be used to connect VPCs in the same project, but it cannot be used to connect VPCs in different project. However, the project peering feature allows you to connect two VPCs in different projects.

* Project peering connects two VPCs in different projects. Connecting to another VPC across a VPC is not supported. For example, in the `A <-> B <-> C` connection, `A` and `C` cannot be connected.
* If you want to connect to a different VPC through a peer VPC, configure the **Route** settings provided from Peering so that the packets are delivered via VM instances.

    !!! tip "Note"
        For how to use route, see "Common Feature > Route" below.
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

<a id="create-a-project-peering"></a>
### Create a Project Peering { #create-a-project-peering }

!!! tip "Note"
    To create a project peering, your project's tenant ID and VPC ID must be allowed in the peering allowed targets of the peer project.
    Before you can create a project peering, you must send your tenant ID and VPC ID to the administrator of the peer project and request to register the information with the peering allowed targets.
    Once the peer project has registered the information, you can create a project peering according to the following steps.
    For how to use the peering allowed targets, see the "Common Feature > Manage Peering Allowed Targets" . 

1. Go to **Network > Peering Gateway > Project Peering**.
2. Click **Create Project Peering**.
3. Enter **Name**, **Local VPC**, **Peer Region**, **Peer Tenant ID**, and **Peer VPC ID**.
    !!! tip "Note"
    See the "Other Considerations" section below for information on how to determine the peer tenant ID and peer VPC ID.

<a id="delete-a-project-peering"></a>
### Delete a Project Peering { #delete-a-project-peering }

1. Go to **Network > Peering Gateway > Project Peering**.
2. In the peering list, select the project peering you want to delete.
3. Click **Delete Project Peering**.

<a id="common-feature"></a>
## Common Feature { #common-feature }

Describes the common feature provided by peering (peering, region peering, and project peering).

<a id="manage-peering-allowed-targets"></a>
### Manage Peering Allowed Targets { #manage-peering-allowed-targets }

The Region Peering, Project Peering page submenu allows you to set up a peering connection request between different projects on the receiving end. Enter the peer tenant ID of the VPC sending the request and the peer VPC ID to add it to the peering allowed VPCs and allow the peer to accept the request.

<a id="manage-peering-allowed-targets-add-an-peering-allowed-target"></a>
#### Add an Peering Allowed Target

1. Go **to****Network >** **Peering****Gateway > Region Peering** or **Network > Peering Gateway > Project Peering**.
2. Click **Manage Peering Allowed Targets**.
3. Click **Add Peering Allowed VPC**.
4. Enter **Name**, **Peer Tenant ID** and **Peer VPC ID** and click Confirm.

    !!! tip "Note"
        To find out the peer tenant ID and peer VPC ID, see the References section below.

<a id="manage-peering-allowed-targets-delete-a-peering-allowed-target"></a>
#### Delete a Peering Allowed Target

1. Go **to****Network >** **Peering****Gateway > Region Peering** or **Network > Peering Gateway > Project Peering**.
2. Click **Manage Peering Allowed Targets**.
3. In the Peering Allowed VPCs, click Delete for the target you want to delete.

<a id="route"></a>
### Route { #route }

The **Route** settings provided from Peering allows you to make a configuration where traffic is delivered to a different VPC via VM instances of a peer VPC. The peering's route allows you to specify and configure a VM instance's port and virtual IP port that processes all incoming traffic from the peering. By deploying Network Virtual Appliance VM in a VM instance that serves as the route's gateway, you can control traffic in the VM instance and deliver it to a different peering. 
* If you want to configure a Hub and Spoke VPC connection through peering and control all traffic with Network Virtual Appliance located in the Hub VPC, you can use the routing feature of Peering.

<a id="route-create-route"></a>
#### Create Route

1. Select a peering to configure Route
2. Select Route at the bottom tab
3. Click **Change Route**

    !!! tip "Note"
        For peering, there are two buttons. Change Peer Route means adding a route to the Peer VPC selected when creating a peering, and Change Local Route means the location for Local VPC.

4. Click the **+** button.
5. Enter the target CIDR.
6. Select a gateway.

    !!! tip "Note"
        For gateway, only instances and virtual IPs can be seleted.

7. Click the **Confirm** button.

<a id="route-delete-route"></a>
#### Delete Route

1. Select a peering for which you want to delete the route settings.
2. Select a route at the bottom tab.
3. Click the **Change Route** button.

    !!! tip "Note"
        For peering, there are two buttons. Change Peer Route means adding a route to the Peer VPC selected when creating a peering, and Change Local Route means the location for Local VPC. 
4. Click the **-** button for the route to delete.
5. Click the **Confirm** button.

<a id="other-considerations"></a>
## Other Considerations { #other-considerations }

<a id="how-to-check-peer-vpc-id"></a>
### How to check peer VPC ID { #how-to-check-peer-vpc-id }

Check the **VPC ID** according to the following steps.

!!! tip "Note"
        If you don't have access to the project that the peer VPC belongs to, ask the administrator of the peer project to provide you with the VPC ID.

1. Access the console screen of the peer project.
2. Go to **Network > VPC > Management**.
3. Choose the peering target VPC.
4. Copy the UUID value shown in **Basic Information > VPC Name**.

<a id="how-to-check-the-peer-tenant-id"></a>
### How to check the peer tenant ID { #how-to-check-the-peer-tenant-id }

Check the **Tenant ID** according to the following steps.

!!! tip "Note"
        If you do not have access to the peer project, obtain a tenant ID from the administrator of the peer project.

1. Access the console screen of the peer project.
2. Go to **Network > VPC > Management**.
3. Select one of the peering targets or any of the VPCs shown on the screen.
4. Copy the ID value shown in **Basic Information > Tenant ID.

<a id="how-to-check-the-peer-id"></a>
### How to check the peer ID { #how-to-check-the-peer-id }

Follow the steps below to check the peer ID of a peering.

!!! tip "Note"
    For peerings without a peer ID, ports can be retrieved using the existing method.

1. Go to **Network > Peering Gateway > Peering**.
2. Select the target peering.
3. Copy the value displayed in **Basic Information > Peer ID**.