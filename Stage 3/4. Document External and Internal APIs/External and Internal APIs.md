# External and Internal APIs

## External APIs

### 1) Moyasar Payments API
**Why it was chosen:** It supports payment charging, refunds, and webhook confirmations, which matches the project’s requirement to collect the full trip payment before sending the request to the companion.

**Main usage:**
- Create payment charges
- Confirm payment status
- Handle refund flows
- Receive webhook events

### 2) Firebase Firestore / Realtime API
**Why it was chosen:** It supports real-time chat, live tracking, presence updates, notifications, and support chat, which are all time-sensitive parts of the platform.

**Main usage:**
- Store chat threads and messages
- Store live tracking updates
- Store online/offline presence
- Push notifications
- Store support chat messages

### 3) Firebase Storage API
**Why it was chosen:** It is suitable for storing companion documents, profile images, car documents, and chat attachments in a secure file storage layer.

**Main usage:**
- Upload companion documents
- Upload profile images
- Upload car-related documents
- Upload message attachments

### 4) Google Maps Platform API
**Why it was chosen:** The project needs map display, location selection, and live trip tracking, so a maps provider is required for geocoding and map visualization.

**Main usage:**
- Display pickup and destination maps
- Convert addresses to coordinates
- Show companion location on the map
- Support routing and location lookup

---

## Internal APIs

### 1) Auth APIs

#### POST /api/v1/auth/register
- **Input format:** JSON
- **Input:** `first_name`, `last_name`, `mobile`, `email`, `password`, `confirm_password`, `consent_type`
- **Output format:** JSON
- **Output:** account creation status, user data, verification state

#### POST /api/v1/auth/login
- **Input format:** JSON
- **Input:** `mobile` or `email`, `password`
- **Output format:** JSON
- **Output:** token, user data, account status

#### POST /api/v1/auth/verify-otp
- **Input format:** JSON
- **Input:** `user_id`, `channel`, `otp_code`
- **Output format:** JSON
- **Output:** verification result

### 2) User APIs

#### GET /api/v1/users/me
- **Input format:** None
- **Output format:** JSON
- **Output:** current user profile

#### PUT /api/v1/users/me
- **Input format:** JSON
- **Input:** profile update fields
- **Output format:** JSON
- **Output:** updated profile

### 3) Companion APIs

#### POST /api/v1/companions/profile
- **Input format:** JSON
- **Input:** `gender`, `nationality`, `date_of_birth`, `city`, `district`, `short_description`, `experience_summary`, `health_status`, `has_car`
- **Output format:** JSON
- **Output:** companion profile status

#### POST /api/v1/companions/documents
- **Input format:** JSON
- **Input:** `document_type`, `file_url`
- **Output format:** JSON
- **Output:** document upload result

#### POST /api/v1/companions/vehicle
- **Input format:** JSON
- **Input:** `car_type`, `car_model`, `plate_number`
- **Output format:** JSON
- **Output:** vehicle data saved result

#### GET /api/v1/companions/dashboard
- **Input format:** None
- **Output format:** JSON
- **Output:** dashboard summary, nearby requests, earnings, ratings, connection status

#### PATCH /api/v1/companions/availability
- **Input format:** JSON
- **Input:** `connection_status`, `work_status`
- **Output format:** JSON
- **Output:** availability update result

### 4) Request APIs

#### POST /api/v1/requests
- **Input format:** JSON
- **Input:** `request_for`, `service_type`, `request_mode`, `with_car`, `scheduled_at`
- **Output format:** JSON
- **Output:** request data and current status

#### POST /api/v1/requests/:request_id/beneficiary
- **Input format:** JSON
- **Input:** `full_name`, `age`, `mobile`, `gender`, `relationship`
- **Output format:** JSON
- **Output:** beneficiary saved result

#### POST /api/v1/requests/:request_id/health-data
- **Input format:** JSON
- **Input:** `health_condition`, `other_text`
- **Output format:** JSON
- **Output:** health data saved result

#### POST /api/v1/requests/:request_id/location
- **Input format:** JSON
- **Input:** pickup and destination address/coordinates
- **Output format:** JSON
- **Output:** location saved result

#### GET /api/v1/requests/:request_id
- **Input format:** None
- **Output format:** JSON
- **Output:** full request details

#### POST /api/v1/requests/:request_id/assign-companion
- **Input format:** JSON
- **Input:** `companion_id`
- **Output format:** JSON
- **Output:** assignment result

#### POST /api/v1/requests/:request_id/cancel
- **Input format:** JSON
- **Input:** `reason`
- **Output format:** JSON
- **Output:** cancellation result

### 5) Payment APIs

#### POST /api/v1/payments/checkout
- **Input format:** JSON
- **Input:** `request_id`, `amount`
- **Output format:** JSON
- **Output:** payment checkout payload and transaction details

#### POST /api/v1/payments/webhook/moyasar
- **Input format:** JSON
- **Input:** Moyasar webhook payload
- **Output format:** JSON
- **Output:** webhook processing status

#### POST /api/v1/payments/:payment_id/refund
- **Input format:** JSON
- **Input:** `reason`
- **Output format:** JSON
- **Output:** refund result

### 6) Trip APIs

#### POST /api/v1/trips/:request_id/start
- **Input format:** JSON
- **Input:** `companion_id`
- **Output format:** JSON
- **Output:** trip session start result

#### POST /api/v1/trips/:request_id/end
- **Input format:** JSON
- **Input:** `trip_session_id`
- **Output format:** JSON
- **Output:** trip end result

#### POST /api/v1/trips/:request_id/extend
- **Input format:** JSON
- **Input:** `extra_hours`
- **Output format:** JSON
- **Output:** extension result

### 7) Rating APIs

#### POST /api/v1/ratings
- **Input format:** JSON
- **Input:** `request_id`, `trip_session_id`, `companion_id`, `stars`, `comment`
- **Output format:** JSON
- **Output:** rating result

### 8) Support APIs

#### POST /api/v1/support/tickets
- **Input format:** JSON
- **Input:** `ticket_type`, `subject`, `message`
- **Output format:** JSON
- **Output:** support ticket result

#### GET /api/v1/support/tickets/:ticket_id
- **Input format:** None
- **Output format:** JSON
- **Output:** support ticket details and messages

### 9) Admin APIs

#### GET /api/v1/admin/companions/pending
- **Input format:** None
- **Output format:** JSON
- **Output:** list of companions waiting for review

#### POST /api/v1/admin/companions/:user_id/approve
- **Input format:** JSON
- **Input:** `notes`
- **Output format:** JSON
- **Output:** approval result

#### POST /api/v1/admin/companions/:user_id/reject
- **Input format:** JSON
- **Input:** `reason`
- **Output format:** JSON
- **Output:** rejection result

#### POST /api/v1/admin/withdrawals/:withdrawal_id/approve
- **Input format:** JSON
- **Input:** `notes`
- **Output format:** JSON
- **Output:** withdrawal approval result

#### POST /api/v1/admin/withdrawals/:withdrawal_id/reject
- **Input format:** JSON
- **Input:** `reason`
- **Output format:** JSON
- **Output:** withdrawal rejection result
