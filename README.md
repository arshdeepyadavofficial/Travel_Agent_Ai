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
Travel_Agent_Ai/
├── travel_ai_notebook.ipynb    # Complete Jupyter notebook implementation
├── arsh.env                  # Your API key file (create this)
└── README.md                 # This file
```

**File Descriptions:**
- **travel_ai_notebook.ipynb**: Interactive notebook with step-by-step implementation and detailed explanations
- **arsh.env**: Your API key configuration file (you need to create this)

## 🚀 Quick Start

### Prerequisites

- Python 3.7+
- IBM Cloud account with Watson AI service
- Required Python packages: `requests`, `re`, `json`

### Installation

1. **Clone or download the project files**
   ```bash
   git clone https://github.com/arshdeepyadavofficial/Travel_Agent_Ai
   cd Travel_Agent_Ai
   ```

2. **Install dependencies**
   ```bash
   pip install requests python-dotenv
   ```

3. **Set up your IBM Cloud API key**
   
   Create a `.env` file in the project root directory and add your IBM Cloud API key:
   ```bash
   API_KEY=your_actual_ibm_cloud_api_key_here
   ```
   
   **How to get your IBM Cloud API key:**
   1. Go to [IBM Cloud Console](https://cloud.ibm.com/)
   2. Navigate to **Manage** → **Access (IAM)** → **API Keys**
   3. Click **Create** to generate a new API key
   4. Copy the key and paste it in your `.env` file
   
   **Important Notes:**
   - Never share your API key publicly
   - Add `.env` to your `.gitignore` file
   - The key should be without quotes or extra spaces

### Usage

#### Option 1: Run the Jupyter Notebook (Recommended)
1. **Set up your API key** (see Configuration section above)
2. **Start Jupyter Notebook**
   ```bash
   jupyter notebook travel_ai_notebook.ipynb
   ```
3. **Run the cells in order** - the notebook includes detailed explanations for each step

#### Option 2: Run as Python Script (if available)
```bash
# Make sure your .env file is set up first
python travel_ai_notebook.py  # If you convert the notebook to .py
```

## 🔧 Configuration

### Getting Your IBM Cloud API Key

1. **Sign up/Login to IBM Cloud**
   - Visit [IBM Cloud](https://cloud.ibm.com/)
   - Create an account or login

2. **Create Watson AI Service**
   - Go to **Catalog** → **AI/Machine Learning** → **Watson Studio**
   - Create a service instance

3. **Generate API Key**
   - Navigate to **Manage** → **Access (IAM)** → **API Keys**
   - Click **Create** button
   - Give it a name (e.g., "Travel AI Assistant")
   - Copy the generated API key

4. **Create Environment File**
   - Create a `.env` file in your project directory
   - Add your API key:
   ```bash
   API_KEY=your_actual_ibm_cloud_api_key_here
   ```

### Alternative Configuration Methods

**Method 1: Custom Environment File (arsh.env)**
```bash
API_KEY=your_ibm_cloud_api_key_here
```

**Method 2: System Environment Variables**
```bash
export API_KEY="your_ibm_cloud_api_key_here"
```

### API Configuration Example

```python
# The code automatically loads from .env or arsh.env files
# Your .env file should contain:
API_KEY=abcd1234-efgh-5678-ijkl-9012mnopqrst

# The deployment URL is already configured in the code
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

Once your API key is configured, you can use the travel assistant for various queries:

### Basic Travel Query
```python
# Ask about destinations
travel_ai_assistant("I'm planning a trip to Paris. Can you suggest popular tourist attractions?")
```

### Itinerary Planning
```python
# Multi-day trip planning
travel_ai_assistant("Create a 3-day itinerary for Rome including must-see attractions")
```

### Travel Tips
```python
# Packing and preparation advice
travel_ai_assistant("What should I pack for a winter trip to Iceland?")
```

### Local Recommendations
```python
# Local experiences and food
travel_ai_assistant("Best authentic street food in Bangkok?")
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

1. **"❌ API key is missing" Error**
   - Ensure your `.env` or `arsh.env` file exists in the project directory
   - Check that the file contains: `API_KEY=your_actual_key_here`
   - Make sure there are no extra spaces around the equals sign
   - Verify the API key is valid and not expired

2. **Authentication Failures**
   - Double-check your API key from IBM Cloud console
   - Ensure the API key has proper permissions for Watson AI
   - Try regenerating the API key if it's old

3. **File Not Found Issues**
   - Make sure you're running the notebook from the correct directory
   - Check that `.env` or `arsh.env` file is in the same folder as the notebook
   - Use absolute paths if needed

4. **Network Issues**
   - Check internet connectivity
   - Verify firewall settings allow HTTPS requests
   - Try different network if corporate firewall blocks requests

5. **Response Parsing Issues**
   - Check if your Watson deployment supports streaming
   - Verify API version compatibility
   - Ensure deployment URL is correct

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
