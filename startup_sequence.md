# Recommended Startup Sequence for Vega-Lite MCP Server

This document outlines the recommended procedure for starting the Vega-Lite MCP server and ensuring proper integration with Claude Desktop.

## Prerequisites

- Claude Desktop application installed
- Vega-Lite MCP server code in `/Users/Roger/Projects/vegalite-new`
- Developer Mode enabled in Claude Desktop

## Step-by-Step Startup Sequence

1. **Ensure Claude Desktop is completely closed**
   ```bash
   pkill -f claude
   ```

2. **Kill any processes using relevant ports**
   ```bash
   lsof -ti:5100 | xargs kill -9
   lsof -ti:5101 | xargs kill -9
   ```

3. **Clean up any existing connection files**
   ```bash
   cd /Users/Roger/Projects/vegalite-new
   rm -f connection.json
   ```

4. **Start the server manually**
   ```bash
   ./run_port_5100.sh
   ```

5. **Verify the server is running properly**
   ```bash
   ./check_claude_connection.sh
   ```
   You should see confirmation that:
   - Server is running on port 5100
   - connection.json exists with correct configuration
   - Claude integration log exists

6. **Wait 10 seconds** to ensure the server is stable

7. **Start Claude Desktop**

8. **Test the Vega-Lite server functionality** by asking Claude to create a visualization

## Troubleshooting

If you encounter issues:

1. Check the logs at `/Users/Roger/Library/Logs/Claude/mcp-server-datavis.log`

2. Verify the `claude_desktop_config.json` configuration at:
   `/Users/Roger/Library/Application Support/Claude/claude_desktop_config.json`

3. Ensure the "datavis" configuration points to the correct directory:
   ```json
   "datavis": {
     "command": "python3",
     "args": [
       "-m",
       "src.mcp_server_vegalite"
     ],
     "cwd": "/Users/Roger/Projects/vegalite-new",
     "env": {
       "PYTHONIOENCODING": "utf-8",
       "SERVER_PORT": "5100"
     }
   }
   ```

4. If needed, enable Developer Mode in Claude Desktop via Help > Enable Developer Mode