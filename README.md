# Linear MCP Server

> Note: This is a custom implementation. For the official Cline Linear MCP server, see [cline/linear-mcp](https://github.com/cline/linear-mcp).

> **Fork Information**: This repository is a fork of [@ibraheem4/linear-mcp](https://github.com/ibraheem4/linear-mcp). texmeijin has extended it based on practical work experience, adding features such as label management to enable more flexible and detailed operations. While maintaining the original functionality, additional features have been added to make daily task management more efficient.

<a href="https://glama.ai/mcp/servers/71fqw0uqmx"> <img width="380" height="200" src="https://glama.ai/mcp/servers/71fqw0uqmx/badge" />

A Model Context Protocol (MCP) server that provides tools for interacting with Linear's API, enabling AI agents to manage issues, projects, and teams programmatically through the Linear platform.

## Features

- **Issue Management**

  - Create new issues with customizable properties (title, description, team, assignee, priority, labels)
  - List issues with flexible filtering options (team, assignee, status)
  - Update existing issues (title, description, status, assignee, priority)

- **Team Management**

  - List all teams in the workspace
  - Access team details including ID, name, key, and description

- **Project Management**
  - List all projects with optional team filtering
  - View project details including name, description, state, and associated teams

- **Label Management**
  - List all labels associated with a team
  - Create new labels with custom name, color, and description
  - Update existing labels to modify their properties

## Prerequisites

- Node.js (v16 or higher)
- A Linear account with API access
- Linear API key with appropriate permissions

## Quick Start

1. Get your Linear API key from [Linear's Developer Settings](https://linear.app/settings/api)

2. Clone the repository and build it locally:

```bash
git clone https://github.com/texmeijin/linear-enhanced-mcp.git
cd linear-enhanced-mcp
npm install
npm run build
```

3. Run with your API key:

```bash
export LINEAR_API_KEY=your-api-key-here
node build/index.js
```

Note: NPX installation is planned for future releases.

## Development Setup

1. Clone the repository:

```bash
git clone [repository-url]
cd linear-mcp
```

2. Install dependencies:

```bash
npm install
```

3. Build the project:

```bash
npm run build
```

## Running with Inspector

For local development and debugging, you can use the MCP Inspector:

1. Install the MCP Inspector:

```bash
npm install -g @modelcontextprotocol/inspector
```

2. Run the server with the inspector:

```bash
LINEAR_API_KEY=your-api-key-here npx @modelcontextprotocol/inspector node build/index.js
```

3. Access the Inspector:
   - Open [localhost:1337](http://localhost:1337) in your browser
   - The Inspector connects via Server-Sent Events (SSE)
   - Test and debug tool calls through the Inspector interface

## Configuration

Configure the MCP server in your settings file based on your client:

### For Claude Desktop

- MacOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%/Claude/claude_desktop_config.json`

```json
{
  "mcpServers": {
    "linear-enhanced-mcp": {
      "command": "node",
      "args": ["/path/to/linear-enhanced-mcp/build/index.js"],
      "env": {
        "LINEAR_API_KEY": "your-api-key-here"
      },
      "disabled": false,
      "alwaysAllow": []
    }
  }
}
```

### For VS Code Extension (Cline)

Location: `~/Library/Application Support/Code/User/globalStorage/rooveterinaryinc.roo-cline/settings/cline_mcp_settings.json`

```json
{
  "mcpServers": {
    "linear-enhanced-mcp": {
      "command": "node",
      "args": ["/path/to/linear-enhanced-mcp/build/index.js"],
      "env": {
        "LINEAR_API_KEY": "your-api-key-here"
      },
      "disabled": false,
      "alwaysAllow": []
    }
  }
}
```

### For Cursor ([cursor.sh](https://cursor.sh))

For Cursor, the server must be run with the full path:

```bash
node /path/to/linear-enhanced-mcp/build/index.js
```

## Available Tools

### create_issue

Creates a new issue in Linear.

```typescript
{
  title: string;          // Required: Issue title
  description?: string;   // Optional: Issue description (markdown supported)
  teamId: string;        // Required: Team ID
  assigneeId?: string;   // Optional: Assignee user ID
  priority?: number;     // Optional: Priority (0-4)
  labels?: string[];     // Optional: Label IDs to apply
}
```

### list_issues

Lists issues with optional filters.

```typescript
{
  teamId?: string;      // Optional: Filter by team ID
  assigneeId?: string;  // Optional: Filter by assignee ID
  status?: string;      // Optional: Filter by status
  first?: number;       // Optional: Number of issues to return (default: 50)
}
```

### update_issue

Updates an existing issue.

```typescript
{
  issueId: string;       // Required: Issue ID
  title?: string;        // Optional: New title
  description?: string;  // Optional: New description
  status?: string;      // Optional: New status
  assigneeId?: string;  // Optional: New assignee ID
  priority?: number;    // Optional: New priority (0-4)
  labels?: string[];   // Optional: Label IDs to apply to the issue
}
```

### list_labels

Lists all labels associated with a team.

```typescript
{
  teamId: string;      // Required: Team ID
}
```

### create_label

Creates a new label for a team.

```typescript
{
  teamId: string;      // Required: Team ID
  name: string;        // Required: Label name
  color: string;       // Required: Label color (hex color code)
  description?: string; // Optional: Label description
}
```

### update_label

Updates an existing label.

```typescript
{
  id: string;          // Required: Label ID
  name?: string;       // Optional: New label name
  color?: string;      // Optional: New color (hex color code)
  description?: string; // Optional: New description
}
```

### list_teams

Lists all teams in the workspace. No parameters required.

### list_projects

Lists all projects with optional filtering.

```typescript
{
  teamId?: string;     // Optional: Filter by team ID
  first?: number;      // Optional: Number of projects to return (default: 50)
}
```

### get_issue

Gets detailed information about a specific issue.

```typescript
{
  issueId: string; // Required: Issue ID
}
```

## Development

For development with auto-rebuild:

```bash
npm run watch
```

## Error Handling

The server includes comprehensive error handling for:

- Invalid API keys
- Missing required parameters
- Linear API errors
- Invalid tool requests

All errors are properly formatted and returned with descriptive messages.

## Technical Details

Built with:

- TypeScript
- Linear SDK (@linear/sdk v37.0.0)
- MCP SDK (@modelcontextprotocol/sdk v0.6.0)

The server uses stdio for communication and implements the Model Context Protocol for seamless integration with AI agents.

## License

MIT
