## Document-Oriented Database (Firebase / Firestore)

### Collection: chats
Document:
```json
{
	"id": "chat_1",
	"request_id": "request_1",
	"user_id": "user_1",
	"companion_id": "companion_1",
	"status": "active",
	"created_at": "2026-04-25T10:00:00Z",
	"updated_at": "2026-04-25T10:00:00Z",
	"last_message": "Hello",
	"last_message_at": "2026-04-25T10:05:00Z",
	"last_message_sender_id": "user_1",
	"user_unread_count": 1,
	"companion_unread_count": 0
}
```
Mandatory fields: `request_id`, `user_id`, `companion_id`, `status`, `created_at`, `updated_at`
Optional fields: `last_message`, `last_message_at`, `last_message_sender_id`, `user_unread_count`, `companion_unread_count`

### Collection: chat_messages
Document:
```json
{
	"id": "message_1",
	"chat_id": "chat_1",
	"sender_id": "user_1",
	"message_type": "text",
	"created_at": "2026-04-25T10:05:00Z",
	"message_text": "I am here",
	"attachment_url": null,
	"is_read": false,
	"read_at": null
}
```
Mandatory fields: `chat_id`, `sender_id`, `message_type`, `created_at`
Optional fields: `message_text`, `attachment_url`, `is_read`, `read_at`

### Collection: trip_live_tracking
Document:
```json
{
	"id": "track_1",
	"request_id": "request_1",
	"trip_session_id": "trip_1",
	"companion_id": "companion_1",
	"latitude": 24.7136,
	"longitude": 46.6753,
	"updated_at": "2026-04-25T10:10:00Z",
	"speed": 45,
	"heading": 180
}
```
Mandatory fields: `request_id`, `trip_session_id`, `companion_id`, `latitude`, `longitude`, `updated_at`
Optional fields: `speed`, `heading`

### Collection: presence
Document:
```json
{
	"user_id": "companion_1",
	"status": "online",
	"updated_at": "2026-04-25T10:10:00Z",
	"last_seen_at": null
}
```
Mandatory fields: `status`, `updated_at`
Optional fields: `last_seen_at`

### Collection: notifications
Document:
```json
{
	"id": "notif_1",
	"user_id": "user_1",
	"title": "Trip Update",
	"body": "Your companion has arrived.",
	"notification_type": "trip_update",
	"is_read": false,
	"created_at": "2026-04-25T10:15:00Z",
	"related_type": "request",
	"related_id": "request_1",
	"read_at": null
}
```
Mandatory fields: `user_id`, `title`, `body`, `notification_type`, `is_read`, `created_at`
Optional fields: `related_type`, `related_id`, `read_at`

### Collection: support_chat
Document:
```json
{
	"id": "support_1",
	"ticket_id": "ticket_1",
	"sender_id": "user_1",
	"message_text": "I have a complaint.",
	"created_at": "2026-04-25T10:20:00Z",
	"attachment_url": null,
	"is_read": false
}
```
Mandatory fields: `ticket_id`, `sender_id`, `message_text`, `created_at`
Optional fields: `attachment_url`, `is_read`
