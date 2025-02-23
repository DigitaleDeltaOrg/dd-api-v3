public record Location(
string Id,
string Name,
Coordinates Coordinates,
string Type);

public record Coordinates(double Latitude, double Longitude);

public class ApiClient(HttpClient httpClient, string apiKey)
{
private readonly HttpClient _httpClient = httpClient;

    public ApiClient(string apiKey) : this(new HttpClient(), apiKey)
    {
        _httpClient.BaseAddress = new Uri("https://api.dd.nld/v3/");
        _httpClient.DefaultRequestHeaders.Authorization = 
            new AuthenticationHeaderValue("Bearer", apiKey);
        _httpClient.DefaultRequestHeaders.Accept.Add(
            new MediaTypeWithQualityHeaderValue("application/json"));
    }

    public async Task<IEnumerable<Location>> GetLocationsAsync(
        string? parameterType = null,
        CancellationToken cancellationToken = default)
    {
        try
        {
            var query = new Dictionary<string, string>
            {
                ["$select"] = "id,name,coordinates"
            };

            if (!string.IsNullOrEmpty(parameterType))
            {
                query["$filter"] = $"type eq '{parameterType}'";
            }

            var queryString = string.Join("&", 
                query.Select(kv => $"{kv.Key}={Uri.EscapeDataString(kv.Value)}"));
            
            var response = await _httpClient.GetAsync(
                $"references/locations?{queryString}", cancellationToken);
            
            response.EnsureSuccessStatusCode();

            var result = await response.Content
                .ReadFromJsonAsync<ODataResponse<Location>>(
                    cancellationToken: cancellationToken);

            return result?.Value ?? Enumerable.Empty<Location>();
        }
        catch (HttpRequestException ex)
        {
            throw new ApiException("Failed to fetch locations", ex);
        }
        catch (JsonException ex)
        {
            throw new ApiException("Failed to parse response", ex);
        }
    }
}

// Gebruik:
public class LocationService(ILogger<LocationService> logger)
{
public async Task ProcessLocationsAsync()
{
using var client = new ApiClient("jouw-api-key");

        try
        {
            var locations = await client.GetLocationsAsync("waterHeight");
            foreach (var location in locations)
            {
                logger.LogInformation(
                    "Location {name} at {lat},{lon}", 
                    location.Name, 
                    location.Coordinates.Latitude, 
                    location.Coordinates.Longitude);
            }
        }
        catch (ApiException ex)
        {
            logger.LogError(ex, "Error processing locations");
            throw;
        }
    }
}