# MCP (Model Context Protocol) Project

This project demonstrates the Model Context Protocol (MCP) implementation with a client-server architecture. It includes a weather service MCP server and a client that can interact with it using Anthropic's Claude AI model.

## Overview

The Model Context Protocol (MCP) is a standard for AI assistants to communicate with external data sources and tools. This project showcases:

- **MCP Weather Server**: A .NET-based MCP server that provides weather data tools
- **MCP Client**: A .NET client that connects to MCP servers and integrates with Anthropic's Claude AI

## Project Structure

```
MCP Server/
├── QuickstartClient/          # MCP Client application
│   ├── Program.cs            # Main client application
│   └── QuickstartClient.csproj
└── weather/                  # MCP Weather Server
    ├── Program.cs            # Server entry point
    ├── WeatherTools.cs       # Weather service tools
    └── weather.csproj
```

## Features

### Weather Server Tools

The weather server provides two main tools:

1. **GetAlerts**: Retrieves active weather alerts for a US state
   - Parameter: `state` (string) - The US state to get alerts for
   - Returns: Formatted weather alerts with event type, area, severity, and instructions

2. **GetForecast**: Gets weather forecast for a specific location
   - Parameters: 
     - `latitude` (double) - Latitude of the location
     - `longitude` (double) - Longitude of the location
   - Returns: Detailed weather forecast with temperature, wind, and conditions

### MCP Client

The client application:
- Connects to MCP servers via stdio transport
- Integrates with Anthropic's Claude AI model
- Provides an interactive chat interface
- Supports multiple server types (Python, Node.js, .NET)

## Prerequisites

- .NET 9.0 SDK
- Anthropic API key (for the client)

## Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd "MCP Server"
   ```

2. **Set up the Weather Server**
   ```bash
   cd weather
   dotnet restore
   dotnet build
   ```

3. **Set up the Client**
   ```bash
   cd ../QuickstartClient
   dotnet restore
   dotnet build
   ```

4. **Configure API Keys**
   
   For the client, you need to set up your Anthropic API key. You can do this in several ways:

   **Option 1: User Secrets (Recommended)**
   ```bash
   cd QuickstartClient
   dotnet user-secrets set "ANTHROPIC_API_KEY" "your-api-key-here"
   ```

   **Option 2: Environment Variable**
   ```bash
   set ANTHROPIC_API_KEY=your-api-key-here
   ```

## Usage

### Running the Weather Server

1. **Start the weather server**:
   ```bash
   cd weather
   dotnet run
   ```

   The server will start and listen for connections via stdio transport.

### Running the Client

1. **Start the client with the weather server**:
   ```bash
   cd QuickstartClient
   dotnet run ../weather/weather.csproj
   ```

2. **Interactive Chat Interface**
   
   Once started, you'll see:
   ```
   Connected to server with tools: GetAlerts
   Connected to server with tools: GetForecast
   MCP Client Started!
   Enter a command (or 'exit' to quit):
   >
   ```

3. **Example Commands**
   
   You can now interact with the weather tools through natural language:
   - "Get weather alerts for California"
   - "What's the forecast for New York City? Use coordinates 40.7128, -74.0060"
   - "Show me weather alerts for Texas"

### Running with Other Server Types

The client supports different server types:

- **Python servers**: `dotnet run script.py`
- **Node.js servers**: `dotnet run script.js`
- **.NET projects**: `dotnet run project.csproj`

## Architecture

### MCP Server (Weather)

The weather server implements the MCP protocol using:

- **Transport**: StdioServerTransport for communication
- **Tools**: WeatherTools class with annotated methods
- **HTTP Client**: Configured to access weather.gov API

Key components:
- `Program.cs`: Server configuration and startup
- `WeatherTools.cs`: Tool implementations with MCP annotations

### MCP Client

The client implements:

- **Transport**: StdioClientTransport for server communication
- **AI Integration**: Anthropic Claude AI for natural language processing
- **Tool Discovery**: Automatic tool listing and integration
- **Interactive Interface**: Console-based chat interface

## Dependencies

### Weather Server
- `Microsoft.Extensions.Hosting` (9.0.7)
- `ModelContextProtocol` (0.3.0-preview.3)

### Client
- `Anthropic.SDK` (5.4.3)
- `Microsoft.Extensions.AI` (9.7.1)
- `Microsoft.Extensions.Hosting` (9.0.7)
- `ModelContextProtocol` (0.3.0-preview.3)

## API Integration

The weather server integrates with the National Weather Service API (weather.gov) to provide:
- Real-time weather alerts
- Detailed weather forecasts
- Location-based weather data

## Development

### Adding New Tools

To add new tools to the weather server:

1. Create a new method in `WeatherTools.cs`
2. Add the `[McpServerTool]` attribute
3. Add a `[Description]` attribute for the tool
4. Add parameter descriptions using `[Description]` attributes

Example:
```csharp
[McpServerTool, Description("Get current weather conditions.")]
public static async Task<string> GetCurrentWeather(
    HttpClient client,
    [Description("Latitude of the location.")] double latitude,
    [Description("Longitude of the location.")] double longitude)
{
    // Implementation
}
```

### Extending the Client

The client can be extended to:
- Support additional AI models
- Add more sophisticated UI
- Implement tool result visualization
- Add configuration management

## Troubleshooting

### Common Issues

1. **API Key Not Found**
   - Ensure your Anthropic API key is properly configured
   - Check user secrets or environment variables

2. **Server Connection Failed**
   - Verify the server path is correct
   - Ensure the server is built and runnable
   - Check for any compilation errors

3. **Weather API Errors**
   - Verify internet connectivity
   - Check weather.gov API status
   - Ensure coordinates are valid

### Debug Mode

To run in debug mode:
```bash
dotnet run --configuration Debug
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Resources

- [Model Context Protocol Documentation](https://modelcontextprotocol.io/)
- [Anthropic API Documentation](https://docs.anthropic.com/)
- [National Weather Service API](https://www.weather.gov/documentation/services-web-api)

## Acknowledgments

- Model Context Protocol team for the MCP specification
- Anthropic for the Claude AI integration
- National Weather Service for the weather data API 