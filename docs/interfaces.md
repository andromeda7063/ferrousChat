# ferrousChat external interfaces

Interface requirements and illustrative protocol payloads from the canonical [SRS](srs.md). The source explicitly labels the example JSON as preliminary.

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
