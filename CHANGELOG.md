# Changelog

## 0.2.0 - 2026-06-25

### Added
- Response schema (response_schema.json) with full JSON Schema for success and error payloads
- Wire-level protocol example (curl) in README
- Server log samples in PRIVACY.md for full transparency

### Changed
- Removed IP address logging entirely — access logs no longer contain any user-identifiable data
- Updated privacy documentation to reflect actual logging behavior

## 0.1.0 - 2026-06-24

### Added
- Initial release with `search` tool
- One-way and round-trip flight search
- Real-time pricing from multiple airlines and booking platforms
- Results grouped by stop count with carrier diversity
- Airport IATA resolution from city/airport names
- Streamable HTTP MCP transport
- Deployed at mcp.octotrip.app/flights
