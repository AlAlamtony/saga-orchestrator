Overview of the code 
This code implements a saga for a multi-step order processing system, where an order is created in an e-commerce system, payment is processed through a payment system (like Stripe), accounting records are created, and inventory is updated in a warehouse system. The orchestrator ensures that if any step fails, a rollback mechanism is triggered to undo the completed steps.

The main components of the code are:

E-Commerce System: Manages the user orders.
Payment System: Handles the payment for the order (e.g., Stripe).
Accounting System: Creates accounting records.
Warehouse System: Updates the inventory based on the order.
The orchestrator performs these operations in sequence. If any step fails, the orchestrator calls a rollback function to reverse the previous successful actions in the reverse order.
Here's a breakdown of the key components:

Service URLs:

The class interacts with four external services via HTTP: e-commerce, payment, accounting, and warehouse. These services have specific API endpoints for order creation, payment processing, record keeping, and inventory management.
Orchestrating the Workflow:

The primary method, createUserOrder, coordinates the entire order process. It initiates the following sequence:
Create an order in the e-commerce system.
Create a payment in the payment system (e.g., Stripe).
Create an accounting record in the accounting system.
Update the inventory in the warehouse system.
HTTP Requests:

The makeHttpRequest method sends HTTP requests (usually POST to create resources and DELETE to cancel them) to each service. It ensures that data is transmitted in JSON format. It also handles HTTP responses, checking for success codes (200 or 201), and throwing an exception if the request fails.
Rollback Mechanism:

If any step fails during the process, the orchestrator triggers a rollback by calling the cancelAction method. This sends a DELETE request to the corresponding service to undo any previous changes (like cancelling the payment or deleting the order).
Data Handling:

The mapToJson method converts request data from a map structure into a JSON string, which is necessary for HTTP communication. The parseResponse method processes the responses from the services, extracting necessary details such as resource IDs.
In summary, this class models a distributed transaction process, ensuring that all services are successfully coordinated or, in case of failure, rolled back to a consistent state. The code relies on HTTP communication for service interaction, JSON data format, and basic error handling to ensure that the entire workflow is reliable and consistent.
