# ferrousChat system architecture

This is the architecture baseline from the SRS. Service names and constraints below are part of the provided draft; do not replace them without recording a change.

## 5. System Architecture

### 5.1 Architecture Style

ferrousChat adopts a cloud-native, event-driven serverless architecture.

The terminal client maintains a WebSocket connection through AWS API Gateway.

Incoming events trigger Rust-based AWS Lambda handlers. Lambda functions validate requests, enforce permissions, access DynamoDB, and send messages to connected clients through API Gateway's connection management interface.

User authentication is provided by Amazon Cognito.

CloudWatch collects execution logs and operational metrics.

File content is stored separately in Cloudflare R2, with the backend authorizing temporary direct upload and download operations.

### 5.2 Cloud Services

| Service | Architectural Responsibility |
|---|---|
| Amazon API Gateway | Secure WebSocket connections, event routing, and connection management |
| AWS Lambda | Rust-based application logic, validation, messaging, and backend integration |
| Amazon DynamoDB | Persistent conversation data, room membership, and connection records |
| Amazon Cognito | User registration and authentication |
| Amazon CloudWatch | Operational logging, monitoring, and error analysis |
| Cloudflare R2 | Private object storage for attachments |

AWS IAM supports service-to-service access control and execution permissions.

Cloudflare R2 is an external service and does not count toward the five required AWS service integrations.

### 5.3 Message Processing Workflow

1. An authenticated client sends a structured message through its WebSocket connection.
2. API Gateway routes the message event to AWS Lambda.
3. Lambda identifies the authenticated sender.
4. Lambda validates the request and conversation permissions.
5. Lambda stores the accepted message in DynamoDB.
6. Lambda acknowledges successful persistence to the sender.
7. Lambda identifies currently connected authorized recipients.
8. Lambda attempts to deliver the event through the API Gateway Management API.
9. Offline recipients retrieve the persisted message when they next connect or request history.

### 5.4 Authentication Workflow

1. A user submits registration or login credentials through the client.
2. Cognito handles authentication and returns the applicable tokens.
3. The client obtains authorization to establish a WebSocket session.
4. A backend authorization mechanism validates the authenticated identity during connection establishment.
5. The established connection is associated with the validated user identity.
6. Subsequent operations are authorized against that identity and the relevant conversation permissions.

The exact mechanism for securely transporting authorization information during the WebSocket handshake is TBD.

Token handling shall avoid unnecessary exposure in logs or persistent local storage.

### 5.5 Attachment Workflow

1. A user selects a local file.
2. The client validates its size against the 2 MiB maximum.
3. The backend checks the user's authentication and conversation permissions.
4. The backend allocates an object key and generates a short-lived upload URL for R2.
5. The client uploads the file directly to R2.
6. The backend verifies successful upload completion.
7. The backend creates the attachment metadata and corresponding conversation message in DynamoDB.
8. The message becomes visible to authorized recipients.
9. A recipient requests access to the attachment.
10. The backend verifies access permission and issues a short-lived download URL.

The client shall not receive long-lived R2 account credentials.

### 5.6 Proposed Logical Components

**Client Components**

- TUI renderer.
- Input and keyboard event handler.
- WebSocket connection manager.
- Authentication manager.
- Conversation state manager.
- File transfer client.
- Command parser.

**Backend Components**

- Connection and authentication handler.
- Message processing handler.
- Conversation and membership handler.
- History retrieval handler.
- Presence and typing event handler.
- Attachment authorization handler.
- Cloud service adapters.

These represent logical responsibilities and do not require one separate Lambda function per component.

---
