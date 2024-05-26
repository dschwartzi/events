# Event-Driven System with EventBridge and Lambda

This repository contains the code for an event-driven system built using AWS EventBridge and AWS Lambda. The system allows you to define, store, and manage events, flows, and routing configurations for processing and transforming data in a flexible and scalable manner.

## Overview

The event-driven system is designed to simplify the process of building event-driven applications by providing a declarative way to define events, flows, and routing configurations. It leverages AWS EventBridge for event communication and routing, and AWS Lambda for executing the processing logic.

The system revolves around the concept of events, which represent significant occurrences or data points in your application. Events are defined using a standard event schema that includes a profile object for session-specific details, a data object for media-related information, and an application object for application-specific information.

## Event Processing Flow

1. Raw data events are sent to the platform via triggers (client SDK, API endpoint, or S3 event listener).
2. EventBridge routes the events to the appropriate subscribers, such as filters, transformers, views, and apps.
3. Filters control the event flow by deduplicating, queuing, or dropping events based on thresholds.
4. Transformers modify the event structure by splitting, combining, or extracting specific elements from events.
5. Views provide different perspectives or representations of events based on specific criteria.
6. Apps are end-to-end solutions that utilize filters, transformers, and views to process events for specific use cases.

## Extensibility through Filters, Transformers, and Views

The platform provides a set of initial filters, transformers, and views that applications can leverage:

- Filters:
  - Counter: Drops events until a specified count or time threshold is reached, then triggers.
  - Aggregator: Caches event metadata until a threshold is reached, then triggers with aggregated metadata.
  - Stitcher: Caches event content until a threshold is reached, then stitches content together and triggers.

- Transformers:
  - Splitter: Takes a single event as input and splits it into multiple output events.
  - Combiner: Caches multiple event types until all are ready, then combines them into a single event.

- Views:
  - Summarizer: Uses LLMs to generate a summary representation of the content in an image event.
  - Transcriber: Converts audio or video event content into a text-based representation using speech recognition.

The platform also allows for the development of custom filters, transformers, and views to extend its capabilities.

## Sample Apps

The event-driven system enables the development of various applications that can subscribe to events processed by filters, transformers, and views, as well as raw data events. Some sample apps include:

- Timecard: Generates detailed timecards by combining video, audio, and application usage events.
- Productivity Coach: Provides personalized productivity insights and recommendations based on aggregated event data.
- Knowledge Base: Automatically captures and organizes knowledge from events into a searchable repository.

## Data Storage and Querying

The platform uses DynamoDB and S3 as the cache for session metadata and object data, respectively. It stores raw data objects, internal data structures, states, questions, responses, etc., in these services. Amazon Q is used to query platform data stored in S3, enabling ad-hoc querying and analysis of data.

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
