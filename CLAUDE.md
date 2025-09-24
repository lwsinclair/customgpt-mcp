# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a comprehensive **CustomGPT MCP Server** built with **FastMCP 2.0** that provides complete access to CustomGPT.ai APIs through the Model Context Protocol. The server implements **49 tools** covering all CustomGPT API endpoints for agent management, conversations, analytics, and content control.

## Core Architecture

### Main Components
- **`server.py`** - Primary MCP server with 49 comprehensive tools using FastMCP framework
- **`requirements.txt`** - Dependencies: `fastmcp`, `customgpt-client>=1.2.8`, `python-dotenv`, etc.
- **`.env`** - Environment configuration (API keys, server settings)
- **`docs/`** - Complete CustomGPT API documentation including OpenAPI spec

### Key Design Patterns

**FastMCP Tool Structure:**
```python
@mcp.tool()
def tool_name(param: type) -> Dict[str, Any]:
    """Tool description."""
    response = CustomGPT.Class.method(param)
    response_data = extract_response_data(response)  # Critical for serialization
    return {"success": True, "data": response_data}
```

**Response Handling:**
All CustomGPT SDK methods return `Response` objects that must be extracted to JSON using `extract_response_data(response)` before returning to avoid serialization errors.

**API Key Management:**
The server pre-configures CustomGPT client on startup from environment variables, eliminating the need for API key parameters in individual tools.

## Development Commands

### Environment Setup
```bash
# Create Python 3.11+ virtual environment (required for FastMCP)
python3.11 -m venv venv
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Configure API key
cp .env.example .env
# Add: CUSTOMGPT_API_KEY=your_key_here
```

### Development Server
```bash
# Run MCP server for Claude Code integration
python server.py

# Test server imports and configuration
python -c "from server import mcp; print('✅ Server ready')"

# Run with environment variables
CUSTOMGPT_API_KEY=your_key python server.py
```

### Build and Deploy
```bash
# Using Makefile
make install          # Setup environment and dependencies
make setup           # Interactive configuration
make dev             # Development server
make deploy-railway  # Deploy to Railway
make docker          # Build and run with Docker

# Direct deployment
railway up           # Railway deployment
vercel --prod        # Vercel deployment
docker-compose up -d # Docker deployment
```

## CustomGPT API Integration

### SDK Integration Pattern
The server uses the official `customgpt-client` SDK with these key classes:
- `CustomGPT.Project` - Agent management (list, create, get, update, delete, replicate, stats)
- `CustomGPT.Conversation` - Conversation handling (create, send, get, update, delete, messages)
- `CustomGPT.Page` - Page management (get, delete, reindex, preview)
- `CustomGPT.Source` - Source management (list, create, update, delete, synchronize)
- `CustomGPT.ProjectSettings` - Agent configuration (get, update)
- `CustomGPT.ReportsAnalytics` - Analytics (traffic, queries, conversations, analysis)
- `CustomGPT.User` - User management (get, update)

### Critical Response Processing
**All CustomGPT SDK responses must be processed with:**
```python
response_data = extract_response_data(response)
```
This extracts JSON from Response objects for proper MCP serialization.

### API Key Configuration
Set `CUSTOMGPT_API_KEY` in `.env` or environment. The server automatically configures the CustomGPT client on startup, so individual tools don't need API key parameters.

## MCP Server Tool Categories

The server provides **49 comprehensive tools** organized into:

- **Agents (7):** Complete agent lifecycle management
- **Conversations (7):** Full conversation and messaging control
- **Messages (2):** Individual message management and feedback
- **Pages (6):** Page content and metadata management
- **Sources (5):** Source and content management with sync
- **Settings (2):** Agent configuration and customization
- **Licenses (5):** License management (limited by SDK)
- **Plugins (3):** Plugin creation and management
- **Reports (5):** Analytics, traffic, queries, intelligence
- **Citations (1):** Citation metadata retrieval
- **User (3):** User profile and team member management
- **Limits (1):** Account usage and quota monitoring
- **Utilities (2):** Server info and API validation

## Claude Code Integration

### MCP Configuration
Add to Claude Code MCP settings:
```json
{
  "customgpt-server": {
    "command": "/path/to/venv/bin/python",
    "args": ["/path/to/server.py"],
    "env": {
      "PYTHONPATH": "/path/to/project",
      "CUSTOMGPT_API_BASE": "https://app.customgpt.ai",
      "CUSTOMGPT_API_KEY": "your_api_key"
    }
  }
}
```

### Usage Examples
- "List my CustomGPT agents"
- "Send a message to agent 123 asking about pricing"
- "Get conversation history for agent 456"
- "Update agent settings to use Claude model"
- "Get traffic analytics for my support agent"

## Important Implementation Details

### Error Handling Strategy
Tools implement comprehensive error handling with:
- Custom exception catching for CustomGPT API errors
- Stderr logging for Claude Code visibility
- Graceful degradation for SDK limitations
- Clear error messages with context

### Environment Variable Usage
- `CUSTOMGPT_API_KEY` - Required CustomGPT API key
- `CUSTOMGPT_API_BASE` - API base URL (default: https://app.customgpt.ai)
- `PORT` - Server port for hosted deployment
- `DEBUG` - Enable debug logging

### Deployment Modes
The server automatically detects deployment environment:
- **Local development:** stdio mode for Claude Code
- **Hosted deployment:** HTTP mode when PORT environment variable is set
- **Production:** Uses environment variables for configuration

## File Structure Significance

- **`server.py`** - Main MCP server with all 49 tools
- **`docs/openapi.json`** - Complete CustomGPT API specification loaded at startup
- **`.env`** - Local environment configuration (not tracked in git)
- **`claude_code_config.json`** - Example Claude Code MCP configuration
- **Deployment configs** - `railway.json`, `docker-compose.yml`, `vercel.json` for various hosting platforms

## CustomGPT SDK Limitations

Some API endpoints require custom implementation as the `customgpt-client` SDK doesn't expose all methods:
- License management endpoints (tools provide helpful error messages)
- Team member search (requires custom API calls)
- Some advanced settings (handled gracefully with parameter validation)

The server transparently handles these limitations by providing clear error messages and suggesting alternatives when SDK methods are unavailable.