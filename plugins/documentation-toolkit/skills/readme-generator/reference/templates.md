# README Templates

## Library Template

```markdown
# Library Name

Brief description of what the library does and why it's useful.

## Installation

\`\`\`bash
npm install library-name
\`\`\`

## Quick Start

\`\`\`javascript
const lib = require('library-name');

// Basic usage
const result = lib.doSomething();
\`\`\`

## API Reference

### `functionName(param1, param2)`

Description of what the function does.

**Parameters:**
- `param1` (string): Description
- `param2` (number): Description

**Returns:** Description of return value

**Example:**
\`\`\`javascript
const result = functionName('value', 42);
\`\`\`

## Examples

### Example 1: Common Use Case

\`\`\`javascript
// Code example
\`\`\`

## Contributing

Contributions welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT © [Author Name](https://github.com/author)
```

## CLI Tool Template

```markdown
# CLI Tool Name

Description of what the CLI tool does.

## Installation

\`\`\`bash
npm install -g cli-tool-name
# or
pip install cli-tool-name
\`\`\`

## Usage

\`\`\`bash
cli-tool-name [command] [options]
\`\`\`

## Commands

### `init`

Initialize a new project.

\`\`\`bash
cli-tool-name init my-project
\`\`\`

### `build`

Build the project.

\`\`\`bash
cli-tool-name build --output dist/
\`\`\`

**Options:**
- `--output, -o`: Output directory (default: dist/)
- `--watch, -w`: Watch for changes

## Configuration

Create a `.toolrc` file:

\`\`\`json
{
  "option1": "value1",
  "option2": "value2"
}
\`\`\`

## Examples

### Example 1: Basic Workflow

\`\`\`bash
cli-tool-name init my-app
cd my-app
cli-tool-name build
\`\`\`

## License

MIT
```

## Web Application Template

```markdown
# App Name

Brief description and key features.

![Screenshot](docs/screenshot.png)

## Features

- ✨ Feature 1
- 🚀 Feature 2
- 🔒 Feature 3

## Getting Started

### Prerequisites

- Node.js 18+
- PostgreSQL 14+

### Installation

1. Clone the repository:
\`\`\`bash
git clone https://github.com/user/repo.git
cd repo
\`\`\`

2. Install dependencies:
\`\`\`bash
npm install
\`\`\`

3. Set up environment:
\`\`\`bash
cp .env.example .env
# Edit .env with your configuration
\`\`\`

4. Run migrations:
\`\`\`bash
npm run migrate
\`\`\`

5. Start development server:
\`\`\`bash
npm run dev
\`\`\`

Visit http://localhost:3000

## Deployment

### Docker

\`\`\`bash
docker build -t app-name .
docker run -p 3000:3000 app-name
\`\`\`

### Production

See [DEPLOYMENT.md](docs/DEPLOYMENT.md) for detailed instructions.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md)

## License

MIT
```

## API Template

```markdown
# API Name

RESTful API for [purpose].

## Base URL

\`\`\`
https://api.example.com/v1
\`\`\`

## Authentication

Include API key in header:

\`\`\`bash
curl -H "Authorization: Bearer YOUR_API_KEY" https://api.example.com/v1/endpoint
\`\`\`

## Endpoints

### GET /users

Get list of users.

**Query Parameters:**
- `page` (number): Page number (default: 1)
- `limit` (number): Items per page (default: 20)

**Response:**
\`\`\`json
{
  "users": [
    {"id": 1, "name": "John Doe"}
  ],
  "total": 100,
  "page": 1
}
\`\`\`

### POST /users

Create a new user.

**Request Body:**
\`\`\`json
{
  "name": "Jane Doe",
  "email": "jane@example.com"
}
\`\`\`

**Response:**
\`\`\`json
{
  "id": 2,
  "name": "Jane Doe",
  "email": "jane@example.com"
}
\`\`\`

## Error Handling

\`\`\`json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid email format"
  }
}
\`\`\`

## Rate Limiting

- 1000 requests per hour per API key
- Rate limit info in response headers

## Examples

### JavaScript

\`\`\`javascript
const response = await fetch('https://api.example.com/v1/users', {
  headers: {
    'Authorization': 'Bearer YOUR_API_KEY'
  }
});
const data = await response.json();
\`\`\`

## License

MIT
```
