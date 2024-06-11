# Integration of Samhita with LOS Platform

## Overview

To integrate Samhita with the LOS platform for seamless user onboarding and auto-login, we need to establish a secure API-based communication between the two systems. This document outlines the necessary API endpoints and the integration flow.

## Single Sign-On (SSO) Flow

1. **User logs in to Samhita.**
2. **User clicks on the LOS icon.**
3. **Samhita sends a request to LOS to either onboard the user (if not already onboarded) or log them in (if already onboarded).**
4. **LOS processes the request and either creates a new user or authenticates an existing user.**
5. **LOS returns a token or session information which is then used to auto-login the user to the LOS platform.**

## Proposed API Endpoints on LOS Platform

### 1. Check User Existence API

**Endpoint:** `POST /api/check_user`

**Request Parameters:**
- `mobile_number`: The mobile number of the user (unique identifier).

**Response:**
- `exists`: Boolean indicating if the user exists.
- `user_id`: The user ID in LOS (if exists).

**Example Request:**
```json
{
    "mobile_number": "9988774455"
}
```

**Example Response:**
```json
{
    "exists": true,
    "user_id": "los_user_123"
}
```

### 2. Onboard User API

**Endpoint:** `POST /api/onboard_user`

**Request Parameters:**
- `mobile_number`: The mobile number of the user.
- `name`: The name of the user.
- Other necessary onboarding details.

**Response:**
- `user_id`: The newly created user ID in LOS.

**Example Request:**
```json
{
    "mobile_number": "9988774455",
    "name": "test user",
    "email": "test_user@yopmail.com"
}
```

**Example Response:**
```json
{
    "user_id": "los_user_124"
}
```

### 3. Generate SSO Token API

**Endpoint:** `POST /api/generate_sso_token`

**Request Parameters:**
- `user_id`: The user ID in LOS.

**Response:**
- `sso_token`: The token to be used for SSO.

**Example Request:**
```json
{
    "user_id": "los_user_124"
}
```

**Example Response:**
```json
{
    "sso_token": "abc123xyz789"
}
```

## Implementation Flow in Samhita

### 1. User Clicks on LOS Icon
- Capture the event when the user clicks on the LOS icon.
- Retrieve the logged-in user's mobile number from the session.

### 2. Check User Existence
- Call the `Check User Existence API` with the user's mobile number.
- If the user exists, proceed to generate an SSO token.
- If the user does not exist, call the `Onboard User API`.

### 3. Generate SSO Token
- After getting the user ID (either from the check or onboarding step), call the `Generate SSO Token API`.

### 4. Redirect to LOS with SSO Token
- Redirect the user to the LOS platform with the SSO token, typically as a query parameter in the URL.

### Sample Redirection URL

Once the SSO token is obtained, the user can be redirected to the LOS platform using a URL formatted as follows:

```
https://losplatform.com/login?sso_token=abc123xyz789
```
