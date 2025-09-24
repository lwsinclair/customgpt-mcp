# 🎯 Claude Code Setup Guide for CustomGPT MCP Server

Complete guide to integrate your CustomGPT MCP Server with Claude Code.

## 🚀 **Quick Setup (3 Steps)**

### **Step 1: Prepare Your Server**
```bash
# 1. Activate your environment
source venv/bin/activate

# 2. Add your CustomGPT API key to .env
echo "CUSTOMGPT_API_KEY=your_actual_api_key_here" >> .env

# 3. Test the server
python -c "from server import mcp; print('✅ Server ready!')"
```

### **Step 2: Configure Claude Code**

Add this to your Claude Code MCP configuration:

```json
{
  "mcpServers": {
    "customgpt": {
      "command": "python",
      "args": ["server.py"],
      "cwd": "/Users/zriyansh/Desktop/Projects/customgpt/customgpt-mcp",
      "env": {
        "PYTHONPATH": "/Users/zriyansh/Desktop/Projects/customgpt/customgpt-mcp",
        "CUSTOMGPT_API_BASE": "https://app.customgpt.ai"
      }
    }
  }
}
```

**Configuration file location:**
- **macOS**: `~/.config/claude-code/mcp_servers.json`
- **Windows**: `%APPDATA%\claude-code\mcp_servers.json`
- **Linux**: `~/.config/claude-code/mcp_servers.json`

### **Step 3: Test in Claude Code**

1. **Restart Claude Code** after adding the configuration
2. **Verify connection**: Look for "CustomGPT MCP Server" in available tools
3. **Test a tool**: Try `validate_api_key` with your API key

## 🛠️ **Available Tools in Claude Code**

### **🤖 Agent Management**
- `list_agents` - Browse all your CustomGPT agents
- `get_agent` - Get detailed agent information
- `create_agent` - Create new agents from sitemaps
- `delete_agent` - Remove agents
- `replicate_agent` - Clone/duplicate agents

### **💬 Conversations**
- `send_message` - Chat with your agents (standard)
- `send_streaming_message` - Real-time streaming responses
- `list_conversations` - Browse conversation history

### **📁 File Management**
- `upload_file_to_agent` - Add files to existing agents
- `create_agent_with_files` - Create agent + upload files in one step
- `get_supported_file_types` - Check what file types are supported

### **📦 Batch Operations**
- `bulk_message_agents` - Send same message to multiple agents
- `create_multiple_agents` - Create many agents at once

### **📊 Analytics & Info**
- `get_usage_analytics` - Usage reports and metrics
- `get_server_info` - Server capabilities and status

### **📚 Documentation**
- `search_api_documentation` - Search CustomGPT API docs
- `get_api_endpoint_details` - Get endpoint documentation

### **🔧 Utilities**
- `validate_api_key` - Test your API key

## 💡 **Example Usage in Claude Code**

### **Basic Agent Interaction**
```
Claude Code> I want to list my CustomGPT agents
[Claude Code calls list_agents tool with your API key]

Claude Code> Send a message to agent 123 asking "What services do you offer?"
[Claude Code calls send_message tool]

Claude Code> Create a new agent called "Customer Support" using my company sitemap
[Claude Code calls create_agent tool]
```

### **Advanced Operations**
```
Claude Code> Upload my product documentation PDF to agent 456
[Claude Code calls upload_file_to_agent tool]

Claude Code> Send "What's our refund policy?" to all my customer service agents
[Claude Code calls bulk_message_agents tool]

Claude Code> Create 5 agents for different departments using these configurations...
[Claude Code calls create_multiple_agents tool]
```

## 🔧 **Configuration Options**

### **Environment Variables**
Update your `.env` file:
```env
# Required
CUSTOMGPT_API_KEY=your_api_key_from_customgpt_dashboard

# Optional
CUSTOMGPT_API_BASE=https://app.customgpt.ai
DEBUG=true
```

### **Claude Code MCP Settings**
You can customize the server behavior:

```json
{
  "mcpServers": {
    "customgpt": {
      "command": "python",
      "args": ["server.py"],
      "cwd": "/path/to/customgpt-mcp",
      "env": {
        "PYTHONPATH": "/path/to/customgpt-mcp",
        "CUSTOMGPT_API_BASE": "https://app.customgpt.ai",
        "DEBUG": "false"
      },
      "timeout": 30000,
      "restart": true
    }
  }
}
```

## 🧪 **Testing Your Setup**

### **1. Test Server Directly**
```bash
# Test imports and basic functionality
source venv/bin/activate
python -c "
from server import mcp
print('✅ FastMCP server imported successfully')
print('✅ CustomGPT client available')
print('✅ API documentation loaded')
"
```

### **2. Test in Claude Code**
1. **Open Claude Code**
2. **Check MCP status**: Look for CustomGPT in the MCP servers list
3. **Test basic tool**: Ask Claude to list your agents
4. **Verify API key**: Ask Claude to validate your API key

### **3. Test Specific Features**
```
# Test in Claude Code terminal:
Claude Code> Can you list my CustomGPT agents?
Claude Code> What information can you get about agent 123?
Claude Code> Help me create a new agent for customer support
Claude Code> Send a test message to my agent
```

## 🚨 **Troubleshooting**

### **"Server not found" in Claude Code**
1. ✅ Check file path in configuration is correct
2. ✅ Ensure Python environment has FastMCP installed
3. ✅ Verify `.env` file exists with API key
4. ✅ Restart Claude Code after configuration changes

### **"Authentication failed"**
1. ✅ Get API key from [CustomGPT Dashboard](https://app.customgpt.ai)
2. ✅ Add to `.env` file: `CUSTOMGPT_API_KEY=your_key`
3. ✅ Test with `validate_api_key` tool

### **"Import errors"**
```bash
# Reinstall dependencies
source venv/bin/activate
pip install -r requirements.txt
```

### **"Tool execution failed"**
1. ✅ Check server logs for error details
2. ✅ Verify internet connection to CustomGPT.ai
3. ✅ Ensure API key has proper permissions

## 🎉 **You're Ready!**

Once configured, you can:

✅ **Manage CustomGPT agents** directly from Claude Code
✅ **Send messages** and get real-time responses
✅ **Upload files** to expand agent knowledge
✅ **Batch operations** for efficiency
✅ **Stream responses** for real-time interaction
✅ **Analytics** to track usage and performance

## 📞 **Need Help?**

- **Server Issues**: Check the console logs when running `python server.py`
- **Claude Code Issues**: Check Claude Code's MCP server status
- **API Issues**: Test your API key at [CustomGPT Dashboard](https://app.customgpt.ai)
- **Documentation**: See README.md and DEPLOYMENT.md

---

**🚀 Your CustomGPT agents are now accessible through Claude Code!**