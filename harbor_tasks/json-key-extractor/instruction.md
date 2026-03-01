# JSON Server Inventory Report

Read the input file at `/app/input.json`. It contains a JSON array of server inventory records.

Each server record has the following fields:

- `hostname` (string): server hostname
- `server_ip` (string): IP address
- `role` (string): server role (e.g., "frontend", "database", "backend", "cache", "monitoring")
- `datacenter` (string): datacenter location
- `status` (string): "active" or "maintenance"
- `config` (object):
  - `ports` (array of integers): open ports
  - `ssl_enabled` (boolean): whether SSL is enabled
  - `max_connections` (integer): maximum connection limit
- `metrics` (object):
  - `cpu_usage_percent` (number): CPU usage percentage
  - `memory_usage_percent` (number): memory usage percentage
  - `disk_usage_percent` (number): disk usage percentage
  - `uptime_hours` (integer): uptime in hours
- `tags` (array of strings): classification tags

## Task

Process the JSON data and generate a plain-text report at `/app/output.txt` with the following sections in order:

1. **Header**: The text `SERVER INVENTORY REPORT` on the first line, followed by a line of exactly 23 `=` characters.
2. **Server Counts**: `Total Servers: <count>` and `Active Servers: <count>` where active means the `status` field equals `"active"`.
3. **Roles**: `Roles: <roles>` where roles is a comma-and-space-separated list of all unique roles sorted alphabetically.
4. **Critical Servers**: A heading `Critical Servers:` followed by one line per server whose `tags` array contains `"critical"`, sorted by hostname, formatted as `- <hostname> (<server_ip>)`.
5. **Resource Leaders**:
   - `Highest CPU Usage: <hostname> at <value>%` for the server with the highest `cpu_usage_percent`.
   - `Highest Memory Usage: <hostname> at <value>%` for the server with the highest `memory_usage_percent`.
   - `Longest Uptime: <hostname> at <value> hours` for the server with the highest `uptime_hours`.
6. **Datacenter Summary**: A heading `Datacenter Summary:` followed by one line per datacenter sorted alphabetically, formatted as `- <datacenter>: <count> servers`.
7. **SSL Disabled Servers**: A heading `SSL Disabled Servers:` followed by one line per server where `ssl_enabled` is `false`, sorted by hostname, formatted as `- <hostname> (<server_ip>)`.

## Output Format

- Separate each section with exactly one blank line.
- Numeric values must preserve their original precision from the input JSON (e.g., `72.5` not `72.50`).
- The file must end with a trailing newline.
