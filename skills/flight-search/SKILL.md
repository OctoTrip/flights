---
name: octotrip-flight-search
description: Search and compare flights with real-time pricing. Use when the user wants to find flights, compare airfares, or book air travel between cities or airports.
license: MIT
metadata:
  author: octotrip
  version: "1.0.0"
---

# OctoTrip Flight Search

Search and compare flights with real-time pricing from multiple airlines and booking platforms. Free, no API key required.

## Connect

Add the OctoTrip Flights MCP server to your configuration:

```json
{
  "mcpServers": {
    "octotrip-flights": {
      "url": "https://mcp.octotrip.app/flights/mcp"
    }
  }
}
```

Transport: Streamable HTTP. No authentication, no API key, no login.

## Search Parameters

Call the `search` tool with:

| Parameter | Required | Default | Description |
|---|---|---|---|
| `origin` | yes | -- | City, airport name, or IATA code (e.g. "Frankfurt", "JFK") |
| `destination` | yes | -- | City, airport name, or IATA code |
| `departure_date` | yes | -- | YYYY-MM-DD or natural-language date |
| `return_date` | no | -- | Set for round-trip, omit for one-way |
| `adults` | no | 1 | Number of adults (1-9) |
| `children` | no | 0 | Children aged 2-11 (0-9) |
| `infants` | no | 0 | Infants under 2 (0-9) |
| `trip_class` | no | "Y" | "Y" for economy, "C" for business |
| `currency` | no | "EUR" | ISO 4217 code (EUR, USD, GBP, etc.) |
| `locale` | no | "en" | Language code (en, de, etc.) |

## Handling Results

Results are ranked by price within each stop-count group (direct flights first, then 1-stop, etc.).

Each result includes:
- **airline**, **flight_numbers**, **stops**, **total_duration_minutes**
- **outbound** and **return** with departure/arrival times, airports, and per-leg details
- **price** and **currency**
- **baggage** info
- **booking_url** -- a link the user can open to book
- **tags** -- e.g. "direct", "convenient_ticket"

Present results by highlighting the cheapest direct option first. If there are no direct flights, show the cheapest 1-stop option. Always include the price, airline, duration, and departure/arrival times.

Results and booking links expire after approximately **15 minutes**. If the user wants to book later, run a fresh search.

## Handling Errors

- **`disambiguation_needed`**: Multiple airports match the input (e.g. "London" returns STN and LHR). Present the options to the user and retry with their choice.
- **`airport_not_found`**: Try an IATA code or more specific name.
- **`no_results`**: Suggest different dates or a nearby airport.

## Tips

- Use IATA codes when you know them -- they avoid disambiguation.
- For cities with multiple airports (London, New York, Tokyo), expect a disambiguation response. Present the list and let the user pick.
- Round-trip searches return both outbound and return legs in one result.
- The server resolves city names to airports automatically. "Frankfurt" resolves to FRA.

## Affiliate Disclosure

Booking links contain affiliate attribution. OctoTrip may earn a commission at no extra cost to the user. Results are ranked by price, not by affiliate payout.
