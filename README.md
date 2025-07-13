# GoogleAds.API

GoogleAds.API is an async, thread-safe singleton implementation for the Google Ads client. It ensures safe and efficient concurrent access to the Google Ads API, making it ideal for high-performance applications.

## Features

- Async support for non-blocking operations.
- Thread-safe singleton design pattern.
- Easy integration with Google Ads API.
- Designed for high concurrency environments.

## Installation

You can install the package via NuGet:

```
Install-Package GoogleAds.API
```



## Example
🔧 1. Configuration (e.g. appsettings.json)
```json
{
  "GoogleAds": {
    "DeveloperToken": "YOUR_DEV_TOKEN",
    "LoginCustomerId": "123-456-7890",
    "ClientCustomerId": "098-765-4321",
    "OAuthClientId": "your-oauth-client-id",
    "OAuthClientSecret": "your-oauth-client-secret",
    "RefreshToken": "your-refresh-token"
  }
}
```

🚀 2. Initialize and Use the Client
```csharp
            // Load configuration
            var config = new ConfigurationBuilder()
                .AddJsonFile("appsettings.json")
                .Build();

            // Initialize the Google Ads client
            var adsClient = GoogleAdsClientSingleton.Create(config.GetSection("GoogleAds"));

            // Example: fetch first 10 campaigns
            var service = adsClient.GetClient().GetService(Services.V14.GoogleAdsService);
            var response = await service.SearchAsync($"SELECT campaign.id, campaign.name FROM campaign ORDER BY campaign.id LIMIT 10");

            foreach (var row in response)
            {
                Console.WriteLine($"Campaign ID: {row.Campaign.Id}, Name: {row.Campaign.Name}");
            }
```

🧩 3. Customizing for Campaign Creation
```csharp
using Google.Ads.GoogleAds.V14.Services;
using Google.Ads.GoogleAds.V14.Resources;

// ... initialize adsClient as shown above ...

var campaignService = adsClient.GetClient().GetService(CampaignServiceClient);

var campaign = new Campaign
{
    Name = "My New Campaign",
    AdvertisingChannelType = AdvertisingChannelType.Search,
    Status = CampaignStatus.Paused,
    ManualCpc = new ManualCpc(),
    CampaignBudget = ResourceNames.CampaignBudget(yourCustomerId, yourBudgetId)
};

var operation = new CampaignOperation { Create = campaign };

var response = await campaignService.MutateCampaignsAsync(customerId.ToString(), new[] { operation });

Console.WriteLine($"Created campaign with resource name: {response.Results[0].ResourceName}");
```
## Requirements

- .NET Standard compatible environment.
- Google Ads API credentials.

## Contributing

Contributions are welcome! Please submit pull requests or open issues on the repository.

## License

MIT License
