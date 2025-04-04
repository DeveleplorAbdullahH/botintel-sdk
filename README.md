# BotIntel Python SDK

[![PyPI version](https://img.shields.io/pypi/v/botintel.svg)](https://pypi.org/project/botintel/)
[![Python Versions](https://img.shields.io/pypi/pyversions/botintel.svg)](https://pypi.org/project/botintel/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Documentation](https://img.shields.io/badge/docs-latest-blue)](https://docs.botintel.ai)

The official Python client for BotIntel's enterprise-grade AI API, featuring multi-model support, real-time web integration, and advanced analytics.

```python
# Basic Example
from botintel import Client

client = Client(api_key="your_api_key")
response = client.chat.completions.create(
    model="botintel-v3",
    messages=[{"role": "user", "content": "Latest AI breakthroughs in 2024"}]
)
print(response)
```

## Table of Contents
- [Features](#features)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Available Models](#available-models)
- [Usage Examples](#usage-examples)
- [Error Handling](#error-handling)
- [Advanced Features](#advanced-features)
- [Pricing](#pricing)
- [Configuration](#configuration)
- [Deployment](#deployment)
- [Contributing](#contributing)
- [License](#license)
- [Support](#support)

## Features <a name="features"></a>
🌐 Web-enhanced responses with real-time data

🧮 Advanced token accounting system

⚡ Streaming support with partial responses

🔒 Enterprise-grade security (SOC 2 compliant)

📈 Built-in usage analytics

♻️ Automatic retries with exponential backoff

## Installation <a name="installation"></a>
```bash
pip install botintel
```

## Quick Start <a name="quick-start"></a>
```python
from botintel import Client

# Initialize with API key
client = Client(api_key="bix-your-api-key-here")

# Basic chat completion
response = client.chat.completions.create(
    model="botintel-v3",
    messages=[
        {"role": "system", "content": "You are a senior developer"},
        {"role": "user", "content": "Explain monorepo architecture"}
    ]
)

print(f"Response: {response.choices[0].message.content}")
print(f"Tokens Used: {response.usage.total_tokens}")
```

## Available Models <a name="available-models"></a>

| Model Name         | Context Window | Best For                                       | Web Access | Parameters | Architecture   | System Prompt Support |
|--------------------|----------------|------------------------------------------------|------------|------------|----------------|-----------------------|
| botintel-v3        | 8K tokens      | General purpose AI                             | ❌         | 789B       | Transformer    |          ✅          |
| botintel-pro       | 32K tokens     | Technical problem solving                      | ❌         | N/A       | Transformer    |           ✅          |
| botintel-v3-search | 4K tokens      | Real-time information                          | ✅         | 489B       | Transformer    |          ✅          |
| imagen-1           | N/A            | Basic image generation                         | ❌         | N/A        | Diffusion      |          N/A         |
| imagen-2           | N/A            | Advanced image generation (latest & most improved)| ❌      | N/A        | Diffusion      |          N/A         |

## Usage Examples <a name="usage-examples"></a>
### Web-Enhanced Search
```python
response = client.chat.completions.create(
    model="botintel-v3-search",
    messages=[
        {"role": "user", "content": "Current USD to EUR exchange rate"}
    ],
    web_search=True,
    cite_sources=True
)

print(response.choices[0].message.content)
# Output might include: [Source: European Central Bank, 2024-03-15]
```

### Technical Analysis (Pro Model)
```python
response = client.chat.completions.create(
    model="botintel-pro",
    messages=[
        {"role": "user", "content": "Optimize this Python code..."},
        {"role": "assistant", "content": "Current code: ..."},
    ],
    temperature=0.2,
    max_tokens=1000
)
```

## Error Handling <a name="error-handling"></a>
```python
from botintel import exceptions

try:
    response = client.chat.completions.create(...)
except exceptions.InsufficientBalanceError as e:
    print(f"Payment required: {e.balance_needed}")
    # Auto-recharge logic here
except exceptions.RateLimitError as e:
    print(f"Retry after: {e.retry_after} seconds")
except exceptions.APIError as e:
    print(f"Server error: {e.status_code}")
```

## Advanced Features <a name="advanced-features"></a>
### Streaming Responses
```python
stream = client.chat.completions.create(
    model="botintel-v3",
    messages=[{"role": "user", "content": "Tell me a story..."}],
    stream=True,
    temperature=0.7
)

for chunk in stream:
    print(chunk.choices[0].delta.content, end="", flush=True)
```

### Usage Analytics
```python
# Get current balance
balance = client.get_balance()
print(f"Remaining balance: ${balance:.2f}")

# Get usage statistics
usage = client.get_usage(start_date="2024-01-01")
print(f"Total tokens used: {usage.total_tokens}")
```

## Pricing <a name="pricing"></a>
| Model            | Input ($/1k tokens) | Output ($/1k tokens) |
|-----------------|--------------------|--------------------|
| botintel-v3     | 0.27               | 1.10               |
| botintel-pro    | 0.55               | 2.19               |
| botintel-search | 0.35               | 1.45               |
*Volume discounts available for >1M tokens/month*

## Configuration <a name="configuration"></a>
```python
client = Client(
    api_key="your_key",
    base_url="https://api.botintel.ai/v1",  # Custom endpoint
    timeout=30.0,          # Request timeout in seconds
    max_retries=5,         # Automatic retries
    cache_enabled=True,    # Response caching
    debug=False            # Enable debug logging
)
```

## Deployment <a name="deployment"></a>
### Recommended Architecture
```
                          +-----------------+
                          |   Load Balancer |
                          +--------+--------+
                                   |
                          +--------+--------+
                          |    API Servers   |
                          +--------+--------+
                                   |
                          +--------+--------+
                          |  PostgreSQL DB  |
                          +-----------------+
                          |   Redis Cache   |
                          +-----------------+
```

### Docker Setup
```dockerfile
# Dockerfile.prod
FROM python:3.11-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["gunicorn", "app:create_app()", "--bind", "0.0.0.0:8000", "--workers", "4"]
```

## Contributing <a name="contributing"></a>
- Fork the repository
- Create feature branch (`git checkout -b feature/amazing-feature`)
- Commit changes (`git commit -m 'Add amazing feature'`)
- Push to branch (`git push origin feature/amazing-feature`)
- Open Pull Request

## License <a name="license"></a>
MIT License - See LICENSE for details.

## Support <a name="support"></a>
📧 Enterprise Support: botintelai1@gmail.com

💬 Community: Discord Server

📄 [Documentation](https://botintel-docs.netlify.app)

⬆ Back to Top

Copy

To use this README:

1. Save as `README.md`
2. Update placeholder URLs (status page, docs, etc.)
3. Add your LICENSE file
4. Customize deployment details as needed

The file includes:
- Interactive code examples
- Detailed model comparisons
- Error handling patterns
- Deployment architecture diagrams
- Complete pricing information
- Docker configuration examples
- Support resources
