# IBM Watson AI Travel Assistant

A comprehensive travel recommendation system built with IBM Watson AI that provides real-time streaming responses for travel planning queries.

## 🌟 Overview

This project demonstrates how to integrate with IBM Watson AI to create an intelligent travel assistant that can help with:
- Destination recommendations
- Itinerary planning  
- Travel tips and advice
- Local recommendations
- Budget-friendly suggestions

## 🏗️ Architecture

### Core Components

- **Authentication Layer**: IBM Cloud IAM OAuth 2.0 integration
- **Streaming API Handler**: Real-time response processing using Server-Sent Events (SSE)
- **Error Management**: Comprehensive error handling for production use
- **Modular Design**: Reusable functions for easy integration

### Watson AI Integration

- **Deployment**: Uses IBM Watson AI streaming endpoint
- **Region**: US-South (us-south.ml.cloud.ibm.com)
- **Protocol**: HTTPS with Bearer token authentication
- **Response Format**: Server-Sent Events (SSE) with JSON chunks

## 📁 Project Structure

```
venv/
├── travel_ai_notebook.ipynb    # Complete Jupyter notebook implementation
├── travelai_clean.py          # Clean, modular Python script
├── travelai.py               # Original working script
└── README.md                 # This file
```

## 🚀 Quick Start

### Prerequisites

- Python 3.7+
- IBM Cloud account with Watson AI service
- Required Python packages: `requests`, `re`, `json`

### Installation

1. **Clone or download the project files**
   ```bash
   git clone <your-repo-url>
   cd venv
   ```

2. **Install dependencies**
   ```bash
   pip install requests
   ```

3. **Configure your API credentials**
   - Replace `API_KEY` in the configuration with your IBM Cloud API key
   - Verify your Watson AI deployment URL

### Usage

#### Option 1: Run the Jupyter Notebook (Recommended)
```bash
jupyter notebook travel_ai_notebook.ipynb
```

#### Option 2: Run the Python Script
```bash
python travelai_clean.py
```

#### Option 3: Import as Module
```python
from travelai_clean import get_watson_response

response = get_watson_response("Plan a trip to Tokyo")
print(response)
```

## 🔧 Configuration

### API Configuration

```python
# IBM Cloud API Configuration
API_KEY = "your-ibm-cloud-api-key-here"
IAM_URL = 'https://iam.cloud.ibm.com/identity/token'
DEPLOYMENT_URL = 'https://us-south.ml.cloud.ibm.com/ml/v4/deployments/{your-deployment-id}/ai_service_stream?version=2021-05-01'
```

### Required Environment Variables (Production)

```bash
export IBM_CLOUD_API_KEY="your-api-key"
export WATSON_DEPLOYMENT_ID="your-deployment-id"
export WATSON_REGION="us-south"
```

## 💡 Features

### ✅ Core Functionality
- **Real-time Streaming**: See AI responses as they're generated
- **Authentication**: Secure IBM Cloud IAM integration
- **Error Handling**: Comprehensive error management
- **Modular Design**: Easy to integrate and extend

### ✅ Travel Assistant Capabilities
- Destination planning and recommendations
- Itinerary creation (multi-day trips)
- Travel tips and packing advice
- Local recommendations (restaurants, attractions)
- Budget-friendly travel options

### ✅ Technical Features
- Server-Sent Events (SSE) parsing
- JSON response chunk assembly
- Token-based authentication
- Production-ready error handling

## 📚 Usage Examples

### Basic Travel Query
```python
response = get_watson_response("I'm planning a trip to Paris. Can you suggest popular tourist attractions?")
```

### Itinerary Planning
```python
response = get_watson_response("Create a 3-day itinerary for Rome including must-see attractions")
```

### Travel Tips
```python
response = get_watson_response("What should I pack for a winter trip to Iceland?")
```

### Local Recommendations
```python
response = get_watson_response("Best authentic street food in Bangkok?")
```

## 🔐 Authentication Flow

1. **API Key**: Your IBM Cloud API key
2. **Token Request**: POST to IBM IAM endpoint
3. **Access Token**: Temporary token (expires in 3600 seconds)
4. **Bearer Auth**: Token used in Watson AI requests

```python
# Authentication process
token_response = requests.post(IAM_URL, data={
    "apikey": API_KEY,
    "grant_type": 'urn:ibm:params:oauth:grant-type:apikey'
})
access_token = token_response.json()["access_token"]
```

## 📡 API Response Format

Watson AI returns streaming responses in SSE format:

```
id: 1
event: message
data: {"choices": [{"index": 0, "delta": {"role": "assistant", "content": "Hello"}}]}

id: 2
event: message  
data: {"choices": [{"index": 0, "delta": {"role": "assistant", "content": " there"}}]}
```

The code parses these chunks and assembles the complete response.

## 🛠️ Technical Details

### Streaming Response Parsing

```python
# Extract data lines from SSE format
data_lines = re.findall(r'data: (.+)', response_text)

# Parse each JSON chunk
for line in data_lines:
    data = json.loads(line)
    content = data['choices'][0]['delta'].get('content', '')
    full_response += content
```

### Error Handling

```python
error_messages = {
    400: "Bad Request - Check your message format",
    401: "Unauthorized - Verify your API key", 
    403: "Forbidden - Check your permissions",
    404: "Not Found - Verify deployment URL",
    429: "Rate Limited - Wait before making more requests",
    500: "Internal Server Error - Try again later",
    503: "Service Unavailable - Watson AI may be down"
}
```

## 🧪 Testing

### Test Categories Covered

1. **Destination Planning**
   - Family vacation recommendations
   - Beach and food destinations
   - Seasonal travel timing

2. **Itinerary Creation**
   - Multi-day trip planning
   - Budget-friendly options
   - City-specific itineraries

3. **Travel Tips**
   - Packing recommendations
   - Family travel advice
   - Solo travel safety

4. **Local Recommendations**
   - Authentic dining experiences
   - Hidden gems and local culture
   - Cultural experiences

### Running Tests

Execute the notebook cells in order, or run:
```python
# Test basic functionality
response = travel_ai_assistant("Plan a trip to Tokyo")

# Test multiple query types
test_multiple_queries()
```

## 🚨 Error Codes & Troubleshooting

| Code | Issue | Solution |
|------|-------|----------|
| 401 | Authentication failed | Check API key validity |
| 404 | Deployment not found | Verify deployment URL |
| 429 | Rate limited | Wait and retry |
| 500 | Server error | Retry after delay |

### Common Issues

1. **Authentication Failures**
   - Verify API key is correct
   - Check if key has proper permissions
   - Ensure no extra spaces in key

2. **Network Issues**
   - Check internet connectivity
   - Verify firewall settings
   - Try different network

3. **Response Parsing Issues**
   - Check if deployment supports streaming
   - Verify API version compatibility

## 🔄 Production Considerations

### Security
- Store API keys in environment variables
- Use secrets management systems
- Implement request rate limiting
- Add input validation and sanitization

### Performance
- Implement response caching
- Add connection pooling
- Use async requests for multiple queries
- Monitor token expiration

### Monitoring
- Log API usage and errors
- Track response times
- Monitor token refresh cycles
- Set up alerting for failures

## 🚀 Enhancement Ideas

### Immediate Improvements
- [ ] Add conversation history support
- [ ] Implement token refresh mechanism
- [ ] Add response caching
- [ ] Create web interface (Flask/FastAPI)

### Advanced Features
- [ ] Multi-language support
- [ ] Image integration for destinations
- [ ] Travel booking integration
- [ ] Real-time pricing data
- [ ] Weather integration
- [ ] Map visualization

### Integration Options
- [ ] Slack/Discord bot
- [ ] WhatsApp integration
- [ ] Voice assistant compatibility
- [ ] Mobile app backend
- [ ] Chrome extension

## 📄 License

This project is open source. Feel free to use, modify, and distribute.

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## 📞 Support

For issues and questions:
- Check the troubleshooting section
- Review IBM Watson AI documentation
- Create an issue in the repository

## �‍💻 Author

**Arshdeep Yadav**  <br>
BTech Computer Science Engineering <br>

[![GitHub](https://img.shields.io/badge/GitHub-arshdeepyadavofficial-black?style=flat&logo=github)](https://github.com/arshdeepyadavofficial)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-arshdeepyadav-blue?style=flat&logo=linkedin)](https://linkedin.com/in/arshdeepyadav)

## �🙏 Acknowledgments

- IBM Watson AI team for the excellent API
- IBM Cloud documentation and examples
- Open source community for inspiration

---

**Made with ❤️ for travelers worldwide** 🌍✈️
