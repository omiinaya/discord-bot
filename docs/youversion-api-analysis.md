# YouVersion Bible API Analysis - Verse of the Day Implementation

## Overview

This document analyzes the YouVersion Bible API client implementation to understand how the "Verse of the Day" (VOTD) functionality works, enabling us to create our own implementation without using the existing package.

## Key Findings

### Authentication Requirements
- **OAuth2 Authentication**: Requires username/password credentials
- **Client Credentials**: Uses base64-encoded client ID and secret
- **API Endpoint**: `https://auth.youversionapi.com/token`

### Verse of the Day API Endpoint
- **URL**: `https://nodejs.bible.com/api/moments/votd/3.1`
- **Method**: GET
- **Authentication**: Bearer token required
- **Response Format**: JSON containing VOTD data for multiple days

### API Response Structure
The VOTD endpoint returns an array of verse objects with the following structure:
```json
{
  "votd": [
    {
      "day": 346,
      "image_id": "12345",
      "usfm": ["GEN.1.1"]
    }
  ]
}
```

### Required Headers
```python
DEFAULT_HEADERS = {
    "Referer": "http://android.youversionapi.com/",
    "X-YouVersion-App-Platform": "android",
    "X-YouVersion-App-Version": "17114",
    "X-YouVersion-Client": "youversion",
    "Authorization": "Bearer {token}"
}
```

## Implementation Flow

1. **Authentication**:
   - POST to `/token` endpoint with credentials
   - Receive OAuth2 access token
   - Extract user_id from JWT token

2. **VOTD Request**:
   - GET request to VOTD endpoint with Bearer token
   - Response contains array of verses for different days

3. **Day Selection**:
   - Default: Current day of year (1-365/366)
   - Optional: Specific day number parameter

4. **Data Processing**:
   - Filter response for requested day
   - Fallback to first available verse if day not found

## Authentication Details

### OAuth2 Token Request
```python
data = {
    "client_id": base64.b64decode("ODViNjFkOTdhNzliOTZiZTQ2NWViYWVlZTgzYjEzMTM=").decode(),
    "client_secret": base64.b64decode("NzVjZjBlMTQxY2JmNDFlZjQxMGFkY2U1YjY1MzdhNDk=").decode(),
    "grant_type": "password",
    "username": "your_username",
    "password": "your_password"
}
```

### Client Credentials (Decoded)
- **Client ID**: `85b61d97a79b96be465ebaeee83b1313`
- **Client Secret**: `75cf0e141cbf41ef410adce5b6537a49`

## Simplified Implementation Strategy

Since we don't want to use the full package, we can create a minimal implementation that:

1. **Handles authentication** using the same OAuth2 flow
2. **Makes direct HTTP requests** to the VOTD endpoint
3. **Processes the response** to extract the verse data
4. **Integrates with Discord bot** as a new command

## Key Files from Original Implementation

- [`config.py`](../youversion-analysis/youversion/config.py): Contains all API endpoints and configuration
- [`authenticator.py`](../youversion-analysis/youversion/core/authenticator.py): Handles OAuth2 authentication
- [`http_client.py`](../youversion-analysis/youversion/core/http_client.py): Makes HTTP requests
- [`base_client.py`](../youversion-analysis/youversion/core/base_client.py): Main client implementation
- [`data_processor.py`](../youversion-analysis/youversion/core/data_processor.py): Processes API responses

## Next Steps

1. Create a simplified authentication module
2. Implement direct VOTD API calls
3. Add error handling and fallback mechanisms
4. Integrate with existing Discord bot structure
5. Create the `votd` command with proper Discord formatting