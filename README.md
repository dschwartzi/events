# Event Store API Handler

This Lambda function serves as an API handler for managing events, flows, and elements in a DynamoDB-based event store. It supports CRUD operations via HTTP methods and routes.

### Setup

Ensure you have the following prerequisites:

- AWS Lambda
- AWS DynamoDB
- AWS API Gateway or Lambda URL

### Routes

The following routes are supported by the handler:

- **GET /events**: Retrieve all events.
- **GET /flows**: Retrieve all flows.
- **GET /elements**: Retrieve all elements across all flows.
- **GET /elements/{flowName}**: Retrieve elements for a specific flow.
- **GET /event/{eventName}**: Retrieve details of a specific event.
- **GET /flow/{flowId}**: Retrieve details of a specific flow.
- **GET /element/{flowName}/{elementId}**: Retrieve details of a specific element within a specific flow.
- **POST /event**: Create a new event.
- **POST /flow**: Create a new flow.
- **POST /element**: Create a new element.
- **DELETE /flow/{flowId}**: Delete a specific flow.

### Request and Response Examples

#### GET /events

**Request:**

```sh
curl -X GET "https://fnm5fxxy2vahrcele6g3akqn5y0spper.lambda-url.us-east-1.on.aws/events" \
     -H "Content-Type: application/json"
```

**Response:**

```json
[
    {
        "eventName": "FiveSecVideoFragEvent",
        "eventSchema": { ... }
    },
    {
        "eventName": "OneMinFiveSecVideoFragEvent",
        "eventSchema": { ... }
    }
]
```

#### GET /flows

**Request:**

```sh
curl -X GET "https://fnm5fxxy2vahrcele6g3akqn5y0spper.lambda-url.us-east-1.on.aws/flows" \
     -H "Content-Type: application/json"
```

**Response:**

```json
[
    {
        "flowId": "ExampleFlow",
        "data": { ... }
    },
    {
        "flowId": "AnotherFlow",
        "data": { ... }
    }
]
```

#### GET /elements

**Request:**

```sh
curl -X GET "https://fnm5fxxy2vahrcele6g3akqn5y0spper.lambda-url.us-east-1.on.aws/elements" \
     -H "Content-Type: application/json"
```

**Response:**

```json
[
    {
        "elementId": "FiveSecVideoFragCounter",
        "flowName": "ExampleFlow",
        "elementDetails": { ... }
    },
    {
        "elementId": "OneMinFiveSecVideoFragExtractor",
        "flowName": "ExampleFlow",
        "elementDetails": { ... }
    }
]
```

#### GET /elements/{flowName}

**Request:**

```sh
curl -X GET "https://fnm5fxxy2vahrcele6g3akqn5y0spper.lambda-url.us-east-1.on.aws/elements/ExampleFlow" \
     -H "Content-Type: application/json"
```

**Response:**

```json
[
    {
        "elementId": "FiveSecVideoFragCounter",
        "elementDetails": { ... }
    },
    {
        "elementId": "OneMinFiveSecVideoFragExtractor",
        "elementDetails": { ... }
    }
]
```

#### GET /event/{eventName}

**Request:**

```sh
curl -X GET "https://fnm5fxxy2vahrcele6g3akqn5y0spper.lambda-url.us-east-1.on.aws/event/FiveSecVideoFragEvent" \
     -H "Content-Type: application/json"
```

**Response:**

```json
{
    "eventName": "FiveSecVideoFragEvent",
    "eventSchema": { ... }
}
```

#### GET /flow/{flowId}

**Request:**

```sh
curl -X GET "https://fnm5fxxy2vahrcele6g3akqn5y0spper.lambda-url.us-east-1.on.aws/flow/ExampleFlow" \
     -H "Content-Type: application/json"
```

**Response:**

```json
{
    "nodes": [
        {
            "id": "Get Eventbridge-1716717420333",
            "label": "Get Eventbridge"
        },
        {
            "id": "Get Eventbridge-1716717423334",
            "label": "Get Eventbridge"
        },
        {
            "id": "OpenAI-1716717747543",
            "label": "OpenAI"
        }
    ],
    "edges": [
        {
            "id": "reactflow__edge-Get Eventbridge-1716717423334-OpenAI-1716717747543",
            "source": "Get Eventbridge-1716717423334",
            "target": "OpenAI-1716717747543"
        }
    ]
}
```

#### GET /element/{flowName}/{elementId}

**Request:**

```sh
curl -X GET "https://fnm5fxxy2vahrcele6g3akqn5y0spper.lambda-url.us-east-1.on.aws/element/ExampleFlow/FiveSecVideoFragCounter" \
     -H "Content-Type: application/json"
```

**Response:**

```json
{
    "elementId": "FiveSecVideoFragCounter",
    "elementDetails": { ... }
}
```

#### POST /event

**Request:**

```sh
curl -X POST "https://fnm5fxxy2vahrcele6g3akqn5y0spper.lambda-url.us-east-1.on.aws/event" \
     -H "Content-Type: application/json" \
     -d '{
           "eventName": "NewEvent",
           "eventSchema": { ... }
         }'
```

**Response:**

```json
{
    "statusCode": 201,
    "body": "Event created"
}
```

#### POST /flow

**Request:**

```sh
curl -X POST "https://fnm5fxxy2vahrcele6g3akqn5y0spper.lambda-url.us-east-1.on.aws/flow" \
     -H "Content-Type: application/json" \
     -d '{
           "flowName": "NewFlow",
           "data": { ... }
         }'
```

**Response:**

```json
{
    "statusCode": 201,
    "body": { "flowId": "new-flow-id" }
}
```

#### POST /element

**Request:**

```sh
curl -X POST "https://fnm5fxxy2vahrcele6g3akqn5y0spper.lambda-url.us-east-1.on.aws/element" \
     -H "Content-Type: application/json" \
     -d '{
           "flowName": "ExampleFlow",
           "elementId": "NewElement",
           "elementDetails": { ... }
         }'
```

**Response:**

```json
{
    "statusCode": 201,
    "body": "Element created"
}
```

#### DELETE /flow/{flowId}

**Request:**

```sh
curl -X DELETE "https://fnm5fxxy2vahrcele6g3akqn5y0spper.lambda-url.us-east-1.on.aws/flow/ExampleFlow" \
     -H "Content-Type: application/json"
```

**Response:**

```json
{
    "statusCode": 200,
    "body": "Flow deleted"
}
```

### Notes

- Ensure to replace `https://fnm5fxxy2vahrcele6g3akqn5y0spper.lambda-url.us-east-1.on.aws` with your actual API endpoint.
- Adjust CORS headers as per your domain requirements.
```
