# HP Environment MCP Server

An MCP server providing productivity and organization tools for AI agent integration. The server exposes 35 tools for managing reminders, documents, calendar events, tasks, and more.

## Capabilities

- **Productivity Tools**: Create and manage documents, presentations, spreadsheets, images, emails
- **Organization Tools**: Reminders, calendar events, task management (Asana/Microsoft/Google)
- **Information Services**: Web search, technical support, PC settings, weather
- **Utilities**: Content summarization, URL summarization, meeting assistance

---

## Connecting to the Server

### Base URL

```
http://3.151.215.26:8007
```

### Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/health` | GET | Health check |
| `/tools` | GET | List all available tools with input schemas |
| `/mcp/tools/{tool_name}` | POST | Execute a tool |

### Example: Create a Reminder

```bash
curl -X POST http://3.151.215.26:8007/mcp/tools/create_reminder \
  -H "Content-Type: application/json" \
  -d '{
    "user_id": "user_2",
    "name": "Team standup",
    "hint": "tomorrow at 9am"
  }'
```

### Example: List Available Tools

```bash
curl http://3.151.215.26:8007/tools
```

---

## Available Tools

| Category | Tools | Description |
|----------|-------|-------------|
| **Reminders** | `create_reminder`, `list_reminders`, `update_reminder`, `delete_reminder` | Personal reminder management |
| **Documents** | `create_document`, `update_document` | Document creation and editing |
| **Presentations** | `create_presentation`, `update_presentation` | Slide deck management |
| **Spreadsheets** | `create_spreadsheet`, `update_spreadsheet` | Spreadsheet creation and editing |
| **Images** | `create_image`, `update_image` | Image generation from text prompts |
| **Emails** | `create_email`, `update_email` | Email draft composition |
| **Calendar** | `create_event`, `list_events`, `update_event`, `delete_event` | Event scheduling with attendees |
| **Tasks** | `create_task`, `list_tasks`, `update_task`, `delete_task` | Task tracking (Asana/Microsoft/Google) |
| **Information** | `web_search`, `tech_help`, `pc_settings`, `weather` | Search and support |
| **Utilities** | `summarize`, `summarize_url`, `join_meeting`, `fallback` | Helper functions |
| **Environment** | `get_environment_stats`, `get_user_data`, `get_database`, `save_environment_state`, `reset_environment` | System management |

---

## Tool Reference

### Reminders

#### create_reminder

Creates a new reminder.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `user_id` | string | Yes | User identifier |
| `name` | string | No | Reminder title |
| `due` | string | No | Due date in ISO 8601 format |
| `hint` | string | No | Natural language timing (e.g., "tomorrow morning") |

**Request:**
```json
{
  "user_id": "user_2",
  "name": "Call the dentist",
  "hint": "tomorrow afternoon"
}
```

**Response:**
```json
{
  "status": "success",
  "message": "Reminder created successfully",
  "reminder": {
    "id": "reminder_a1b2c3",
    "name": "Call the dentist",
    "due": "2025-01-21T14:00:00Z"
  }
}
```

#### list_reminders

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `user_id` | string | Yes | User identifier |

#### update_reminder

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `user_id` | string | Yes | User identifier |
| `reminder_id` | string | Yes | Reminder to update |
| `name` | string | No | New title |
| `due` | string | No | New due date |
| `hint` | string | No | New timing hint |

#### delete_reminder

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `user_id` | string | Yes | User identifier |
| `reminder_id` | string | Yes | Reminder to delete |

---

### Documents

#### create_document

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `user_id` | string | Yes | User identifier |
| `title` | string | No | Document title |
| `content` | string | No | Document body (supports markdown) |

**Request:**
```json
{
  "user_id": "user_2",
  "title": "Meeting Notes",
  "content": "## Attendees\n- Alice\n- Bob\n\n## Action Items\n1. Review proposal"
}
```

#### update_document

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `user_id` | string | Yes | User identifier |
| `document_id` | string | Yes | Document to update |
| `title` | string | No | New title |
| `content` | string | No | New content |

---

### Presentations

#### create_presentation

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `user_id` | string | Yes | User identifier |
| `title` | string | No | Presentation title |

#### update_presentation

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `user_id` | string | Yes | User identifier |
| `presentation_id` | string | Yes | Presentation to update |
| `title` | string | No | New title |
| `slides` | array | No | Array of slide objects |

**Slides format:**
```json
{
  "slides": [
    {"slide_number": 1, "title": "Agenda", "content": "Topics..."},
    {"slide_number": 2, "title": "Overview", "content": "Summary..."}
  ]
}
```

---

### Spreadsheets

#### create_spreadsheet

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `user_id` | string | Yes | User identifier |
| `title` | string | No | Spreadsheet title |

#### update_spreadsheet

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `user_id` | string | Yes | User identifier |
| `spreadsheet_id` | string | Yes | Spreadsheet to update |
| `title` | string | No | New title |
| `sheets` | array | No | Array of sheet objects |

**Sheets format:**
```json
{
  "sheets": [
    {
      "name": "Budget",
      "data": [
        ["Category", "Amount"],
        ["Marketing", "$5000"]
      ]
    }
  ]
}
```

---

### Images

#### create_image

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `user_id` | string | Yes | User identifier |
| `prompt` | string | No | Image generation prompt |
| `title` | string | No | Image title |

**Request:**
```json
{
  "user_id": "user_2",
  "prompt": "Modern office workspace with dual monitors",
  "title": "Office Setup"
}
```

#### update_image

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `user_id` | string | Yes | User identifier |
| `image_id` | string | Yes | Image to update |
| `prompt` | string | No | New generation prompt |
| `title` | string | No | New title |

---

### Emails

#### create_email

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `user_id` | string | Yes | User identifier |
| `to` | array | No | Recipient email addresses |
| `subject` | string | No | Email subject |
| `body` | string | No | Email body |
| `cc` | array | No | CC recipients |
| `bcc` | array | No | BCC recipients |

**Request:**
```json
{
  "user_id": "user_2",
  "to": ["alice@example.com"],
  "subject": "Project Update",
  "body": "Hi Team,\n\nHere's the latest update...",
  "cc": ["manager@example.com"]
}
```

#### update_email

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `user_id` | string | Yes | User identifier |
| `email_id` | string | Yes | Email to update |
| `to` | array | No | New recipients |
| `subject` | string | No | New subject |
| `body` | string | No | New body |
| `cc` | array | No | New CC list |
| `bcc` | array | No | New BCC list |

---

### Calendar Events

#### create_event

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `user_id` | string | Yes | User identifier |
| `name` | string | No | Event name |
| `start` | string | No | Start time (ISO 8601) |
| `end` | string | No | End time (ISO 8601) |
| `hint` | string | No | Natural language timing |
| `attendees` | array | No | List of `{name, email}` objects |
| `provider` | string | No | `"Microsoft"` (default), `"Google"`, `"Asana"` |

**Request:**
```json
{
  "user_id": "user_2",
  "name": "Sprint Planning",
  "start": "2025-01-22T10:00:00Z",
  "end": "2025-01-22T11:30:00Z",
  "attendees": [
    {"name": "Alice Johnson", "email": "alice@example.com"},
    {"name": "Bob Smith", "email": "bob@example.com"}
  ],
  "provider": "Microsoft"
}
```

#### list_events

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `user_id` | string | Yes | User identifier |
| `start` | string | No | Filter: range start |
| `end` | string | No | Filter: range end |
| `hint` | string | No | Natural language filter |
| `provider` | string | No | Filter by provider |

#### update_event

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `user_id` | string | Yes | User identifier |
| `event_id` | string | Yes | Event to update |
| `name` | string | No | New name |
| `start` | string | No | New start time |
| `end` | string | No | New end time |
| `hint` | string | No | New timing hint |
| `attendees` | array | No | New attendee list |

#### delete_event

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `user_id` | string | Yes | User identifier |
| `event_id` | string | Yes | Event to delete |

---

### Tasks

#### create_task

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `user_id` | string | Yes | User identifier |
| `name` | string | No | Task name |
| `due` | string | No | Due date (ISO 8601) |
| `hint` | string | No | Natural language timing |
| `provider` | string | No | `"Asana"` (default), `"Microsoft"`, `"Google"` |

**Request:**
```json
{
  "user_id": "user_2",
  "name": "Review pull request #42",
  "hint": "end of day today",
  "provider": "Asana"
}
```

#### list_tasks

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `user_id` | string | Yes | User identifier |
| `provider` | string | No | Filter by provider |

#### update_task

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `user_id` | string | Yes | User identifier |
| `task_id` | string | Yes | Task to update |
| `name` | string | No | New name |
| `due` | string | No | New due date |
| `hint` | string | No | New timing hint |
| `completed` | boolean | No | Mark as completed |

#### delete_task

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `user_id` | string | Yes | User identifier |
| `task_id` | string | Yes | Task to delete |

---

### Information & Support

#### web_search

Searches for current information on news, sports, or stocks.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `type` | string | No | `"News"`, `"Sports"`, or `"Stocks"` |
| `prompt` | string | No | Search query |

#### tech_help

Technical support for HP, Poly, and Omen products.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `prompt` | string | No | Support question |

#### pc_settings

Executes PC settings commands.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `prompt` | string | No | Settings command (e.g., "increase brightness to 80%") |

#### weather

Retrieves weather information.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `location` | string | No | Location name |
| `hint` | string | No | Timing context (e.g., "this weekend") |

---

### Utilities

#### summarize

Summarizes provided text content.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `content` | string | No | Text to summarize |

#### summarize_url

Summarizes content from a URL.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `url` | string | No | URL to fetch and summarize |

#### join_meeting

Handles meeting join requests.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | No | Meeting name |
| `location` | string | No | Meeting location |

#### fallback

Handles general queries that don't fit other tools.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `prompt` | string | No | User query |

---

### Environment Management

#### get_environment_stats

Returns counts of all entity types in the system. No parameters required.

#### get_user_data

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `user_id` | string | Yes | User identifier |

#### get_database

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `include` | array | No | Top-level keys to include (e.g., `["users"]`) |

#### save_environment_state

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `filepath` | string | No | Path to save state file |

#### reset_environment

Resets the environment to initial state. No parameters required.

---

## Key Concepts

### User ID

Most tools require a `user_id` parameter. The server validates that the user exists before performing operations. Placeholder values (`"unknown"`, `"null"`, `"none"`, etc.) are rejected.

### Date/Time Format

Use ISO 8601 format:
```
2025-01-20T10:00:00Z     # January 20, 2025 at 10:00 AM UTC
2025-01-20T14:30:00Z     # January 20, 2025 at 2:30 PM UTC
```

### Natural Language Hints

The `hint` parameter accepts natural language timing:
- `"tomorrow morning"` → Next day at 9:00 AM
- `"next Friday at 3pm"` → Upcoming Friday at 3:00 PM
- `"end of day"` → Today at 5:00 PM
- `"in 2 hours"` → 2 hours from now

When both explicit date and `hint` are provided, the explicit date takes precedence.

### Providers

Calendar and task tools support multiple providers:

| Provider | Description |
|----------|-------------|
| `"Microsoft"` | Microsoft 365 / Outlook (default for calendar) |
| `"Google"` | Google Calendar / Google Tasks |
| `"Asana"` | Asana (default for tasks) |

---

## Response Format

**Success:**
```json
{
  "status": "success",
  "message": "Operation completed successfully",
  "data": { ... }
}
```

**Error:**
```json
{
  "status": "error",
  "message": "Description of the error"
}
```

### Common Error Codes

| HTTP Code | Error | Cause |
|-----------|-------|-------|
| 400 | `"Invalid user_id"` | User ID is empty or a placeholder value |
| 400 | `"Unknown user_id: xxx"` | User does not exist |
| 415 | `"Content-Type must be application/json"` | Missing or incorrect Content-Type header |
| 422 | `"Missing required field(s): ..."` | Required parameters not provided |
| 404 | `"Unknown tool 'xxx'"` | Tool name does not exist |
