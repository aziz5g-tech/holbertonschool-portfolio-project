## chats
Mandatory fields:
- request_id
- user_id
- companion_id
- status
- created_at
- updated_at

Optional fields:
- last_message
- last_message_at
- last_message_sender_id
- user_unread_count
- companion_unread_count

## chat_messages
Mandatory fields:
- chat_id
- sender_id
- message_type
- created_at

Optional fields:
- message_text
- attachment_url
- is_read
- read_at

## trip_live_tracking
Mandatory fields:
- request_id
- trip_session_id
- companion_id
- latitude
- longitude
- updated_at

Optional fields:
- speed
- heading

## presence
Mandatory fields:
- status
- updated_at

Optional fields:
- last_seen_at

## notifications
Mandatory fields:
- user_id
- title
- body
- notification_type
- is_read
- created_at

Optional fields:
- related_type
- related_id
- read_at

## support_chat
Mandatory fields:
- ticket_id
- sender_id
- message_text
- created_at

Optional fields:
- attachment_url
- is_read
