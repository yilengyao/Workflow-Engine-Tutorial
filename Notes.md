# Important Files of Temporal

Which holds all the details about your Temporal Server and worker etc

Workflow (Interface): Workflow starting point and other signal methods annotated by @WorkflowInterface

Workflow IMPL (class): Implements workflow and hold declaration of workflow methods

Activity (Interface): Holds the activity process to be defined

Activity IMPL (class): Implements Activity and holds business logic for activity





At a high level, Temporal consists of 2 distinct components - Temporal Server and Teporal Client components (running as part of your application deployment). The client applications communicate to Temporal Server using temporal SDK

The Temporal server primary consists of a web console, to visualize and troubleshoot workflow execution, a bunch of services, and an event-source database.

On the client application side, the Main Temproal components are - Workflow, Activities, and Workers.

# Temporal Workflow
Only a single workflow method in Workflow interface annotated by @WorkflowMethod, you can have multiple signal methods(@SignalMethod)

In Temporal, we define workflow in code (also called Workflow Definition), Temporal Workflow is an instance of workflow definition (executed by a Worker)

When a client application submits the Workflow to the Temporal Server, it becomes the responsibility of the Temporal Server to execute the Workflow and all Activities associated with the Workflow in the appropriate Worker.

Example of a Workflow defined in Java
```
public void createOrder(OrderDTO orderDTO) {
  paymentActivity.debitPayment(orderDTO);
  reserveInventoryActivity.reserveInventory(orderDto);
  shipGoodsAcivity.shipGoods(orderDTO);
  orderActivity.completeOrder(orderDTO);
}
```

The above Workflow definition consists of 4 distinct steps (Activities).

# Temporal Activities
An Activity is a funciton that execuetes a single, well-defined action (either short or long-running), such as calling another service, transcoding a media file, or transforming data.
You can think of an Activity as a distinct step (business action) in a workflow execution.

In the above workflow definition, there are 4 different Activities - Debit Payment, Reserve Inventory, Goods Shipped and "Local Activity" completeOrder.

A Local Activity is an Activity that executes in the same process as the Workflow that spawns it. You should use Local Activities for very short-living tasks that do not need flow control, rate limiting, and routing capabilities.

# Temporal Workers
A Temporal worker is responsible for executing Workflowws and Activities. A Worker connects to the Temporal server via client SDK and polls the server for the workflow or activities tasks. You can register Workflows and ACtivities with the Worker using APIs provided by Client SDK
