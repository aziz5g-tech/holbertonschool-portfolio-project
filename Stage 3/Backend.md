## Key Classes (Attributes & Methods)

### User
Represents any account type in the system: user, companion, or admin.
- **Attributes**: `id`, `first_name`, `last_name`, `mobile`, `email`, `password_hash`, `account_type`, `account_status`, `last_login_at`, `created_at`, `updated_at`
- **Methods**: `updateProfile()`, `changePassword()`, `activate()`, `suspend()`, `recordLogin()`

### UserConsent
Stores the user’s accepted legal agreements.
- **Attributes**: `id`, `user_id`, `consent_type`, `consent_version`, `accepted_at`, `created_at`
- **Methods**: `acceptConsent()`, `validateConsentVersion()`, `hasAccepted()`

### OTPVerification
Stores OTP verification attempts and results.
- **Attributes**: `id`, `user_id`, `channel`, `otp_code_hash`, `expires_at`, `verified_at`, `attempts_count`, `status`, `created_at`
- **Methods**: `generateCode()`, `verifyCode()`, `expireCode()`, `incrementAttempts()`

### CompanionProfile
Stores the companion’s profile details and public health status.
- **Attributes**: `id`, `user_id`, `gender`, `nationality`, `date_of_birth`, `city`, `district`, `short_description`, `experience_summary`, `health_status`, `has_car`, `created_at`, `updated_at`
- **Methods**: `completeProfile()`, `updateProfile()`, `setHealthStatus()`, `setCarFlag()`

### CompanionDocument
Stores documents uploaded by the companion for approval.
- **Attributes**: `id`, `user_id`, `document_type`, `file_url`, `status`, `review_note`, `reviewed_by_admin_id`, `reviewed_at`, `created_at`
- **Methods**: `uploadDocument()`, `approveDocument()`, `rejectDocument()`, `addReviewNote()`

### CompanionVehicle
Stores car details when the companion chooses to work with a car.
- **Attributes**: `id`, `user_id`, `car_type`, `car_model`, `plate_number`, `created_at`, `updated_at`
- **Methods**: `addVehicle()`, `updateVehicle()`, `removeVehicle()`

### CompanionApprovalReview
Stores the admin approval workflow for companion activation.
- **Attributes**: `id`, `user_id`, `profile_completed`, `documents_completed`, `approval_status`, `rejection_reason`, `approved_by_admin_id`, `approved_at`, `created_at`, `updated_at`
- **Methods**: `reviewProfile()`, `approve()`, `reject()`, `recordRejectionReason()`

### CompanionAvailability
Tracks whether the companion is online and willing to receive work.
- **Attributes**: `id`, `user_id`, `connection_status`, `work_status`, `updated_at`
- **Methods**: `setOnline()`, `setOffline()`, `setAvailable()`, `setUnavailable()`

### ServiceRequest
Represents the main service request created by the user.
- **Attributes**: `id`, `user_id`, `request_for`, `service_type`, `request_mode`, `with_car`, `status`, `scheduled_at`, `created_at`, `updated_at`
- **Methods**: `createRequest()`, `attachBeneficiary()`, `attachHealthData()`, `setLocation()`, `assignCompanion()`, `changeStatus()`, `cancelRequest()`, `scheduleRequest()`

### RequestBeneficiary
Stores beneficiary details when the request is made for someone else.
- **Attributes**: `id`, `request_id`, `full_name`, `age`, `mobile`, `gender`, `relationship`, `created_at`
- **Methods**: `saveBeneficiary()`, `updateBeneficiary()`

### RequestHealthData
Stores health-related data attached to a request.
- **Attributes**: `id`, `request_id`, `health_condition`, `other_text`, `created_at`
- **Methods**: `addCondition()`, `updateCondition()`, `removeCondition()`

### RequestLocation
Stores pickup and destination information for a request.
- **Attributes**: `id`, `request_id`, `pickup_city`, `pickup_district`, `pickup_street`, `pickup_building_number`, `pickup_apartment_number`, `pickup_short_address`, `pickup_lat`, `pickup_lng`, `destination_city`, `destination_district`, `destination_street`, `destination_building_number`, `destination_apartment_number`, `destination_short_address`, `destination_lat`, `destination_lng`, `created_at`
- **Methods**: `setPickupLocation()`, `setDestinationLocation()`, `updateCoordinates()`

### TripSession
Represents a real trip session within a request, especially when the companion changes.
- **Attributes**: `id`, `request_id`, `companion_id`, `status`, `started_at`, `ended_at`, `created_at`
- **Methods**: `startTrip()`, `endTrip()`, `extendTrip()`, `transferToAnotherCompanion()`

### Payment
Stores payment records for a request.
- **Attributes**: `id`, `request_id`, `user_id`, `amount`, `status`, `provider_transaction_id`, `paid_at`, `created_at`
- **Methods**: `initiatePayment()`, `confirmPayment()`, `refundPayment()`, `handleWebhook()`

### Refund
Stores payment refunds.
- **Attributes**: `id`, `payment_id`, `amount`, `status`, `reason`, `refunded_at`, `created_at`
- **Methods**: `requestRefund()`, `completeRefund()`, `failRefund()`

### Wallet
Represents the companion wallet.
- **Attributes**: `id`, `companion_id`, `available_balance`, `monthly_earnings`, `status`, `created_at`, `updated_at`
- **Methods**: `addEarning()`, `requestWithdrawal()`, `approveWithdrawal()`, `deductAmount()`

### WalletTransaction
Stores all wallet movements.
- **Attributes**: `id`, `wallet_id`, `trip_session_id`, `transaction_type`, `amount`, `net_profit`, `status`, `created_at`
- **Methods**: `recordEarning()`, `recordWithdrawal()`, `recordAdjustment()`

### WithdrawalRequest
Stores companion withdrawal requests.
- **Attributes**: `id`, `wallet_id`, `amount`, `bank_name`, `iban`, `status`, `reviewed_by_admin_id`, `reviewed_at`, `created_at`
- **Methods**: `submitWithdrawal()`, `approveWithdrawal()`, `rejectWithdrawal()`, `transferFunds()`

### Rating
Stores user ratings and comments after a trip.
- **Attributes**: `id`, `request_id`, `trip_session_id`, `user_id`, `companion_id`, `stars`, `comment`, `created_at`
- **Methods**: `submitRating()`, `updateRating()`, `deleteRating()`

### SupportTicket
Stores complaints, suggestions, and support requests.
- **Attributes**: `id`, `user_id`, `request_id`, `ticket_type`, `subject`, `message`, `status`, `created_at`, `updated_at`
- **Methods**: `createTicket()`, `addMessage()`, `changeStatus()`, `closeTicket()`

### AdminAction
Stores administrative actions on users, companions, withdrawals, documents, and requests.
- **Attributes**: `id`, `admin_user_id`, `target_type`, `target_id`, `action_type`, `notes`, `created_at`
- **Methods**: `logAction()`, `addNotes()`, `linkTarget()`

### AdminReview
Stores companion review records handled by the admin.
- **Attributes**: `id`, `companion_id`, `review_status`, `reviewed_by_admin_id`, `review_note`, `reviewed_at`, `created_at`
- **Methods**: `approveCompanion()`, `rejectCompanion()`, `recordReview()`

### AuditLog
Stores critical system audit records.
- **Attributes**: `id`, `actor_user_id`, `action`, `entity_type`, `entity_id`, `metadata`, `created_at`
- **Methods**: `recordEvent()`, `appendMetadata()`, `traceAction()`

### Chat
Represents a real-time chat thread in Firebase.
- **Attributes**: `id`, `request_id`, `user_id`, `companion_id`, `status`, `last_message`, `last_message_at`, `last_message_sender_id`, `user_unread_count`, `companion_unread_count`, `created_at`, `updated_at`
- **Methods**: `createChat()`, `closeChat()`, `markAsRead()`, `updateLastMessage()`

### ChatMessage
Represents a single message in a Firebase chat thread.
- **Attributes**: `id`, `chat_id`, `sender_id`, `message_type`, `message_text`, `attachment_url`, `is_read`, `read_at`, `created_at`
- **Methods**: `sendMessage()`, `markAsRead()`, `attachFile()`

### LiveTracking
Represents the companion’s live location updates during an active trip.
- **Attributes**: `id`, `request_id`, `trip_session_id`, `companion_id`, `latitude`, `longitude`, `speed`, `heading`, `updated_at`
- **Methods**: `publishLocation()`, `stopTracking()`, `updateRoute()`

### Presence
Tracks whether a user is online or offline in real time.
- **Attributes**: `user_id`, `status`, `last_seen_at`, `updated_at`
- **Methods**: `setOnline()`, `setOffline()`, `updateLastSeen()`

### Notification
Stores real-time notifications.
- **Attributes**: `id`, `user_id`, `title`, `body`, `notification_type`, `related_type`, `related_id`, `is_read`, `read_at`, `created_at`
- **Methods**: `pushNotification()`, `markAsRead()`, `archiveNotification()`

### SupportChat
Represents a real-time support conversation.
- **Attributes**: `id`, `ticket_id`, `sender_id`, `message_text`, `attachment_url`, `is_read`, `created_at`
- **Methods**: `sendSupportMessage()`, `markAsRead()`, `attachSupportFile()`

