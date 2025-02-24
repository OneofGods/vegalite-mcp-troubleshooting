# Common Errors and Solutions

This document catalogs common errors encountered when working with the Vega-Lite MCP server and their solutions.

## Import Errors

### `ImportError: cannot import name 'run_stdio_server' from 'mcp'`

This error occurs because the server code is trying to import a function that doesn't exist or has been renamed in the MCP package.

**Solution:**
- Edit the file that's trying to import `run_stdio_server` and change it to `stdio_server`
- Example:
  ```python
  # Change this
  from mcp import run_stdio_server, Response, initialize, visualize
  
  # To this
  from mcp import stdio_server, Response, initialize, visualize
  ```
- Also update any function calls from `run_stdio_server()` to `stdio_server()`

## WebSocket Connection Errors

### `EOFError: connection closed while reading HTTP request line`

This error indicates that a client tried to connect to the WebSocket server, but the connection was closed before the HTTP handshake could complete.

**Solution:**
- Make sure no other processes are using the same port (5100)
- Ensure Claude Desktop is completely closed before starting the server
- Check that the server is properly initialized before starting Claude
- Follow the [recommended startup sequence](startup_sequence.md)

### `websockets.exceptions.InvalidMessage: did not receive a valid HTTP request`

This is similar to the above error and indicates a problem with the WebSocket handshake.

**Solution:**
- Same as for the EOF error above
- Additionally, check that the WebSocket protocol versions match between client and server

## Python Command Not Found

### `zsh: command not found: python`

This occurs when trying to run a command with `python` but the Python executable isn't in the system PATH.

**Solution:**
- Use `python3` instead of `python`
- If using Claude Desktop, update the `claude_desktop_config.json` file to use `python3` instead of `python`
- Example:
  ```json
  "datavis": {
    "command": "python3",  // Changed from "python"
    "args": [
      "-m",
      "src.mcp_server_vegalite"
    ],
    ...
  }
  ```

## Port Conflicts

### `OSError: [Errno 48] Address already in use`

This error occurs when trying to start the server on a port that's already in use.

**Solution:**
- Kill any processes using the port:
  ```bash
  lsof -ti:5100 | xargs kill -9
  ```
- Make sure Claude Desktop is not running and trying to use the same port
- Try using a different port if necessary by setting the `SERVER_PORT` environment variable

## Connection File Issues

### `FileNotFoundError: connection.json not found`

Claude or the server cannot find the required connection file.

**Solution:**
- Make sure the server has successfully started and created the connection.json file
- If the file exists but Claude can't find it, check the file permissions
- Verify the file is in the expected location (usually the same directory as the server)