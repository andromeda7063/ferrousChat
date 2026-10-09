# ferrousChat data requirements

Data requirements and proposed field tables from the canonical [SRS](srs.md), retained with their original DR IDs.

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
