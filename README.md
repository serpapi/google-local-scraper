# Google Local Scraper

[![Google Local Scraper](https://github.com/user-attachments/assets/67ca5895-65f9-4ebf-a1fc-921ab2a2a97d)](https://serpapi.com/google-local-api?utm_source=github_google_local_scraper)

[SerpApi Google Local API](https://serpapi.com/google-local-api?utm_source=github_google_local_scraper)

Google Local Scraper - A tool to scrape Google Local search results with a simple API. Get business names, addresses, phone numbers, hours, ratings, review counts, GPS coordinates, and more when available.

We provide the results in a structured JSON format, eliminating the need for parsing, coding, proxies, or any other web scraping headaches for developers.

This guide uses the `google_local` engine for Google Local results. For searches on Google Maps, see our [Google Maps Scraper](../google-maps-scraper).

## How to scrape Google Local?

Using a simple GET request, you can retrieve Google Local search results:

```
https://serpapi.com/search.json?engine=google_local&q=Coffee&location=Austin%2C+Texas%2C+United+States&gl=us&hl=en&api_key=YOUR_API_KEY
```

- Register for free at [SerpApi to get your API Key](https://serpapi.com?utm_source=github_google_local_scraper).
- `q` parameter: defines the search query.
- `location` parameter (optional): defines where the search originates. A city-level location is recommended; without it, results may use the proxy's location.

## Code examples
Here are some code examples based on your favorite programming languages.

### cURL Integration

``` bash
curl --get https://serpapi.com/search \
 --data-urlencode engine="google_local" \
 --data-urlencode q="Coffee" \
 --data-urlencode location="Austin, Texas, United States" \
 --data-urlencode api_key="YOUR_SERPAPI_API_KEY"
```

### Python Integration

Step 1:
Create a new `main.py` file.

Step 2:
Install [requests package](https://pypi.org/project/requests/) with:
```
pip install requests
```

Step 3:
Add this code to your file:
``` py
import requests

SERPAPI_API_KEY = "YOUR_SERPAPI_API_KEY"

params = {
    "api_key": SERPAPI_API_KEY,
    "engine": "google_local",
    "q": "Coffee",
    "location": "Austin, Texas, United States",
    "gl": "us",
    "hl": "en"
}

search = requests.get("https://serpapi.com/search", params=params, timeout=60)
search.raise_for_status()
response = search.json()
if "error" in response:
    raise RuntimeError(response["error"])
print(response)
```

If you're only interested in the `local_results`, you can print them from the response directly. It's an array of local businesses:

``` py
print(response["local_results"])
```

### JavaScript Integration

Step 1:
Install the [SerpApi JavaScript package](https://github.com/serpapi/serpapi-javascript):
```
npm install serpapi
```

Step 2:
Create a new `index.js` file.

Step 3:
Add this code to your file:
``` js
const { getJson } = require("serpapi");
const API_KEY = "YOUR_SERPAPI_API_KEY";

getJson({
  api_key: API_KEY,
  engine: "google_local",
  q: "Coffee",
  location: "Austin, Texas, United States",
  gl: "us",
  hl: "en"
}, (json) => {
  if (json.error) {
    throw new Error(json.error);
  }
  console.log(json["local_results"]);
});
```

### Other Programming Languages
While you can use our APIs using a simple GET request with any programming language, you can also see our ready-to-use libraries here: [SerpApi Integrations](https://serpapi.com/integrations?utm_source=github_google_local_scraper).

## Google Local Scraper Parameters
Please find the main parameters for the Google Local API below:

| Name | Description | Requirement |
|------|-------------|-------------|
| engine | Must be set to `google_local`. | Required |
| api_key | Your SerpApi private API key. | Required |
| q | The query you want to search, as you would in a regular Google Local search. | Required |
| **Geographic Location** | | |
| location | Search origin, preferably at city level. Cannot be used with `uule`. Use the [Locations API](https://serpapi.com/locations-api) to find supported locations. | Optional |
| uule | Google-encoded location. Cannot be used with `location`. Coordinate-based values require a matching `gl` for consistent results. | Optional |
| **Localization** | | |
| google_domain | Google domain to use. Defaults to `google.com`. | Optional |
| gl | Two-letter country code, such as `us`, `uk`, or `fr`. | Optional |
| hl | Language code, such as `en`, `es`, or `fr`; regional variants such as `en-gb` are also supported. | Optional |
| **Advanced Filters** | | |
| ludocid | Google CID of a specific place. Available as `place_id` in Google Local results or `data_cid` in Google Maps results. | Optional |
| tbs | Advanced Google search filters that cannot be expressed in the regular query field. | Optional |
| **Pagination** | | |
| start | Result offset. Pages may contain 10 or 20 results; use `serpapi_pagination.next` rather than assuming a fixed increment. | Optional |
| **SerpApi Parameters** | | |
| device | Device type: `desktop` (default), `tablet`, or `mobile`. | Optional |
| no_cache | Set to `true` to request fresh results instead of using the one-hour cache. Cannot be combined with `async`. | Optional |
| async | Set to `true` to submit a search for later retrieval through the [Searches Archive API](https://serpapi.com/search-archive-api). Cannot be combined with `no_cache` or used with Ludicrous Speed enabled. | Optional |
| output | Output format: `json` (default), `html`, or `md`. The examples above use JSON. | Optional |

Visit [our documentation](https://serpapi.com/google-local-api) for all available parameters.

### Pagination

When another page is available, the response provides `serpapi_pagination.next`. Use that URL for the next request with your API key. SerpApi handles the appropriate result offset in the link, including differences between devices. Stop when there is no next-page link.

## Available data on Google Local (JSON Response)
The fields returned depend on what Google makes available for the query and each business. The following is a field guide, not a literal API response:

``` json
{
  "local_map": {
    "gps_coordinates": {
      "latitude": "Float - Latitude of the map center",
      "longitude": "Float - Longitude of the map center"
    }
  },
  "local_results": [
    {
      "position": "Integer - Position of the local result",
      "title": "String - Business name",
      "type": "String - Business category",
      "address": "String - Business address",
      "rating": "Float - Business rating",
      "reviews": "Integer - Number of reviews",
      "reviews_original": "String - Review count as displayed by Google",
      "price": "String - Displayed price or price range",
      "description": "String - Description or review excerpt",
      "place_id": "String - Google CID, usable as the ludocid parameter",
      "place_id_search": "String - SerpApi URL for a place search",
      "provider_id": "String - Google provider identifier",
      "gps_coordinates": {
        "latitude": "Float - Latitude of the business",
        "longitude": "Float - Longitude of the business"
      },
      "thumbnail": "String - Thumbnail image URL",
      "thumbnail_large": "String - Larger image URL"
    }
  ],
  "serpapi_pagination": {
    "next": "String - SerpApi URL for the next page, when available"
  }
}
```

Phone numbers and hours may also be available. Do not assume every result includes every field.

## Use cases
Here are some use cases for the Google Local API:

- Build city-specific business directories with names, addresses, and categories.
- Monitor local search rankings for target keywords and locations.
- Compare nearby competitors by ratings, review counts, and price ranges.
- Discover businesses for lead research and CRM enrichment.
- Plot business locations using the returned GPS coordinates.
- Connect Google Local results to Google Maps searches using the shared CID (`place_id` in Local results and `data_cid` in Maps results).

## Blog tutorial
- [How to Scrape Google Local Results](https://serpapi.com/blog/how-to-scrape-google-local-results/)
- [Bridging Google Local API and Google Maps API](https://serpapi.com/blog/bridging-google-local-api-and-google-maps-api/)

## Contacts
Feel free to reach out via `contact@serpapi.com`.

Check other [Google Scrapers](https://github.com/serpapi/google-scraper) from SerpApi.

