# **Fulfillment Application Design Document**

Order fulfillment is 3 step process,
*   Step 1: The staff at the fulfillment center starts the fulfillment process with a [batch of orders](createOrderFulfillmentWave.md). 
*   Step 2: Shipments are created for orders. Based on the inventory allocation rules inventory from a certain location is assigned to Shipment. The shipment data is then used to prepare a picklist based on preferred picking strategy. 
*   Step 3: Once shipment is created, an optionally background process is initiated to get shipping labels from the shipping provider.




```
<StatusType description="Shipment" hasTable="N"  statusTypeId="SHIPMENT_STATUS"/>
        <!-- Shipment status -->
    <StatusItem description="Created" sequenceId="01" statusCode="INPUT" statusId="SHIPMENT_INPUT" statusTypeId="SHIPMENT_STATUS"/>
    <StatusItem description="Approved" sequenceId="08" statusCode="APPROVED" statusId="SHIPMENT_APPROVED" statusTypeId="SHIPMENT_STATUS"/>

    <!-- Shipment status -->
    <StatusItem description="Scheduled" sequenceId="02" statusCode="SCHEDULED" statusId="SHIPMENT_SCHEDULED" statusTypeId="SHIPMENT_STATUS"/>
    <StatusItem description="Picked" sequenceId="03" statusCode="PICKED" statusId="SHIPMENT_PICKED" statusTypeId="SHIPMENT_STATUS"/>
    <StatusItem description="Packed" sequenceId="04" statusCode="PACKED" statusId="SHIPMENT_PACKED" statusTypeId="SHIPMENT_STATUS"/>
    <StatusItem description="Shipped" sequenceId="05" statusCode="SHIPPED" statusId="SHIPMENT_SHIPPED" statusTypeId="SHIPMENT_STATUS"/>
    <StatusItem description="Delivered" sequenceId="06" statusCode="DELIVERED" statusId="SHIPMENT_DELIVERED" statusTypeId="SHIPMENT_STATUS"/>
    <StatusItem description="Cancelled" sequenceId="99" statusCode="CANCELLED" statusId="SHIPMENT_CANCELLED" statusTypeId="SHIPMENT_STATUS"/>

    <StatusValidChange statusId="SHIPMENT_APPROVED" statusIdTo="SHIPMENT_PACKED" transitionName="Pack"/>
    <StatusValidChange statusId="SHIPMENT_APPROVED" statusIdTo="SHIPMENT_INPUT" transitionName="Create" conditionExpression="directStatusChange == false"/>
    <StatusValidChange statusId="SHIPMENT_PACKED" statusIdTo="SHIPMENT_CANCELLED" transitionName="Cancel"/>
    <!-- Shipment SHIPMENT_STATUS status valid change data -->
    <StatusValidChange statusId='SHIPMENT_APPROVED' statusIdTo='SHIPMENT_PACKED' transitionName='Pack' sequenceNum='01' />
    <StatusValidChange statusId='SHIPMENT_APPROVED' statusIdTo='SHIPMENT_CANCELLED' transitionName='Cancel' sequenceNum='03' />
    <StatusValidChange statusId='SHIPMENT_APPROVED' statusIdTo='SHIPMENT_SHIPPED' transitionName='Ship' sequenceNum='02' />
    <StatusValidChange statusId='SHIPMENT_INPUT' statusIdTo='SHIPMENT_SCHEDULED' transitionName='Schedule' sequenceNum='01' conditionExpression="directStatusChange == false" />
    <StatusValidChange statusId='SHIPMENT_INPUT' statusIdTo='SHIPMENT_PICKED' transitionName='Pick' sequenceNum='02' conditionExpression="directStatusChange == false" />
    <StatusValidChange statusId='SHIPMENT_INPUT' statusIdTo='SHIPMENT_CANCELLED' transitionName='Cancel' sequenceNum='04' />
    <StatusValidChange statusId='SHIPMENT_INPUT' statusIdTo='SHIPMENT_PACKED' transitionName='Pack' sequenceNum='03' conditionExpression="directStatusChange == false" />
    <StatusValidChange statusId="SHIPMENT_INPUT" statusIdTo="SHIPMENT_APPROVED" transitionName="Approve" sequenceNum="05" conditionExpression="directStatusChange == false" />
    <StatusValidChange statusId='SHIPMENT_SCHEDULED' statusIdTo='SHIPMENT_PICKED' transitionName='Pick' sequenceNum='01' />
    <StatusValidChange statusId='SHIPMENT_SCHEDULED' statusIdTo='SHIPMENT_CANCELLED' transitionName='Cancel' sequenceNum='03' />
    <StatusValidChange statusId='SHIPMENT_SCHEDULED' statusIdTo='SHIPMENT_PACKED' transitionName='Pack' sequenceNum='02' />
    <StatusValidChange statusId='SHIPMENT_PACKED' statusIdTo='SHIPMENT_INPUT' transitionName='input' sequenceNum='02' conditionExpression="directStatusChange == false" />
    <StatusValidChange statusId='SHIPMENT_PACKED' statusIdTo='SHIPMENT_SHIPPED' transitionName='Ship' sequenceNum='01' />
    <StatusValidChange statusId='SHIPMENT_PACKED' statusIdTo='SHIPMENT_CANCELLED' transitionName='Cancel' sequenceNum='03' />
    <StatusValidChange statusId="SHIPMENT_PACKED" statusIdTo="SHIPMENT_APPROVED" transitionName="Approve" sequenceNum="04" conditionExpression="directStatusChange == false" />
    <StatusValidChange statusId='SHIPMENT_PICKED' statusIdTo='SHIPMENT_CANCELLED' transitionName='Cancel' sequenceNum='02' />
    <StatusValidChange statusId='SHIPMENT_PICKED' statusIdTo='SHIPMENT_PACKED' transitionName='Pack' sequenceNum='01' />
    <StatusValidChange statusId='SHIPMENT_SHIPPED' statusIdTo='SHIPMENT_DELIVERED' transitionName='Deliver' sequenceNum='01' />
```
