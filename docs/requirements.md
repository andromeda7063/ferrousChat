# ferrousChat functional and non-functional requirements

Verbatim implementation-focused requirements extracted from the canonical [SRS](srs.md). Requirement wording, priorities, IDs, and unresolved details are preserved.

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
