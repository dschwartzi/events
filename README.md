# Event-Driven System with EventBridge and Lambda

This repository contains the code for an event-driven system built using AWS EventBridge and AWS Lambda. The system allows you to define, store, and manage events, flows, and routing configurations for processing and transforming data.

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Getting Started](#getting-started)
- [Event Store](#event-store)
- [Event Store Wrapper](#event-store-wrapper)
- [Creating Event Store Mappings](#creating-event-store-mappings)
- [API Endpoints](#api-endpoints)
- [Contributing](#contributing)
- [License](#license)

## Overview

The event-driven system provides a scalable and flexible way to process and transform data based on predefined events, flows, and routing configurations. It leverages AWS EventBridge for event management and AWS Lambda for executing the processing logic.

## Architecture

The system architecture consists of the following components:

- **Event Store**: A DynamoDB table that stores event schemas, flows, and routing configurations.
- **Event Store Wrapper**: A Lambda function that provides an HTTP interface for interacting with the Event Store.
- **Router**: A Lambda function that receives events from EventBridge and routes them to the appropriate processing logic based on the routing configurations.
- **Processing Functions**: Lambda functions that perform specific data processing tasks based on the received events.

## Getting Started

To get started with the event-driven system, follow these steps:

1. Clone the repository:

   ```
   git clone https://github.com/your-username/event-driven-system.git
   ```

2. Set up the required AWS resources:
   - Create a DynamoDB table for the Event Store.
   - Create an EventBridge event bus.
   - Set up the necessary IAM permissions for the Lambda functions.

3. Configure the environment variables in the `template.yaml` file:
   - Set the `EVENT_STORE_TABLE_NAME` to the name of your DynamoDB table.
   - Set the `EVENT_BUS_ARN` to the ARN of your EventBridge event bus.

4. Deploy the system using AWS SAM:

   ```
   sam build
   sam deploy --guided
   ```

5. Create event store mappings by running the `create_event_store_mappings.py` script:

   ```
   python create_event_store_mappings.py
   ```

6. Start sending events to the system and observe the processing results.

## Event Store

The Event Store is a DynamoDB table that stores event schemas, flows, and routing configurations. It uses the following structure:

- **PK** (Partition Key): Indicates the type of item stored (`events`, `flow`).
- **SK** (Sort Key): Represents the unique identifier of the item (event name, flow ID).
- **Schema**: Contains the JSON schema of the event.
- **RoutingConfig**: Contains the routing configuration for the event.
- **Data**: Contains the flow data.

## Event Store Wrapper

The Event Store Wrapper is a Lambda function that provides an HTTP interface for interacting with the Event Store. It exposes the following endpoints:

- `GET /events`: Retrieves all registered events.
- `GET /event/{event-id}`: Retrieves details of a specific event.
- `GET /flows`: Retrieves all flows.
- `GET /flow/{flow-id}`: Retrieves details of a specific flow.
- `POST /flow`: Creates a new flow.
- `POST /route/{event-id}`: Updates the routing configuration for an event.
- `DELETE /flow/{flow-id}`: Deletes a specific flow.

## Creating Event Store Mappings

The `create_event_store_mappings.py` script is used to define event schemas and routing configurations and store them in the Event Store. It provides a convenient way to register events and set up their routing logic.

To create event store mappings, update the `event_schemas` and `routing_configs` dictionaries in the script with your desired event schemas and routing configurations, respectively. Then, run the script to populate the Event Store.

## API Endpoints

The system exposes the following API endpoints through the Event Store Wrapper Lambda function:

- `GET /events`
  - Description: Retrieves all registered events.
  - Response: Array of event objects.

- `GET /event/{event-id}`
  - Description: Retrieves details of a specific event.
  - Path Parameters:
    - `event-id`: The unique identifier of the event.
  - Response: Event object.

- `GET /flows`
  - Description: Retrieves all flows.
  - Response: Array of flow objects.

- `GET /flow/{flow-id}`
  - Description: Retrieves details of a specific flow.
  - Path Parameters:
    - `flow-id`: The unique identifier of the flow.
  - Response: Flow object.

- `POST /flow`
  - Description: Creates a new flow.
  - Request Body: Flow data object.
  - Response: Object containing the created flow ID.

- `POST /route/{event-id}`
  - Description: Updates the routing configuration for an event.
  - Path Parameters:
    - `event-id`: The unique identifier of the event.
  - Request Body: Routing configuration object.
  - Response: Success message.

- `DELETE /flow/{flow-id}`
  - Description: Deletes a specific flow.
  - Path Parameters:
    - `flow-id`: The unique identifier of the flow.
  - Response: Success message.

## Contributing

Contributions to the event-driven system are welcome! If you find any issues or have suggestions for improvements, please open an issue or submit a pull request.

## License

This project is licensed under the [MIT License](LICENSE).
