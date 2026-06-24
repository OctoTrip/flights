# OctoTrip Flights MCP Server

Free, no-login MCP server for searching and comparing flights with real-time pricing from multiple airlines and booking platforms worldwide.

**Endpoint:** `https://mcp.octotrip.app/flights/mcp`

## Affiliate Disclosure

OctoTrip is free to use. Booking links contain affiliate attribution -- OctoTrip may earn a commission at no extra cost to you. Search results are ranked by price within each stop-count group, not by affiliate payout.

## Quick Start

Add to your MCP client configuration:

```json
{
  "mcpServers": {
    "octotrip-flights": {
      "url": "https://mcp.octotrip.app/flights/mcp"
    }
  }
}
```

No API key or login required.

## Tool: `search`

Searches live flight offers between an origin and destination for given travel dates. Supports one-way and round-trip searches with flexible passenger counts and cabin class. Returns price-ranked results grouped by number of stops, showing the cheapest options from multiple airlines and booking platforms. The tool queries external flight aggregator APIs in real time and includes affiliate booking links. It does not book flights, modify reservations, charge users, or store user data.

### Parameters

| Parameter | Type | Required | Default | Description |
|---|---|---|---|---|
| `origin` | string | yes | -- | Departure city, airport name, or IATA code (e.g. "Frankfurt", "JFK") |
| `destination` | string | yes | -- | Arrival city, airport name, or IATA code |
| `departure_date` | string | yes | -- | Departure date (YYYY-MM-DD, DD.MM.YYYY, or natural-language) |
| `return_date` | string | no | -- | Return date for round-trip. Omit for one-way |
| `adults` | integer | no | `1` | Number of adult passengers (1-9) |
| `children` | integer | no | `0` | Number of children aged 2-11 (0-9) |
| `infants` | integer | no | `0` | Number of infants under 2 (0-9) |
| `trip_class` | string | no | `"Y"` | Cabin class: Y for economy, C for business |
| `currency` | string | no | `"EUR"` | ISO 4217 currency code (EUR, USD, GBP, etc.) |
| `locale` | string | no | `"en"` | Response language code (en, de, etc.) |

### Response Format

Results are grouped by number of stops with the cheapest options per group:

```json
{
  "results": [
    {
      "airline": "Lufthansa",
      "airline_code": "LH",
      "alliance": "Star Alliance",
      "flight_numbers": ["LH120", "LH121"],
      "is_direct": true,
      "stops": 0,
      "total_duration_minutes": 110,
      "outbound": {
        "departure": "FRA",
        "arrival": "MUC",
        "departure_time": "18:15",
        "arrival_time": "19:10",
        "departure_date": "2026-07-15",
        "arrival_date": "2026-07-15",
        "duration_minutes": 55,
        "stops": 0,
        "legs": [
          {
            "flight_number": "LH120",
            "carrier": "Lufthansa",
            "aircraft": "Airbus A319",
            "departure": "FRA",
            "arrival": "MUC",
            "departure_time": "18:15",
            "arrival_time": "19:10",
            "departure_date": "2026-07-15",
            "arrival_date": "2026-07-15",
            "duration_minutes": 55
          }
        ]
      },
      "return": { ... },
      "price": 292.39,
      "currency": "EUR",
      "baggage": "No checked bag, Cabin bag: 1 piece(s)",
      "gate": "City.Travel",
      "booking_url": "https://...",
      "link_type": "affiliate",
      "affiliate_disclosure": "This link contains affiliate attribution. OctoTrip may earn a commission at no extra cost to you.",
      "tags": ["convenient_ticket", "direct"]
    }
  ],
  "total": 6,
  "total_available": 876,
  "origin_resolved": { "iata": "FRA", "name": "Frankfurt Airport", "city": "Frankfurt am Main", "country_code": "DE" },
  "destination_resolved": { "iata": "MUC", "name": "Munich Airport", "city": "Munich", "country_code": "DE" },
  "validity": "Results and booking links are valid for approximately 15 minutes.",
  "query": { ... }
}
```

### Result Validity

Search results and booking links are valid for approximately **15 minutes**. Flight prices are determined through a real-time bidding process and are not guaranteed beyond this window. If a user wants to book, they should do so promptly. For stale results, run a new search.

### Error Responses

The server returns structured errors with suggestions:

- **`airport_not_found`** -- airport could not be resolved. Try an IATA code, city name, or airport name.
- **`disambiguation_needed`** -- multiple airports found (e.g. "New York" returns JFK, LGA). Pick one and retry.
- **`invalid_date`** -- date format not recognized. Use YYYY-MM-DD, DD.MM.YYYY, or similar.
- **`no_results`** -- no flights available for the given criteria. Try different dates or a nearby airport.

## Example Prompts

- Find me the cheapest direct flight from New York to London in August
- Compare flights from Frankfurt to Munich on July 15, round-trip returning July 22
- I need a one-way flight from LAX to Tokyo next Friday, economy class
- Search for 2 adults and 1 child flying from Barcelona to Paris on September 1
- What's the cheapest business class flight from Dubai to Singapore this month?

## Use with Claude Desktop

Add to `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "octotrip-flights": {
      "url": "https://mcp.octotrip.app/flights/mcp"
    }
  }
}
```

## Use with Cursor / Windsurf

Add to your MCP settings:

```json
{
  "mcpServers": {
    "octotrip-flights": {
      "url": "https://mcp.octotrip.app/flights/mcp"
    }
  }
}
```

## Use with Cline

Add to your Cline MCP settings:

```json
{
  "mcpServers": {
    "octotrip-flights": {
      "url": "https://mcp.octotrip.app/flights/mcp"
    }
  }
}
```

## Privacy

This server does not store, log, or track any user data. Queries are forwarded to provider APIs and results are returned directly. See [PRIVACY.md](PRIVACY.md) for details.

## Security

To report a vulnerability, see [SECURITY.md](SECURITY.md).

## License

MIT
