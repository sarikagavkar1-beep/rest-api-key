# REST API Weather Assignment — No API Key Required

This React + Vite weather dashboard uses **Open-Meteo**, which offers free API access for non-commercial use without an API key or sign-up. See its current terms and usage limits at https://open-meteo.com/en/pricing.

## Run in VS Code
1. Extract this ZIP.
2. Open the extracted folder in VS Code.
3. Open Terminal → New Terminal.
4. Run:
   ```bash
   npm install
   npm run dev
   ```
5. Open the local URL printed in the terminal, usually `http://localhost:5173`.
6. Search for Mumbai, Delhi, Pune, or another city.

No `.env` file or API key is needed.

## How it works
- Calls Open-Meteo's geocoding API to find a city's coordinates.
- Calls the forecast API using latitude and longitude.
- Parses the JSON response and displays current temperature, relative humidity, apparent temperature, wind speed, and weather conditions.
- Handles loading states and API/network errors.

## Notes
The free API is intended for non-commercial use, subject to fair-use limits and attribution requirements. The dashboard credits Open-Meteo. See https://open-meteo.com/en/pricing and https://open-meteo.com/en/docs.

## Troubleshooting
- Make sure your internet connection is active.
- If PowerShell blocks `npm.ps1`, use Command Prompt in VS Code or run `npm.cmd install` and `npm.cmd run dev`.
- If a city is not found, check its spelling or include a country name, such as `London, UK`.
