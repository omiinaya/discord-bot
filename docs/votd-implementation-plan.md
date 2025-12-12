# Verse of the Day (VOTD) Implementation Plan

## Overview
Create a Discord bot command that fetches and displays the YouVersion Verse of the Day without using the existing Python package.

## Implementation Steps

### Phase 1: Authentication Module
1. **Create authentication helper**
   - Implement OAuth2 token retrieval
   - Handle credential management (environment variables)
   - Add token caching/refresh logic

2. **Create API configuration**
   - Extract required API endpoints and headers
   - Define constants for client credentials

### Phase 2: VOTD API Client
1. **Create minimal HTTP client**
   - Handle authenticated requests
   - Implement proper error handling
   - Add timeout and retry logic

2. **Implement VOTD fetching**
   - Create function to get verse of the day
   - Support optional day parameter
   - Add fallback mechanisms

### Phase 3: Verse Processing
1. **Create verse data model**
   - Define structure for verse information
   - Add methods for formatting output

2. **Implement verse lookup**
   - Fetch actual verse text from Bible reference
   - Support different Bible versions

### Phase 4: Discord Integration
1. **Create votd command**
   - Add to existing MyCog class
   - Implement proper Discord command structure
   - Add error handling and user feedback

2. **Enhance output formatting**
   - Create attractive Discord message format
   - Add optional parameters (day, version)
   - Include verse reference and text

## Technical Specifications

### Authentication Flow
```python
# Pseudo-code
1. Check for cached token
2. If expired or missing:
   a. POST to auth endpoint with credentials
   b. Extract token from response
   c. Cache token with expiration
3. Return Bearer token for API calls
```

### VOTD API Call
```python
# Pseudo-code
1. Make GET request to VOTD endpoint with Bearer token
2. Parse response JSON
3. Find verse for requested day (default: current day)
4. Return verse data structure
```

### Verse Text Lookup
```python
# Pseudo-code
1. Extract USFM reference from VOTD response
2. Make API call to Bible chapter endpoint
3. Parse verse text from chapter content
4. Return formatted verse text
```

## File Structure
```
faithup-discord-bot/
├── youversion/
│   ├── __init__.py
│   ├── auth.py          # Authentication module
│   ├── client.py        # API client
│   └── models.py        # Data models
├── mycog.py            # Add votd command
└── docs/
    ├── youversion-api-analysis.md
    └── votd-implementation-plan.md
```

## Dependencies
- `requests` - Already in requirements.txt
- `python-dotenv` - Already in requirements.txt  
- `base64` - Standard library
- `datetime` - Standard library

## Error Handling Requirements
- Authentication failures
- Network timeouts
- Invalid day parameters
- API rate limiting
- Missing verse data

## Testing Strategy
1. **Unit tests** for authentication and API client
2. **Integration tests** for full VOTD workflow
3. **Manual testing** with Discord bot

## Success Criteria
- Command responds within 5 seconds
- Properly formatted verse display
- Error handling for common failure scenarios
- Support for optional day parameter