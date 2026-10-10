# Assignment: REST APIs and Real-World API Integration

**Selected API:** Open-Meteo Geocoding and Forecast APIs  
**Practical application:** React weather dashboard  
**Cost/authentication:** Free access for non-commercial use, with no API key or sign-up required. Check the provider's current terms and fair-use limits.

## 1. Introduction
A REST API (Representational State Transfer Application Programming Interface) enables software applications to communicate over a network using standard HTTP methods. APIs allow applications to use data or capabilities provided by other services. A weather application can request forecast information from a weather API rather than collecting weather measurements itself.

## 2. Objectives
- Understand REST API concepts and HTTP methods.
- Perform GET requests to a public API.
- Parse JSON responses.
- Display returned data in a React interface.
- Handle loading states and request errors.
- Identify API benefits and industry applications.

## 3. Fundamental REST concepts
- **Client:** Application making a request.
- **Server:** System processing a request and returning a response.
- **Endpoint:** URL for a resource or operation.
- **HTTP method:** Describes the requested operation.
- **Request:** Data sent by the client.
- **Response:** Status, headers, and optional response body from the server.
- **JSON:** Lightweight format for structured data exchange.
- **Statelessness:** Each request contains the information needed to process it without depending on stored client-session context on the server.

### Common HTTP methods
| Method | Purpose | Example |
|---|---|---|
| GET | Retrieve data | Fetch weather |
| POST | Create a resource | Submit an order |
| PUT | Replace a resource | Replace a profile |
| PATCH | Partially update a resource | Change an address |
| DELETE | Delete a resource | Remove a saved item |

## 4. How the application works
1. The user enters a city name.
2. The application sends a GET request to Open-Meteo's geocoding API.
3. The geocoding response provides the location's latitude and longitude.
4. The application sends a second GET request to the forecast API.
5. The forecast API returns JSON weather data.
6. React parses and displays the weather details.

## 5. Selected API: Open-Meteo
Open-Meteo provides weather forecast data. For non-commercial use, its public API does not require an API key or sign-up. It does have fair-use limits and attribution requirements.

**Geocoding endpoint**
```text
https://geocoding-api.open-meteo.com/v1/search?name=Mumbai&count=1&language=en&format=json
```

**Forecast endpoint example**
```text
https://api.open-meteo.com/v1/forecast?latitude=19.076&longitude=72.878&current=temperature_2m,relative_humidity_2m,apparent_temperature,wind_speed_10m,weather_code
```

The geocoding endpoint finds coordinates for the city. The forecast endpoint accepts latitude and longitude and returns current conditions and forecast variables.

## 6. Understanding JSON responses
A simplified geocoding response may contain:
```json
{
  "results": [
    {
      "name": "Mumbai",
      "country": "India",
      "latitude": 19.076,
      "longitude": 72.878
    }
  ]
}
```

A simplified forecast response may contain:
```json
{
  "current": {
    "temperature_2m": 29.5,
    "relative_humidity_2m": 78,
    "apparent_temperature": 33.1,
    "wind_speed_10m": 4.2,
    "weather_code": 3
  }
}
```

These values are illustrative examples, not live readings. JavaScript reads the forecast with expressions such as `data.current.temperature_2m` and `data.current.relative_humidity_2m`.

## 7. GET request and JSON parsing example
```javascript
async function getWeather(city) {
  const geoUrl =
    `https://geocoding-api.open-meteo.com/v1/search?name=${encodeURIComponent(city)}&count=1&language=en&format=json`;

  const geoResponse = await fetch(geoUrl);
  if (!geoResponse.ok) throw new Error(`Geocoding HTTP error: ${geoResponse.status}`);

  const geoData = await geoResponse.json();
  if (!geoData.results?.length) throw new Error("City not found");

  const place = geoData.results[0];
  const weatherUrl =
    `https://api.open-meteo.com/v1/forecast?latitude=${place.latitude}&longitude=${place.longitude}&current=temperature_2m,relative_humidity_2m,apparent_temperature,wind_speed_10m,weather_code`;

  const weatherResponse = await fetch(weatherUrl);
  if (!weatherResponse.ok) throw new Error(`Weather HTTP error: ${weatherResponse.status}`);

  const weatherData = await weatherResponse.json();
  console.log(place.name, weatherData.current);
}

getWeather("Mumbai").catch(console.error);
```

`fetch()` makes an HTTP request. `response.ok` checks for a successful HTTP status. `response.json()` parses the response body into a JavaScript object. `try...catch` (or a promise `.catch()`) handles errors. Fetch does not automatically throw for HTTP responses such as 404, so checking the response status is important.

## 8. React application
The included React application allows a user to search by city and displays temperature, feels-like temperature, humidity, wind speed, and a readable weather description. It has loading feedback and handles unknown cities and network/API errors. It uses two public GET endpoints and requires no API key.

## 9. HTTP status codes
| Status | Meaning | Handling |
|---|---|---|
| 200 | OK | Display the response |
| 400 | Bad request | Check parameters |
| 404 | Not found | Check endpoint or city |
| 429 | Too many requests | Wait and retry later |
| 500 | Server error | Show an error and retry later if appropriate |

Actual error behavior depends on the provider's implementation.

## 10. Benefits of REST APIs
- **Reusability:** One service can support multiple applications.
- **Interoperability:** Different languages and platforms can communicate over HTTP.
- **Scalability:** Client and server components can be scaled independently.
- **Faster development:** Existing services can be integrated rather than rebuilt.
- **Maintainability:** APIs separate user interfaces from service logic.
- **Fresh information:** Applications can request updated data when needed.

## 11. Industry use cases
1. **E-commerce:** Product, inventory, payment, and shipping APIs connect the checkout process.
2. **Banking:** Authenticated APIs support account information and payment services with appropriate security controls.
3. **Weather and travel:** Weather, maps, flight, and booking APIs provide information useful for planning.
4. **Healthcare:** Authorized APIs can connect appointments, laboratory systems, and electronic health records, subject to privacy and access requirements.

These are representative use cases rather than claims of measured results from a specific company.

## 12. Security and best practices
- Use HTTPS.
- Validate and encode user input.
- Handle network failures and unsuccessful HTTP responses.
- Respect rate limits and API terms.
- Avoid logging credentials.
- Request only the data needed.
- Attribute Open-Meteo as required by its data licence.

## 13. Conclusion
REST APIs allow applications to exchange data using standardized HTTP requests and responses. JSON provides a practical format for structured data, and JavaScript's Fetch API supports asynchronous requests. The Open-Meteo React project demonstrates GET requests, JSON parsing, data visualization, and error handling without requiring an API key. API integration supports modular, reusable applications across weather, commerce, finance, travel, and healthcare.

## 14. References
1. Open-Meteo, API documentation: https://open-meteo.com/en/docs
2. Open-Meteo, Pricing and free-tier terms: https://open-meteo.com/en/pricing
3. Open-Meteo, Geocoding API: https://open-meteo.com/en/docs/geocoding-api
4. MDN Web Docs, Using the Fetch API: https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch
5. MDN Web Docs, `Response.ok`: https://developer.mozilla.org/en-US/docs/Web/API/Response/ok
6. Roy T. Fielding (2000), *Architectural Styles and the Design of Network-based Software Architectures*: https://ics.uci.edu/~fielding/pubs/dissertation/top.htm

## Submission checklist
- [ ] Run the app and capture a screenshot showing current API data.
- [ ] Capture a screenshot showing an unknown-city error.
- [ ] Include this report and the source code in your submission.
