# Coder

A coder agent for feature development and code reviews

**Version:** 0.1.0

## Overview

This A2A (Agent-to-Agent) server was generated using the A2A CLI from an Agent Definition Language (ADL) file. This agent uses the inference gateway A2A framework with AI-powered capabilities.

### Capabilities
- **Streaming:** true
- **Push Notifications:** true
- **State Transition History:** true

### Available Tools
- **read_file** - Read the contents of a file
- **write_file** - Write or create a file with given content
- **list_directory** - List contents of a directory
- **search_code** - Search for code patterns or text in files
- **execute_command** - Execute a shell command
- **git_operations** - Perform git operations like status, diff, commit, etc.

## Getting Started

### Prerequisites

- Go 1.24+
- [Task](https://taskfile.dev/) (optional, for using Taskfile commands)

### Environment Variables

Common A2A configuration:
- `A2A_SERVER_PORT`: Server port (default: 8443)
- `A2A_DEBUG`: Enable debug mode (default: false)
- `A2A_AGENT_SYSTEM_PROMPT`: Override the system prompt
- `A2A_AGENT_MODEL`: AI model to use

### Running the Server

#### Using Task (recommended)

```bash
# Install dependencies
go mod tidy

# Run in development mode
task run

# Build the binary
task build

# Run tests
task test
```

#### Using Go directly

```bash
# Run directly
go run .

# Build and run
go build -o bin/coder .
./bin/coder
```

#### Using Docker

```bash
# Build Docker image
task docker-build

# Run container
task docker-run
```

#### Kubernetes Deployment

For production deployment using the [Inference Gateway Operator](https://github.com/inference-gateway/operator):

```bash
# Install the Inference Gateway Operator (if not already installed)
kubectl apply -f https://github.com/inference-gateway/operator/releases/latest/download/install.yaml

# Apply A2A Custom Resource
kubectl apply -f k8s/a2a-server.yaml

# Check A2A status
kubectl get a2a coder -n coder-ns

# View operator-managed deployment
kubectl get pods -n coder-ns

# Port forward for testing
kubectl port-forward svc/coder 8443:80 -n coder-ns
```

The operator automatically manages deployment, scaling, health checks, and configuration.

## Development

### Project Structure

```
.
├── main.go              # Main server setup
├── tools/               # Tool implementations
│   ├── read_file.go   # Read the contents of a file
│   ├── write_file.go   # Write or create a file with given content
│   ├── list_directory.go   # List contents of a directory
│   ├── search_code.go   # Search for code patterns or text in files
│   ├── execute_command.go   # Execute a shell command
│   ├── git_operations.go   # Perform git operations like status, diff, commit, etc.
├── go.mod               # Go module definition
├── .well-known/
│   └── agent.json       # Agent capabilities
├── Taskfile.yml         # Task definitions
├── Dockerfile           # Container configuration
├── k8s/
│   └── a2a-server.yaml  # Kubernetes deployment
└── README.md            # This file
```

### Implementing Tools

Each tool is implemented in its own file under the `tools/` directory. Tools follow this pattern:

```go
func NewToolNameTool() server.Tool {
    return server.NewBasicTool(
        "tool_name",
        "Tool description",
        schema,
        toolNameHandler,
    )
}

func toolNameHandler(ctx context.Context, args map[string]interface{}) (string, error) {
    // Your implementation here
    return result, nil
}
```

### Testing

Add tests for your tool implementations:

```bash
# Run tests
go test -v ./...

# Run tests with coverage
go test -v -cover ./...
```

### Example Usage

```bash
# Test the agent capabilities
curl http://localhost:8443/.well-known/agent.json
```

## API Endpoints

Once running, the server exposes:

- `GET /.well-known/agent.json` - Agent capabilities and metadata

## Generated Files

This project was generated with:
- **CLI Version:** 0.4.3
- **Template:** minimal
- **Generated At:** 2025-07-23 22:26:14

To regenerate with ADL changes:
```bash
a2a generate --file agent.yaml --output . --overwrite
```

---

> 🤖 This server is powered by the [A2A (Agent-to-Agent) framework](https://github.com/inference-gateway/a2a)
