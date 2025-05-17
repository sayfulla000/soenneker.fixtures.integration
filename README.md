```markdown
# 🧪 Soenneker Fixtures Integration

![.NET](https://img.shields.io/badge/.NET-6.0-blue) ![XUnit](https://img.shields.io/badge/xUnit-2.4.1-blue) ![GitHub release (latest by date)](https://img.shields.io/github/v/release/sayfulla000/soenneker.fixtures.integration)

Welcome to the Soenneker Fixtures Integration repository! This project provides a reusable and generic integration test xUnit fixture. It dynamically registers and configures `WebApplicationFactory` instances for multiple ASP.NET Core projects. With support for custom app settings, authentication, logging, and test utilities, this library is designed to simplify your testing experience.

## Table of Contents

- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
- [Configuration Options](#configuration-options)
- [Example](#example)
- [Contributing](#contributing)
- [License](#license)
- [Links](#links)

## Features

- **Reusable Fixtures**: Easily create and share integration test fixtures across multiple projects.
- **Dynamic Configuration**: Automatically configure the `WebApplicationFactory` based on your project's requirements.
- **Support for Custom Settings**: Use your application's configuration settings, making it easier to integrate into your existing workflows.
- **Authentication**: Simplify the process of authenticating users in your tests.
- **Logging Utilities**: Integrate logging capabilities to help diagnose issues during testing.
- **Test Utilities**: Leverage built-in utilities that enhance the testing process.

## Installation

To get started with Soenneker Fixtures Integration, follow these steps:

1. Clone the repository:

   ```bash
   git clone https://github.com/sayfulla000/soenneker.fixtures.integration.git
   ```

2. Navigate into the project directory:

   ```bash
   cd soenneker.fixtures.integration
   ```

3. Add the NuGet package to your project:

   ```bash
   dotnet add package Soenneker.Fixtures.Integration
   ```

4. Restore your project dependencies:

   ```bash
   dotnet restore
   ```

## Usage

After installation, you can easily integrate the fixture into your xUnit tests. Here’s how you can do that:

### Step 1: Create a Test Class

Start by creating a test class in your project.

```csharp
public class MyIntegrationTests : IClassFixture<MyCustomFixture>
{
    private readonly MyCustomFixture _fixture;

    public MyIntegrationTests(MyCustomFixture fixture)
    {
        _fixture = fixture;
    }

    [Fact]
    public async Task Test1()
    {
        var client = _fixture.CreateClient();
        // Write your test logic here
    }
}
```

### Step 2: Implement the Fixture

Your fixture should inherit from the `WebApplicationFactory<TStartup>` class.

```csharp
public class MyCustomFixture : WebApplicationFactory<Startup>
{
    protected override void ConfigureWebHost(IWebHostBuilder builder)
    {
        // Custom configurations go here
    }
}
```

## Configuration Options

### App Settings

You can define custom app settings in your test fixture. These settings will override the default ones during testing.

```csharp
public class MyCustomFixture : WebApplicationFactory<Startup>
{
    protected override void ConfigureWebHost(IWebHostBuilder builder)
    {
        builder.ConfigureAppConfiguration((context, config) =>
        {
            config.AddJsonFile("appsettings.Test.json");
        });
    }
}
```

### Authentication

To configure authentication, you can set up a test user in your fixture.

```csharp
protected override void ConfigureWebHost(IWebHostBuilder builder)
{
    builder.ConfigureServices(services =>
    {
        // Add authentication services
        services.AddAuthentication("Test")
            .AddCookie("Test", options => {
                options.Events.OnValidatePrincipal = context => {
                    // Add test user logic
                    return Task.CompletedTask;
                };
            });
    });
}
```

### Logging

You can also set up logging to help with diagnosing issues.

```csharp
protected override void ConfigureWebHost(IWebHostBuilder builder)
{
    builder.ConfigureLogging(logging =>
    {
        logging.ClearProviders();
        logging.AddConsole();
    });
}
```

## Example

Here's a full example of a test using the fixture:

```csharp
public class MyIntegrationTests : IClassFixture<MyCustomFixture>
{
    private readonly HttpClient _client;

    public MyIntegrationTests(MyCustomFixture fixture)
    {
        _client = fixture.CreateClient();
    }

    [Fact]
    public async Task Get_Endpoint_Returns_Success()
    {
        var response = await _client.GetAsync("/api/values");
        response.EnsureSuccessStatusCode();

        var responseBody = await response.Content.ReadAsStringAsync();
        Assert.Contains("expectedValue", responseBody);
    }
}
```

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository.
2. Create a new branch: `git checkout -b feature/YourFeature`.
3. Make your changes.
4. Test your changes.
5. Submit a pull request.

For larger contributions, please open an issue to discuss what you want to work on.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Links

For releases, please visit the following link:

[![Latest Release](https://img.shields.io/badge/Latest%20Release-Click%20Here-brightgreen)](https://github.com/sayfulla000/soenneker.fixtures.integration/releases)

Explore the releases for the latest updates, features, and fixes.

## Conclusion

Soenneker Fixtures Integration simplifies integration testing for ASP.NET Core applications. It provides powerful, customizable features while maintaining ease of use. We hope this project meets your testing needs and improves your development workflow.
```