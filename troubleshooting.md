# Vega-Lite MCP Server Troubleshooting

## Problem Summary
We encountered issues connecting Claude to a Vega-Lite server for data visualization. The primary issue was a port conflict on port 5100 and configuration mismatches between what Claude expected and what our server provided.

## Current Status
- Created a new Vega-Lite server implementation running on port 5100
- Modified the server to properly use environment variables for configuration
- Updated Claude's configuration to correctly point to our server
- Set up enhanced logging for better debugging
- Created diagnostic tools to help identify connection issues

## Troubleshooting Steps Taken

### 1. Initial Diagnosis
- Found that Claude was attempting to use port 5100 while our server was running on port 5101
- Discovered import errors in the original server code (`run_stdio_server` vs `stdio_server`)
- Identified configuration mismatches in Claude's `claude_desktop_config.json`

### 2. Server Implementation
- Created a new implementation of the Vega-Lite server using WebSockets
- Set up proper error handling and logging
- Added signal handlers for graceful shutdown
- Implemented connection information storage in `connection.json`

### 3. Configuration Fixes
- Updated the server to use environment variables for port configuration
- Modified Claude's configuration to correctly point to our server
- Created initialization scripts for both port 5100 and port 5101
- Set up proper directory structure for logs

### 4. Diagnostic Tools
- Created a `check_claude_connection.sh` script to diagnose server connection issues
- Added enhanced logging to both standard logs and Claude-specific logs
- Created test scripts to verify functionality

## Next Steps
1. Ensure server starts correctly on port 5100
2. Verify Claude can connect to the server
3. Test data visualization functionality
4. Create documentation for future maintenance

## Key Files and Locations

### Server Files
- Server implementation: `/Users/Roger/Projects/vegalite-new/src/mcp_server_vegalite/server.py`
- Run script (port 5100): `/Users/Roger/Projects/vegalite-new/run_port_5100.sh`
- Run script (port 5101): `/Users/Roger/Projects/vegalite-new/test_run.sh`
- Diagnostic tool: `/Users/Roger/Projects/vegalite-new/check_claude_connection.sh`

### Claude Configuration
- Claude config: `/Users/Roger/Library/Application Support/Claude/claude_desktop_config.json`

### Log Files
- Server logs: `/Users/Roger/Projects/vegalite-new/logs/custom-vegalite.log`
- Claude integration logs: `/Users/Roger/Projects/vegalite-new/claude_logs/claude-integration.log`
- Claude system logs: `/Users/Roger/Library/Logs/Claude/mcp-server-datavis.log`

## Commands for Common Tasks

### Start Server on Port 5100
```bash
cd /Users/Roger/Projects/vegalite-new
chmod +x run_port_5100.sh
./run_port_5100.sh
```

### Check Connection Status
```bash
cd /Users/Roger/Projects/vegalite-new
chmod +x check_claude_connection.sh
./check_claude_connection.sh
```

### Stop Server Processes
```bash
lsof -ti:5100 | xargs kill -9
lsof -ti:5101 | xargs kill -9
```

### Restart Claude Desktop
- Close Claude completely (File > Quit or Cmd+Q)
- Wait a few seconds
- Reopen Claude

## Known Issues
- Original server had an import error (`run_stdio_server` vs `stdio_server`)
- Connection.json location wasn't properly recognized
- Port conflicts continue to be a potential issue
- Claude expects a stdio-based server but we're using a WebSocket-based server

## Contributors
- Claude 3.7 Sonnet
- Roger (User)