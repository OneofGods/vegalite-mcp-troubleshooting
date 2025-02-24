# Vega-Lite MCP Server Troubleshooting Guide

## Key Findings

### Environment Configuration

- **Multiple Server Implementations**: We discovered there were two different Vega-Lite server implementations in different directories:
  - `/Users/Roger/Projects/mcp-vegalite-server` - Using stdio-based communication
  - `/Users/Roger/Projects/vegalite-new` - Using websocket-based communication

- **Claude Configuration**: Claude Desktop is configured to use the websocket-based server in `/Users/Roger/Projects/vegalite-new` but was encountering issues with starting or connecting to it.

- **Import Error**: The logs showed an error where the server code was trying to import `run_stdio_server` from the `mcp` package, but that function was actually named `stdio_server`.

### Websocket Connection Issues

- The websocket server was encountering handshake errors when clients tried to connect
- Error message: `EOFError: connection closed while reading HTTP request line`
- This suggested that connections were being terminated prematurely during the handshake process

### Solution Approaches

1. **Code Fix Option**: We fixed the import statement in `/Users/Roger/Projects/mcp-vegalite-server/src/mcp_server_vegalite/stdio_bridge.py` to use the correct function name:
   ```python
   from mcp import stdio_server
   ```

2. **Process Management**: The recommended approach for stable operation is:
   - Ensure Claude Desktop is completely closed
   - Kill any processes using ports 5100 and 5101
   - Remove existing connection.json files
   - Start the server manually using `./run_port_5100.sh`
   - Verify it's running with `./check_claude_connection.sh`
   - Only then start Claude Desktop

## Lessons Learned

1. **Project Directory Structure**: Having multiple implementations in different directories caused confusion. Consider consolidating or clearly documenting the differences.

2. **API Compatibility**: The server code was importing a function that had changed names in the MCP package (`run_stdio_server` → `stdio_server`). Always check API compatibility when updating dependencies.

3. **Configuration Management**: The critical clue was in the `claude_desktop_config.json` file, which showed which implementation Claude was actually trying to use.

4. **Process Coordination**: Properly sequencing the startup and shutdown of Claude and the server was essential to prevent port conflicts and connection issues.

## Next Steps

1. Consider cleaning up the project structure to have a single, clear implementation
2. Document the correct startup sequence for future reference
3. Set up automated tests to catch API compatibility issues earlier
