# Software Requirements Specification (SRS)

# ferrousChat
**A Cloud-Native Real-Time Terminal Chat Application**

**Document Version:** 0.1 — Initial Draft  
**Document Status:** Draft for Review  
**Course:** BCSE355L — Cloud Architecture Design  
**Development Team:** 2–3 members  
**Estimated Development Period:** Less than 2 weeks  
**Target Platforms:** Linux and macOS  
**Primary Language:** Rust  
**Cloud Platform:** Amazon Web Services (AWS)  
**External Object Storage:** Cloudflare R2

---

## 1. Introduction

### 1.1 Purpose

This Software Requirements Specification (SRS) defines the functional and non-functional requirements, system interfaces, architectural constraints, data requirements, and acceptance criteria for **ferrousChat**, a cloud-native real-time messaging application accessed through a terminal-based user interface.

The document establishes a common understanding of the proposed system among developers, project evaluators, testers, and other stakeholders.

It serves as the baseline for system design, implementation, testing, deployment, and academic evaluation.

### 1.2 Project Overview

ferrousChat is a multi-user, real-time chat application with a Terminal User Interface (TUI) developed using Rust and the Ratatui framework.

The application enables users to communicate through public and private chat rooms, exchange direct messages, maintain persistent conversation histories, and share small files.

The backend is hosted on AWS using a serverless architecture. AWS services handle WebSocket communication, application execution, authentication, persistent storage, and operational monitoring.

Cloudflare R2 provides S3-compatible object storage for file attachments.

The system is intended as an educational demonstration of cloud-native architecture, integrating distributed communication, managed infrastructure, serverless computing, database persistence, access control, observability, and cost-conscious deployment.

### 1.3 Objectives

The primary objectives are:

- Develop a responsive, keyboard-driven terminal chat client using Rust and Ratatui.
- Enable real-time communication between multiple authenticated users.
- Support public and private communication channels.
- Preserve messages for retrieval across sessions.
- Implement authentication and authorization using AWS-managed identity services.
- Enable secure file sharing using Cloudflare R2.
- Integrate at least five AWS services with clearly defined responsibilities.
- Demonstrate cloud computing concepts including scalability, fault tolerance, access control, and monitoring.
- Operate within permitted free-tier or free-account resource allowances without incurring monetary charges.

### 1.4 Scope

The system shall provide:

1. User registration and authentication.
2. Public chat rooms.
3. Private chat rooms with membership restrictions.
4. One-to-one direct messaging.
5. Persistent message history.
6. Real-time message delivery using WebSockets.
7. Online/offline presence indicators.
8. Typing indicators.
9. File and image attachments.
10. Keyboard-driven navigation and slash commands.
11. Automatic reconnection and message history synchronization.
12. Cloud-based logging and basic monitoring.

The application will be implemented as a proof-of-concept academic project, not a production-grade commercial messaging platform.

### 1.5 Definitions and Abbreviations

| Term | Definition |
|---|---|
| AWS | Amazon Web Services |
| TUI | Terminal User Interface |
| API | Application Programming Interface |
| WebSocket | Protocol supporting bidirectional communication over a persistent connection |
| Lambda | AWS serverless compute service |
| DynamoDB | AWS-managed NoSQL database |
| Cognito | AWS authentication and user identity service |
| CloudWatch | AWS logging and monitoring service |
| R2 | Cloudflare object storage service |
| IAM | AWS Identity and Access Management |
| DM | Direct Message |
| TTL | Time to Live |
| MVP | Minimum Viable Product |
| Presigned URL | Time-limited authorization URL for an object storage operation |
| TBD | To Be Determined |

### 1.6 Document Conventions

Requirements use the following identifiers:

- **FR-XXX:** Functional Requirements
- **NFR-XXX:** Non-Functional Requirements
- **IR-XXX:** Interface Requirements
- **DR-XXX:** Data Requirements
- **AC-XXX:** Acceptance Criteria

Priority classifications:

- **MUST:** Mandatory for acceptance.
- **SHOULD:** Desirable within the project timeline.
- **COULD:** Optional enhancement.

The words *shall*, *should*, and *may* indicate mandatory, recommended, and optional behavior respectively.

---

## 2. Overall Description

### 2.1 Product Perspective

ferrousChat is a client-server application with a serverless cloud backend.

Unlike conventional terminal chat applications that maintain their own continuously running server process, ferrousChat delegates connection management and backend execution to managed AWS infrastructure.

The application consists of three principal subsystems.

**Terminal Client**

A Rust application responsible for displaying the user interface, accepting user input, managing client-side state, and communicating with the cloud backend.

**AWS Backend**

A collection of managed cloud services responsible for authentication, processing messages, maintaining user connections, storing application data, and recording operational information.

**Object Storage**

A Cloudflare R2 bucket responsible for storing user-uploaded files and images.

### 2.2 User Classes and Actors

**ACT-01: Guest User**

An individual who has not authenticated.

Permitted actions:

- Launch the application.
- Register an account.
- Log in.

Guests cannot access messages, rooms, attachments, or presence information.

**ACT-02: Authenticated User**

A registered user who has successfully authenticated.

Permitted actions:

- Join and participate in public rooms.
- Create public or private rooms.
- Access private rooms of which they are members.
- Send and receive direct messages.
- View permitted message histories.
- Upload and download permitted attachments.
- View presence and typing indicators.
- Use terminal interface commands.

**ACT-03: Private Room Creator**

An authenticated user who creates a private room.

Additional permissions:

- Invite or add participants through a supported membership mechanism.
- Manage membership of the room they created.

Ownership transfer, moderation, and removal policies are TBD.

**ACT-04: System Administrator / Developer**

A team member responsible for AWS infrastructure and system maintenance.

Responsibilities:

- Deploy cloud resources.
- Configure IAM permissions.
- Inspect application logs.
- Monitor errors and resource usage.
- Manage application configuration.

Administrative access to user message content is not part of the normal TUI feature set.

**ACT-05: External Cloud Services**

AWS and Cloudflare infrastructure participate as external service actors in authentication, storage, and message processing.

### 2.3 Operating Environment

**Client Environment**

- Linux operating systems.
- macOS operating systems.
- Terminal emulator supporting ANSI terminal controls.
- Rust-compatible target architecture.
- Stable internet connection for cloud communication.

Minimum supported terminal dimensions: TBD.

**Server Environment**

- AWS API Gateway WebSocket API.
- AWS Lambda using Rust executables on a supported custom runtime.
- Amazon DynamoDB.
- Amazon Cognito User Pools.
- Amazon CloudWatch.
- AWS IAM roles and policies.

**Storage Environment**

- Cloudflare R2 bucket.
- S3-compatible object storage API.
- Private object access using temporary signed URLs.

### 2.4 Design and Implementation Constraints

**CON-01:** The terminal client shall be implemented in Rust.

**CON-02:** The terminal UI shall use Ratatui.

**CON-03:** Application backend logic shall be implemented in Rust and deployed using AWS Lambda.

**CON-04:** WebSocket connectivity shall be provided through Amazon API Gateway.

**CON-05:** Persistent application data shall be stored in DynamoDB.

**CON-06:** User authentication shall be provided through Amazon Cognito.

**CON-07:** Operational logging and monitoring shall use Amazon CloudWatch.

**CON-08:** File and image objects shall be stored in Cloudflare R2 rather than Amazon S3.

**CON-09:** At least five distinct AWS services shall have demonstrable architectural responsibilities.

**CON-10:** The project shall be designed for zero out-of-pocket expenditure, subject to account eligibility and configured usage restrictions.

**CON-11:** The project shall prioritize a working academic demonstration over enterprise-scale production capabilities.

### 2.5 Assumptions and Dependencies

- Developers have access to a personal AWS account supporting the selected services.
- AWS account eligibility, service availability, and free-account restrictions must be verified before deployment.
- A Cloudflare R2 account and bucket can be configured.
- Users possess a working network connection.
- All clients use a compatible version of the application protocol.
- The expected demonstration workload is small.
- Persistent messages are not automatically deleted by application retention policies.
- The system does not guarantee uninterrupted operation when AWS or Cloudflare services are unavailable.
- The application is intended for authorized educational use.

---

## 3. Functional Requirements

### 3.1 User Registration and Authentication

**FR-001 — User Registration [MUST]**

The system shall allow new users to register using a unique account identifier and authentication credentials supported by Amazon Cognito.

The final choice between email-based and username-based sign-in is TBD.

**FR-002 — User Authentication [MUST]**

The system shall authenticate registered users through Amazon Cognito before allowing access to messaging functionality.

**FR-003 — Authentication Tokens [MUST]**

The client shall manage authentication tokens and provide valid authorization credentials when initiating authenticated backend operations.

**FR-004 — Session Expiration [MUST]**

The system shall reject expired or invalid authentication credentials and provide a mechanism for the client to reauthenticate.

**FR-005 — Logout [MUST]**

The client shall provide a logout operation that clears locally retained authentication information and terminates the active chat connection.

**FR-006 — User Identity [MUST]**

Every authenticated user shall have a stable internal identifier independent of their current WebSocket connection.

---

### 3.2 Public and Private Chat Rooms

**FR-007 — Room Listing [MUST]**

The system shall provide a list of public rooms available to authenticated users.

**FR-008 — Public Room Creation [MUST]**

Authenticated users shall be able to create public chat rooms.

**FR-009 — Public Room Participation [MUST]**

Authenticated users shall be able to join available public rooms.

**FR-010 — Private Room Creation [MUST]**

Authenticated users shall be able to create private rooms with restricted membership.

**FR-011 — Private Room Membership [MUST]**

Only authorized members shall be allowed to view private room messages or submit new messages to a private room.

**FR-012 — Room Switching [MUST]**

Users shall be able to switch between available rooms without restarting the terminal client.

**FR-013 — Room Metadata [MUST]**

The system shall maintain a stable room identifier, room name, room visibility, creator identifier, and creation timestamp.

**FR-014 — Private Room Invitations [SHOULD]**

Private room creators should be able to invite other registered users by identifier or username.

**FR-015 — Room Deletion [COULD]**

Authorized users may be permitted to delete rooms.

Deletion ownership rules and message retention behavior are TBD.

---

### 3.3 Real-Time Messaging

**FR-016 — Send Message [MUST]**

Authenticated users shall be able to submit text messages to rooms for which they have permission to send messages.

**FR-017 — Real-Time Reception [MUST]**

The system shall attempt to deliver newly accepted messages to connected recipients without requiring manual refresh.

**FR-018 — Message Persistence [MUST]**

Successfully accepted messages shall be stored in DynamoDB before the sender receives a successful persistence acknowledgement.

**FR-019 — Message Identification [MUST]**

Each stored message shall have a unique identifier.

**FR-020 — Message Metadata [MUST]**

Messages shall contain sufficient metadata to identify the sender, conversation, creation time, and message type.

**FR-021 — Message Ordering [MUST]**

The client shall present retrieved messages in a deterministic order using the server-recorded message ordering fields.

Strict total ordering across concurrent writers is not guaranteed.

**FR-022 — Message Length Validation [MUST]**

The backend shall reject messages exceeding the configured text message size limit.

The final maximum text length is TBD.

**FR-023 — Delivery Semantics [MUST]**

The system shall use best-effort live message delivery with durable persistence.

The system shall not claim guaranteed exactly-once WebSocket delivery.

**FR-024 — History Retrieval [MUST]**

Authorized users shall be able to retrieve previously stored conversation messages.

**FR-025 — History Pagination [SHOULD]**

Message history should be retrieved in bounded pages rather than loading an unlimited conversation into memory.

**FR-026 — Reconnection Synchronization [MUST]**

After reconnection, the client shall be able to retrieve messages missed during its disconnected period.

**FR-027 — Sender Acknowledgement [MUST]**

The system shall distinguish a successfully persisted message from a message that failed validation or persistence.

---

### 3.4 Direct Messaging

**FR-028 — Direct Message Initiation [MUST]**

Authenticated users shall be able to initiate one-to-one conversations with other registered users.

**FR-029 — Direct Message Privacy [MUST]**

Only the participants in a direct conversation shall be authorized to retrieve its messages.

**FR-030 — Offline Direct Messages [MUST]**

Messages sent to offline recipients shall remain in persistent storage for later retrieval.

**FR-031 — Direct Message History [MUST]**

Users shall be able to retrieve previous messages from their direct conversations.

**FR-032 — Multiple Conversations [MUST]**

The client shall support switching between room conversations and direct conversations.

---

### 3.5 User Presence and Typing Indicators

**FR-033 — Connection Presence [MUST]**

The system shall maintain sufficient connection information to determine whether a user appears to be online.

**FR-034 — Online/Offline Display [MUST]**

The client shall display online/offline presence indicators for relevant users.

**FR-035 — Presence Synchronization [MUST]**

The backend shall update presence state on connection events, disconnection events, or stale-session cleanup.

**FR-036 — Stale Connection Handling [MUST]**

The system shall provide a mechanism for eventually clearing invalid or abandoned connection records.

**FR-037 — Typing Events [MUST]**

The client shall be able to emit temporary typing events while a user is composing a message.

**FR-038 — Typing Visibility [MUST]**

The backend shall forward typing events only to recipients authorized to participate in the conversation.

**FR-039 — Typing Expiration [MUST]**

Typing indicators shall expire automatically if no further typing activity is received.

**FR-040 — Non-Persistence of Typing Events [MUST]**

Typing events shall not be stored as conversation history.

Presence and typing indicators are advisory and may be temporarily inaccurate during failures or connection interruptions.

---

### 3.6 File and Image Attachments

**FR-041 — Attachment Upload [MUST]**

Authenticated users shall be able to upload files to conversations in which they are authorized to send messages.

**FR-042 — Object Storage [MUST]**

Attachment content shall be stored in Cloudflare R2.

**FR-043 — Maximum File Size [MUST]**

The system shall reject attachments larger than **2 MiB (2,097,152 bytes)**.

**FR-044 — Demonstration Image Size [SHOULD]**

Image attachments used in project demonstrations should be smaller than **1 MiB (1,048,576 bytes)**.

Images from 1 MiB to 2 MiB remain valid under the general attachment limit.

**FR-045 — Upload Authorization [MUST]**

The backend shall authorize uploads and provide time-limited presigned URLs for direct object storage operations.

**FR-046 — Attachment Metadata [MUST]**

The system shall persist attachment metadata, including object key, original filename, file size, content type, uploader, and associated conversation.

**FR-047 — Attachment Message [MUST]**

Successfully uploaded attachments shall be represented within chat conversations as attachment messages or message references.

**FR-048 — Secure Downloads [MUST]**

The backend shall verify conversation access permissions before issuing attachment download authorization.

**FR-049 — Private Bucket [MUST]**

The R2 bucket shall not provide unrestricted public read access.

**FR-050 — Upload Validation [MUST]**

Upload requests shall be validated for permitted size and supported content types.

**FR-051 — Attachment Failure Handling [MUST]**

Failed uploads shall not be represented to other users as successfully uploaded attachments.

**FR-052 — Terminal Attachment Display [MUST]**

The client shall display the filename and basic metadata of an attachment and allow users to download it.

Inline terminal image rendering is not required.

**FR-053 — Orphaned Object Cleanup [SHOULD]**

The system should provide a mechanism for identifying and removing uploads that were never finalized into valid attachment messages.

**FR-054 — Attachment Deletion [COULD]**

Authorized users may be allowed to delete attachments and their associated metadata.

Deletion policy and authorization rules are TBD.

---

### 3.7 Terminal User Interface

**FR-055 — Ratatui Interface [MUST]**

The terminal client shall use Ratatui to display the chat interface.

**FR-056 — Conversation View [MUST]**

The interface shall display messages from the currently selected conversation.

**FR-057 — Room Navigation [MUST]**

The interface shall provide a mechanism for viewing and switching between accessible rooms.

**FR-058 — Message Input [MUST]**

The interface shall provide a text input area for composing and sending messages.

**FR-059 — Keyboard Navigation [MUST]**

The application shall support keyboard-based navigation without requiring mouse interaction.

**FR-060 — Status Display [MUST]**

The interface shall indicate connection, authentication, and message-send status.

**FR-061 — Slash Commands [MUST]**

The client shall support command-based operations such as:

- `/help` — Display available commands.
- `/rooms` — List available rooms.
- `/join` — Join a public room.
- `/dm` — Open a direct conversation.
- `/upload` — Upload an attachment.
- `/quit` — Exit the application.

Exact argument syntax is TBD.

**FR-062 — Scrollable History [MUST]**

Users shall be able to scroll through loaded message history.

**FR-063 — Error Feedback [MUST]**

The client shall display understandable error messages when operations fail.

**FR-064 — Terminal Resize [SHOULD]**

The interface should respond to terminal resizing without crashing or corrupting terminal state.

---

### 3.8 Connection Management

**FR-065 — WebSocket Connection [MUST]**

The terminal client shall communicate with the messaging backend using secure WebSocket connections.

**FR-066 — Automatic Reconnection [MUST]**

The client shall attempt to reconnect when an unexpected connection interruption occurs.

**FR-067 — Retry Backoff [SHOULD]**

Reconnect attempts should use bounded exponential backoff or another mechanism that avoids excessive retry traffic.

**FR-068 — Connection Tracking [MUST]**

The backend shall maintain mappings between authenticated users and active WebSocket connection identifiers.

**FR-069 — Multiple Connections [MUST]**

The backend shall be capable of routing events to multiple active connections belonging to the same user.

**FR-070 — Offline Recovery [MUST]**

Following successful reconnection and authentication, the client shall recover accessible persisted conversation history as needed.

---

### 3.9 Logging and Monitoring

**FR-071 — Structured Logging [MUST]**

Backend functions shall generate structured operational logs.

**FR-072 — Error Monitoring [MUST]**

The system shall expose sufficient information through CloudWatch to identify Lambda failures and other application errors.

**FR-073 — Operational Metrics [MUST]**

The deployment shall make invocation counts, execution errors, and execution duration observable through CloudWatch.

**FR-074 — Sensitive Data Redaction [MUST]**

Application logs shall not intentionally include passwords, raw authentication tokens, R2 access keys, or presigned URLs.

**FR-075 — Cost Visibility [MUST]**

Developers shall be able to inspect AWS resource usage and billing information through available account management facilities.

**FR-076 — Monitoring Dashboard [SHOULD]**

The project should provide a basic CloudWatch monitoring dashboard where supported by account permissions and cost constraints.

---

## 4. Non-Functional Requirements

### 4.1 Performance

**NFR-001 — Interactive Responsiveness [MUST]**

The TUI shall remain responsive to navigation and input while asynchronous network requests are in progress.

**NFR-002 — Messaging Latency [SHOULD]**

During controlled demonstrations under normal network conditions, 95% of successfully delivered live text messages should become visible to connected recipients within 2 seconds of backend acceptance.

**NFR-003 — Demonstration Capacity [MUST]**

The deployed system shall support at least five simultaneously connected test clients in the planned demonstration environment.

**NFR-004 — History Retrieval [SHOULD]**

A request for a bounded page of recent message history should complete within 3 seconds under normal demonstration conditions.

**NFR-005 — Resource Consumption [SHOULD]**

The terminal client should avoid unbounded growth of in-memory message history and background tasks.

### 4.2 Reliability and Availability

**NFR-006 — Durable Message Acceptance [MUST]**

A successful persistence acknowledgement shall only occur after DynamoDB confirms the corresponding write operation.

**NFR-007 — Best-Effort Live Delivery [MUST]**

A message may fail to reach an active recipient in real time without invalidating its persisted state.

**NFR-008 — Failure Recovery [MUST]**

The system shall support restoration of conversation state after a client reconnects.

**NFR-009 — Fault Isolation [SHOULD]**

Failures involving attachment storage should not prevent normal text messaging operations when the messaging backend remains available.

**NFR-010 — No Formal Availability SLA [MUST]**

The academic prototype shall not claim a contractual uptime guarantee.

### 4.3 Security

**NFR-011 — Transport Encryption [MUST]**

Client-to-backend communication shall use encrypted HTTPS and WSS connections.

**NFR-012 — Authenticated Access [MUST]**

Messaging operations shall be available only to authenticated users.

**NFR-013 — Server-Side Authorization [MUST]**

Authorization decisions for rooms, DMs, and attachments shall be enforced by trusted backend code.

**NFR-014 — Least Privilege [MUST]**

AWS IAM roles shall provide only permissions necessary for assigned operations.

**NFR-015 — Secret Protection [MUST]**

AWS credentials and Cloudflare R2 credentials shall not be embedded in distributed client binaries or committed to source control.

**NFR-016 — Identity Integrity [MUST]**

Backend processing shall derive the authenticated sender identity from trusted authentication context rather than trusting a user ID supplied in a message payload.

**NFR-017 — Private Conversation Isolation [MUST]**

Unauthorized users shall not be able to retrieve private room or direct message data.

**NFR-018 — Attachment Access Control [MUST]**

Attachment operations shall require authorization checks before upload or download credentials are issued.

**NFR-019 — Credential Exposure Prevention [MUST]**

Authentication tokens and temporary signed URLs shall not be exposed through routine logs or debugging output.

**NFR-020 — Stored Data Security [MUST]**

The system shall use the encryption-at-rest capabilities provided by its configured managed storage services.

### 4.4 Usability

**NFR-021 — Keyboard-First Operation [MUST]**

All essential chat actions shall be accessible through keyboard input.

**NFR-022 — Consistent Navigation [MUST]**

The interface shall maintain consistent navigation and command behavior across supported client platforms.

**NFR-023 — Error Clarity [MUST]**

Errors shall be communicated using understandable descriptions rather than exposing raw backend exception traces.

**NFR-024 — Recoverable Interface State [MUST]**

Temporary network failures shall not terminate the TUI without allowing the user to observe or handle the failure.

### 4.5 Scalability and Maintainability

**NFR-025 — Managed Scaling [SHOULD]**

The backend architecture should use the scaling capabilities provided by API Gateway, Lambda, and DynamoDB, subject to configured service limits.

**NFR-026 — Modular Structure [MUST]**

The implementation shall separate terminal presentation, client networking, protocol models, backend application logic, and cloud service access responsibilities.

**NFR-027 — Configuration Management [MUST]**

Environment-specific endpoint URLs, table names, bucket names, and other deployment settings shall be configurable without modifying core application logic.

**NFR-028 — Protocol Versioning [SHOULD]**

Application messages should include a versioned protocol convention or equivalent compatibility mechanism.

**NFR-029 — Reproducible Deployment [SHOULD]**

Cloud resources should be created through documented, repeatable configuration or infrastructure-as-code.

### 4.6 Cost Constraints

**NFR-030 — Zero-Spend Objective [MUST]**

The deployment shall be designed to avoid monetary expenditure.

**NFR-031 — Account Eligibility Verification [MUST]**

The project team shall verify that each deployed AWS service is usable under the account's applicable free plan, credits, or free-tier allowances before provisioning it.

**NFR-032 — Minimal Resource Provisioning [MUST]**

The deployment shall avoid unnecessary always-on compute resources, paid networking resources, and avoidable managed service charges.

**NFR-033 — Storage Usage Control [MUST]**

Application file size restrictions and test-data volumes shall be controlled to remain within the intended R2 free allowance.

**NFR-034 — Resource Cleanup [MUST]**

The project shall provide documented steps to remove unused infrastructure and test resources after evaluation.

**NFR-035 — Billing Safeguards [MUST]**

Where supported, account-level spending restrictions or notifications shall be configured before deployment. Notifications alone shall not be represented as hard spending limits.

---

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

## 6. External Interface Requirements

### 6.1 Terminal User Interface

**IR-001:** The system shall expose a keyboard-operable terminal interface.

**IR-002:** The client shall provide a message display region.

**IR-003:** The client shall provide a conversation navigation region.

**IR-004:** The client shall provide a message input region.

**IR-005:** The client shall provide connection status and error indicators.

The final visual layout, colors, and shortcut mappings are TBD.

### 6.2 WebSocket Interface

**IR-006:** Real-time events shall use a secure WSS endpoint.

**IR-007:** Application events shall use a structured JSON message format.

**IR-008:** Every request type shall have a defined operation identifier.

**IR-009:** Error responses shall identify the failed operation and provide a machine-readable error code.

**IR-010:** The protocol shall support messaging, presence, typing, and conversation-related events.

Illustrative client request:

```json
{
  "action": "send_message",
  "request_id": "req-001",
  "conversation_id": "room-123",
  "content": "Hello from ferrousChat!"
}
```

Illustrative server event:

```json
{
  "event": "message_created",
  "message_id": "msg-456",
  "conversation_id": "room-123",
  "sender_id": "user-789",
  "content": "Hello from ferrousChat!",
  "timestamp": "2026-10-09T12:00:00Z"
}
```

These payloads are preliminary and do not constitute a frozen protocol specification.

### 6.3 Authentication Interface

**IR-011:** The backend shall integrate with Amazon Cognito to establish authenticated user identity.

**IR-012:** Authentication failures shall be communicated without disclosing sensitive implementation details.

**IR-013:** Access to messaging interfaces shall be denied when authorization cannot be validated.

### 6.4 Database Interface

**IR-014:** Backend functions shall use authorized AWS credentials to access DynamoDB.

**IR-015:** Conversation data shall be accessed through queries appropriate to the expected conversation access patterns.

**IR-016:** Database access shall not be exposed directly to the terminal client.

### 6.5 Object Storage Interface

**IR-017:** The backend shall integrate with Cloudflare R2 through an S3-compatible object storage interface.

**IR-018:** File content shall be transferred using authorized HTTPS requests.

**IR-019:** R2 secret credentials shall remain in trusted backend configuration.

### 6.6 Observability Interface

**IR-020:** Backend execution logs shall be accessible through CloudWatch.

**IR-021:** Developers shall be able to identify application errors and associate them with relevant request identifiers where practical.

---

## 7. Data Requirements

### 7.1 User Data

**DR-001:** Each registered user shall have a unique immutable internal identifier.

**DR-002:** User authentication credentials shall be managed through Cognito rather than a custom password database.

**DR-003:** Application-specific profile information may be stored separately from authentication records.

Proposed fields:

| Field | Description |
|---|---|
| user_id | Stable Cognito-associated identity |
| username | Display identifier |
| created_at | Registration timestamp |
| last_seen_at | Most recently observed activity time, if implemented |

Final username rules are TBD.

### 7.2 Conversation Data

**DR-004:** Every conversation shall have a unique identifier.

**DR-005:** Conversations shall be classified as public rooms, private rooms, or direct conversations.

**DR-006:** Private rooms and direct conversations shall maintain access-control membership information.

Proposed fields:

| Field | Description |
|---|---|
| conversation_id | Unique identifier |
| conversation_type | Public, private, or DM |
| name | Display name, where applicable |
| creator_id | Creator identity, where applicable |
| created_at | Creation timestamp |
| membership | Authorized participants, represented through an appropriate data model |

### 7.3 Message Data

**DR-007:** Successfully accepted text messages shall be stored persistently.

**DR-008:** Messages shall be associated with a conversation and an authenticated sender.

**DR-009:** Messages shall retain server-assigned timestamps.

**DR-010:** Conversation history shall be retrievable without performing a full table scan for each routine history request.

Proposed fields:

| Field | Description |
|---|---|
| message_id | Unique message identifier |
| conversation_id | Parent conversation |
| sender_id | Authenticated author |
| message_type | Text or attachment |
| content | Text content or display information |
| created_at | Server timestamp |
| attachment_id | Optional attachment reference |

### 7.4 Attachment Data

**DR-011:** Attachment metadata shall be stored separately from attachment file content.

**DR-012:** Object storage references shall use internal object keys rather than permanently public file URLs.

**DR-013:** Attachment metadata shall contain the associated conversation identifier.

Proposed fields:

| Field | Description |
|---|---|
| attachment_id | Unique attachment identifier |
| object_key | Private R2 object key |
| original_filename | User-visible filename |
| content_type | Declared or validated media type |
| size_bytes | File size |
| uploader_id | Uploader identity |
| conversation_id | Associated conversation |
| uploaded_at | Completion timestamp |

### 7.5 Presence and Connection Data

**DR-014:** Active WebSocket connections shall be associated with authenticated user identifiers.

**DR-015:** Connection records shall support stale-session cleanup.

**DR-016:** Typing status shall be treated as temporary, non-durable application state.

Proposed connection fields:

| Field | Description |
|---|---|
| connection_id | API Gateway connection identifier |
| user_id | Authenticated user |
| connected_at | Connection timestamp |
| last_activity_at | Most recent observed activity |
| expires_at | Cleanup eligibility timestamp |

### 7.6 Retention Policy

**DR-017:** Stored messages shall have no automatic application-level age-based deletion policy.

**DR-018:** Application data shall remain available until explicitly deleted through an authorized mechanism or removed during project teardown.

**DR-019:** Expiration policies for temporary connection state shall not remove durable conversation history.

**DR-020:** File and message deletion semantics shall be specified before implementing user-controlled deletion features.

Persistent-until-deletion is the intended retention policy, but the exact deletion permissions and procedures remain TBD.

---

## 8. Key Use Cases

### UC-01 — Register and Log In

**Primary Actor:** Guest User

**Precondition:** User has internet access.

**Main Flow:**

1. User launches ferrousChat.
2. User selects registration or login.
3. User provides required credentials.
4. Cognito authenticates or registers the user.
5. The client obtains valid authorization.
6. The application opens the authenticated interface.

**Alternative Flow:** Invalid credentials or registration failure result in an error message.

**Postcondition:** User has an authenticated application session.

### UC-02 — Send a Room Message

**Primary Actor:** Authenticated User

**Precondition:** User has access to a room.

**Main Flow:**

1. User selects the room.
2. User types a message.
3. User submits the message.
4. Backend validates authorization.
5. Backend persists the message.
6. Sender receives persistence acknowledgement.
7. Backend attempts to notify online participants.

**Postcondition:** Message is stored and available in authorized history requests.

### UC-03 — Send a Direct Message

**Primary Actor:** Authenticated User

**Precondition:** Both participants have valid registered accounts.

**Main Flow:**

1. User selects or opens a DM conversation.
2. User composes a message.
3. Backend validates conversation membership.
4. Backend stores the message.
5. Backend attempts live delivery.
6. Offline recipients retrieve the message later.

**Postcondition:** Message is persisted and restricted to authorized participants.

### UC-04 — Upload an Attachment

**Primary Actor:** Authenticated User

**Precondition:** User has permission to send messages in the conversation.

**Main Flow:**

1. User chooses a local file.
2. Client verifies file size.
3. Backend authorizes upload.
4. Client uploads using a temporary R2 URL.
5. Backend verifies the completed object.
6. Attachment metadata and message reference are persisted.
7. Recipients can view and retrieve the attachment.

**Alternative Flow:** Oversized or unauthorized uploads are rejected.

**Postcondition:** The attachment is available to authorized participants.

### UC-05 — Recover After Network Disconnection

**Primary Actor:** Authenticated User

**Precondition:** Client loses its active WebSocket connection.

**Main Flow:**

1. Client detects the disconnected state.
2. Client displays a connection warning.
3. Client attempts reconnection.
4. Backend validates the re-established session.
5. Client requests missing conversation history.
6. Client updates its displayed conversation state.

**Postcondition:** The client resumes normal messaging without requiring already persisted messages to be recreated.

---

## 9. Acceptance Criteria

The following tests constitute the minimum acceptance baseline.

| ID | Acceptance Test | Expected Result |
|---|---|---|
| AC-001 | Register a new user | Registration completes through Cognito |
| AC-002 | Log in with valid credentials | Authenticated chat session established |
| AC-003 | Attempt access without valid authentication | Access rejected |
| AC-004 | Connect five clients | All five establish usable sessions |
| AC-005 | Send message in public room | Authorized connected participants receive it |
| AC-006 | Create and join a private room | Authorized membership enforced |
| AC-007 | Unauthorized private room access | Access denied |
| AC-008 | Send direct message | Only intended participants have access |
| AC-009 | Disconnect a recipient before sending | Message remains available in history |
| AC-010 | Restart a client | Previously stored history can be retrieved |
| AC-011 | Type in a conversation | Other authorized participants see temporary typing status |
| AC-012 | Disconnect a user | Presence eventually changes to offline |
| AC-013 | Upload a file below 2 MiB | Upload succeeds with valid authorization |
| AC-014 | Upload a file above 2 MiB | Upload rejected |
| AC-015 | Request attachment from unauthorized account | Access rejected |
| AC-016 | Upload a demo image below 1 MiB | Image stored and referenced successfully |
| AC-017 | Use slash commands | Supported commands invoke intended operations |
| AC-018 | Interrupt network connection | Client displays failure and attempts recovery |
| AC-019 | Trigger a controlled backend error | Diagnostic evidence appears in CloudWatch |
| AC-020 | Inspect cloud deployment | All five required AWS service integrations are demonstrable |
| AC-021 | Validate client portability | Client operates on Linux and macOS |
| AC-022 | Review service usage and charges | Deployment remains within approved zero-spend conditions |

Acceptance tests involving external cloud infrastructure shall be performed only after eligibility and cost controls have been verified.

---

## 10. Test Strategy

### 10.1 Unit Testing

Unit tests shall cover:

- Command parsing.
- Message serialization and deserialization.
- Input validation.
- File size validation.
- Conversation membership checks.
- Message routing decisions.
- Local UI state transitions.

### 10.2 Integration Testing

Integration tests shall cover:

- Lambda and DynamoDB interactions.
- API Gateway and Lambda event processing.
- Authentication and authorization.
- File authorization and R2 transfers.
- Message history retrieval.
- Error reporting through CloudWatch.

### 10.3 End-to-End Testing

End-to-end tests shall simulate multiple users operating independent terminal clients.

Test scenarios shall include room messaging, DMs, offline recovery, presence changes, and attachment handling.

### 10.4 Security Testing

Security testing shall include:

- Invalid and expired credentials.
- Unauthorized private room access.
- Unauthorized DM history access.
- Unauthorized attachment download requests.
- Invalid message payloads.
- Oversized uploads.
- Verification that credentials are absent from distributed binaries and routine logs.

### 10.5 Performance Testing

Lightweight performance tests shall measure:

- Message delivery latency.
- History retrieval latency.
- Connection stability.
- Behavior under five simultaneous clients.
- Response to repeated connection interruptions.

A large-scale load test is outside the project scope.

---

## 11. Out-of-Scope Features

The following features are excluded from the initial release:

- Voice and video calls.
- Screen sharing.
- End-to-end encryption.
- Mobile applications.
- Web browser chat client.
- Rich media inline rendering in the terminal.
- Large file transfer.
- Enterprise administration features.
- Advanced moderation and content filtering.
- Message reactions.
- Threaded replies.
- Full-text search.
- AI-powered message processing.
- Push notifications outside the terminal client.
- Guaranteed exactly-once message delivery.
- Multi-region disaster recovery.
- Commercial-scale uptime guarantees.

These may be considered future enhancements after the core system is complete.

---

## 12. Project Risks and Mitigation

| Risk | Impact | Proposed Mitigation |
|---|---|---|
| Free-account service restrictions | Deployment may be blocked | Verify supported AWS services before implementation |
| Rust Lambda deployment complexity | Development delay | Establish a working minimal Lambda early |
| WebSocket state handling | Incorrect delivery or presence | Store connection metadata and implement cleanup |
| Authentication integration | Unauthorized access or connection failures | Test authentication before implementing advanced chat features |
| R2 billing or configuration issues | Unexpected costs or upload failure | Use minimal test files and confirm free allowance and billing behavior |
| Scope expansion | Incomplete project | Prioritize mandatory acceptance tests |
| Service outages | Failed demonstration | Prepare controlled test accounts, logs, and recovery procedures |
| Limited development time | Incomplete integrations | Use modular architecture and stable protocol interfaces |

---

## 13. Future Enhancements

Potential improvements beyond the initial project include:

- Message editing and deletion.
- File preview rendering.
- User-defined keyboard shortcuts.
- Read receipts.
- Advanced notification controls.
- Terminal themes.
- Searchable conversation archives.
- Large-group chat optimization.
- Improved delivery acknowledgements and deduplication.
- Infrastructure automation and CI/CD.
- Extended observability and dashboarding.
- More advanced role-based room permissions.

Future enhancements are not part of initial acceptance unless explicitly promoted to mandatory requirements.

---

## 14. Open Questions and Items to Be Determined

The following matters require further design decisions.

| ID | Pending Decision | Current Status |
|---|---|---|
| TBD-01 | Account sign-in identifier: email or username | Open |
| TBD-02 | Account verification and password recovery workflow | Open |
| TBD-03 | Minimum terminal size | Open |
| TBD-04 | Maximum text message length | Open |
| TBD-05 | Exact WebSocket authorization mechanism | Open |
| TBD-06 | Exact JSON protocol schema and error codes | Open |
| TBD-07 | DynamoDB table design and partition keys | Open |
| TBD-08 | Private room invitation and removal rules | Open |
| TBD-09 | Message and attachment deletion permissions | Open |
| TBD-10 | R2 bucket region/jurisdiction and object naming policy | Open |
| TBD-11 | Supported attachment MIME types | Open |
| TBD-12 | Terminal keyboard shortcuts and final visual layout | Open |
| TBD-13 | Infrastructure deployment framework | Open |
| TBD-14 | AWS account eligibility and service-specific free usage | Open |
| TBD-15 | Presence expiry and typing timeout thresholds | Open |
| TBD-16 | Client local token storage policy | Open |
| TBD-17 | Concurrency behavior, duplicate delivery, and reconciliation details | Open |
| TBD-18 | Private room creator administration permissions | Open |
| TBD-19 | Log retention and monitoring configuration | Open |
| TBD-20 | Privacy and retention treatment of data at project teardown | Open |

These items must be resolved before the affected components are considered implementation-ready.

---

## 15. Academic Alignment

The project supports the learning objectives of BCSE355L — Cloud Architecture Design.

| Syllabus Area | ferrousChat Application |
|---|---|
| AWS Cloud Foundations and IAM | Least-privilege permissions and cloud architecture |
| AWS Compute Services | Lambda-based Rust backend |
| AWS Networking | Secure WebSocket and HTTPS communication |
| AWS Database Services | DynamoDB persistence and data access patterns |
| AWS Management and Cost | CloudWatch monitoring, resource usage, and cost controls |
| AWS Security and Compliance | Cognito authentication, authorization, credential protection |
| AWS Application Integration | API Gateway and Lambda event processing |
| Architectural Best Practices | Managed scaling, fault recovery, modularity, security and cost-conscious design |

The project demonstrates practical use of multiple AWS services while keeping the principal user interaction within a Rust terminal application.

---

## 16. Conclusion

ferrousChat is designed to demonstrate a functional, cloud-native real-time messaging application using Rust, AWS serverless infrastructure, and Cloudflare R2.

The proposed system combines authenticated user communication, persistent message storage, room-based access control, private messaging, presence tracking, typing indicators, and secure file sharing.

Its architecture emphasizes the separation of client presentation, real-time communication, backend processing, persistent data, identity management, monitoring, and object storage.

The SRS defines a testable baseline for academic implementation and evaluation while explicitly documenting non-goals, financial constraints, and unresolved design choices.

**Document Status:** Draft v0.1 — Pending stakeholder review and architectural validation.