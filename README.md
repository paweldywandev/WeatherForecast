# Weather Forecast Application

A full-stack weather forecast application built with ASP.NET Core 9 and React + TypeScript that provides current weather and forecast data using the OpenWeatherMap API.

**Production URL:** https://weatherforecast.paweldywandev.com/

**Local Development URL:** https://localhost:7176

## Overview

This application allows users to retrieve weather information by city name or geographic coordinates. It features a clean separation of concerns with a multi-layered architecture, comprehensive unit testing, and modern frontend built with React and Vite.

The application integrates with OpenWeatherMap API to provide:
- Current weather conditions
- Weather forecasts
- Location geocoding services

## Key Features

- Current weather data retrieval by city name or coordinates
- Weather forecast data with flexible querying options
- Geocoding service integration for location-based queries
- RESTful API with Swagger/OpenAPI documentation
- Responsive React frontend with Bootstrap UI
- Docker containerization support
- Comprehensive unit testing with xUnit, Moq, and FluentAssertions
- AutoMapper for object mapping
- User Secrets for secure API key management

## Architecture

The solution follows a layered architecture pattern with clear separation of concerns:

```
WeatherForecast/
|
+-- WeatherForecast.Server/
|   +-- Controllers/
|   |   +-- WeatherForecastController.cs
|   +-- Program.cs
|   +-- WeatherForecast.Server.csproj
|
+-- WeatherForecast.BLL/
|   +-- Services/
|   |   +-- WeatherService.cs
|   |   +-- GeocodingService.cs
|   |   +-- WeatherForecastService.cs
|   +-- Interfaces/
|   |   +-- IWeatherService.cs
|   |   +-- IGeocodingService.cs
|   |   +-- IWeatherForecastService.cs
|   +-- Models/
|   |   +-- WeatherData/
|   |   +-- GeocodingData/
|   +-- Enums/
|   +-- WeatherForecastMappingProfile.cs
|   +-- WeatherForecast.BLL.csproj
|
+-- WeatherForecast.Server.Tests/
|   +-- WeatherForecast.Server.Tests.csproj
|
+-- WeatherForecast.BLL.Tests/
|   +-- WeatherForecast.BLL.Tests.csproj
|
+-- weatherforecast.client/
    +-- src/
    +-- package.json
    +-- vite.config.ts
```

### Project Structure

#### WeatherForecast.Server
The API layer built with ASP.NET Core 9 that exposes RESTful endpoints. Configured with:
- Swagger/OpenAPI for API documentation
- SPA proxy for React development server integration
- HTTPS redirection and authorization middleware
- Static file serving for the React frontend

#### WeatherForecast.BLL (Business Logic Layer)
Contains the core business logic and services:
- **Services**: Implementation of weather and geocoding functionality
- **Interfaces**: Abstractions for dependency injection
- **Models**: Data transfer objects for weather and geocoding data
- **Enums**: Enumeration types for weather modes and units
- **AutoMapper Profile**: Object mapping configuration

#### Test Projects
- **WeatherForecast.Server.Tests**: Unit tests for API controllers
- **WeatherForecast.BLL.Tests**: Unit tests for business logic services

#### weatherforecast.client
React + TypeScript frontend built with Vite:
- Modern React 18 with hooks
- TypeScript for type safety
- Bootstrap and Reactstrap for UI components
- Fast development with Vite HMR (Hot Module Replacement)

## Getting Started

### Prerequisites

- [.NET 9 SDK](https://dotnet.microsoft.com/download/dotnet/9.0)
- [Node.js 20.x](https://nodejs.org/) (for the React frontend)
- [OpenWeatherMap API Key](https://openweathermap.org/api) (free tier available)
- [Docker](https://www.docker.com/) (optional, for containerized deployment)

### Installation

1. Clone the repository:
```bash
git clone https://github.com/paweldywandev/WeatherForecast.git
cd WeatherForecast
```

2. Configure User Secrets for the API key:
```bash
cd WeatherForecast.Server
dotnet user-secrets set "OpenWeatherMapApiKey" "YOUR_API_KEY_HERE"
```

Alternatively, for the BLL tests:
```bash
cd WeatherForecast.BLL.Tests
dotnet user-secrets set "OpenWeatherMapApiKey" "YOUR_API_KEY_HERE"
```

3. Restore NuGet packages:
```bash
dotnet restore
```

4. Install frontend dependencies:
```bash
cd weatherforecast.client
npm install
```

### Running the Application

#### Option 1: Using Visual Studio
1. Open `WeatherForecast.sln` in Visual Studio 2022 or later
2. Set `WeatherForecast.Server` as the startup project
3. Press F5 to run with debugging or Ctrl+F5 to run without debugging
4. The application will open at https://localhost:7176 with the React frontend
5. Swagger UI is available at https://localhost:7176/swagger

#### Option 2: Using .NET CLI
```bash
cd WeatherForecast.Server
dotnet run
```

The application will be available at:
- HTTPS: https://localhost:7176
- HTTP: http://localhost:5104
- Swagger UI: https://localhost:7176/swagger

The React development server will run on https://localhost:5173 (proxied automatically).

#### Option 3: Using Docker

Build and run the Docker container:
```bash
docker build -t weatherforecast .
docker run -p 8080:8080 -p 8081:8081 weatherforecast
```

Access the application at:
- HTTP: http://localhost:8080
- HTTPS: https://localhost:8081

### Running Tests

Run all tests:
```bash
dotnet test
```

Run tests with coverage:
```bash
dotnet test /p:CollectCoverage=true
```

Run tests for a specific project:
```bash
dotnet test WeatherForecast.Server.Tests
dotnet test WeatherForecast.BLL.Tests
```

## Technology Stack

### Backend
- **.NET 9**: Latest version of .NET for high-performance web APIs
- **ASP.NET Core**: Web framework for building RESTful APIs
- **AutoMapper 14.0.0**: Object-to-object mapping
- **Swashbuckle.AspNetCore 8.1.1**: Swagger/OpenAPI documentation
- **User Secrets**: Secure configuration management for development

### Frontend
- **React 18.3.1**: Modern JavaScript library for building user interfaces
- **TypeScript 5.6.3**: Type-safe JavaScript
- **Vite 5.4.10**: Next-generation frontend tooling
- **Bootstrap 5.3.3**: CSS framework for responsive design
- **Reactstrap 9.2.3**: React Bootstrap components

### Testing
- **xUnit 2.9.3**: Testing framework
- **Moq 4.20.72**: Mocking framework
- **FluentAssertions 8.2.0**: Fluent assertion library
- **Coverlet 6.0.4**: Code coverage collection

### External APIs
- **OpenWeatherMap API**: Weather data provider
  - Current Weather Data API (v2.5)
  - Forecast API (v2.5)
  - Geocoding API (v1.0)

## API Documentation

### Endpoints

#### GET /api/weatherforecast

Retrieves weather information based on query parameters.

**Query Parameters:**

By City:
- `cityName` (string, required): Name of the city
- `countryCode` (string, required): ISO 3166 country code
- `stateCode` (string, optional): State code (for US locations)

By Coordinates:
- `latitude` (decimal, required): Latitude coordinate
- `longitude` (decimal, required): Longitude coordinate

Additional:
- `mode` (enum, optional): Response mode (Current, Forecast)

**Example Requests:**

By city:
```
GET /api/weatherforecast?cityName=London&countryCode=GB
```

By coordinates:
```
GET /api/weatherforecast?latitude=51.5074&longitude=-0.1278
```

**Response:**
```json
{
  "current": {
    "temperature": 15.5,
    "humidity": 72,
    "description": "partly cloudy"
  },
  "forecast": [
    {
      "date": "2024-01-20T12:00:00Z",
      "temperature": 16.2,
      "description": "sunny"
    }
  ]
}
```

### Swagger UI

Interactive API documentation is available at `/swagger` when running in Development mode.

## Configuration

### User Secrets

The application uses User Secrets for secure API key storage during development:

**WeatherForecast.Server** (UserSecretsId: `facc69db-7815-4f0e-8790-b0470650a4af`)
```json
{
  "OpenWeatherMapApiKey": "your_api_key_here"
}
```

**WeatherForecast.BLL.Tests** (UserSecretsId: `03400f5d-e7d0-4dfb-a1b9-af26f780c4aa`)
```json
{
  "OpenWeatherMapApiKey": "your_api_key_here"
}
```

### Application Settings

Base URLs for external services are configured in `Program.cs`:
- Weather API: `https://api.openweathermap.org/data/2.5/`
- Geocoding API: `http://api.openweathermap.org/geo/1.0/`

## Development

### Frontend Development

The frontend uses Vite for fast development with HMR:

```bash
cd weatherforecast.client
npm run dev      # Start development server
npm run build    # Build for production
npm run lint     # Run ESLint
npm run preview  # Preview production build
```

### Code Quality

The solution includes:
- ESLint for TypeScript/React code quality
- Nullable reference types enabled for C# projects
- Implicit usings for cleaner code
- Comprehensive unit tests

## Contributing

Contributions are welcome! Please follow these guidelines:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

Please ensure:
- All tests pass
- Code follows existing conventions
- New features include appropriate tests
- API changes are documented

## License

This project is available as open source. Please check with the repository owner for specific license terms.

## Author

**Pawel Dywan**

- GitHub: [@paweldywandev](https://github.com/paweldywandev)
- Website: https://paweldywandev.com/

## Acknowledgments

- [OpenWeatherMap](https://openweathermap.org/) for providing the weather data API
- [ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/) team for the excellent framework
- [React](https://reactjs.org/) team for the frontend library
- [Vite](https://vitejs.dev/) for the blazing fast build tool
- All contributors and users of this project

---

For questions, issues, or suggestions, please open an issue on the GitHub repository.

