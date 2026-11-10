For this assignment I'd go with the Open-Meteo Weather API. It's a pretty good one to start with as it provides weather data via public REST endpoints (no API key required). Your app would simply allow the user to enter a location, use JavaScript fetch() to get the weather data and then display that in a table or as weather cards.

Below is a complete Markdown planning document which you can use as the basis of your GitHub repo. It covers the REST principles, the endpoint mapping, the data flow, the form validation, the tech stack and the implementation steps.

One key design decision: the public weather service will only be used for GET requests. The form's POST request will go to a separate Python Flask backend, which you'll build at a later stage. That keeps things clean and doesn't imply in any way that Open-Meteo is going to accept your application's form submissions. Open-Meteo documents both its location search endpoint and its forecast endpoint and its public non-commercial service does not require an API key.

1. Planning document
Create a new GitHub repository named restful-api-planning. Inside it, create a file named restful-api-planning.md and paste the following:
:::writing{variant=“document” id=“63821”}

REST API Planning Document: Weather Forecast Client

1. API Selection

For this project I went with the Open-Meteo weather API which is listed as a public weather API. Open-Meteo provides current weather and forecasts for locations worldwide. I selected it because it's appropriate for a beginner level REST client, no API key is required for non-commercial use and the data is returned in JSON format.

The purpose of my app will be to allow the user to search for a city and then view the current weather and the forecast for that city. It will use the Open-Meteo Geocoding API to find the location and the Weather Forecast API to get the weather data.

The design is RESTful in that it identifies resources via the endpoint path, uses standard HTTP methods and keeps the requests stateless. Each request will contain all the information the API needs to process it. So the forecast request will have the latitude and longitude of the location you've selected as query parameters.

2. Endpoint Mapping

The app will be talking to two public GET endpoints and will have a separate endpoint in place for the Python backend that we're planning.

Public API Endpoints

Endpoint 1: Location Search

* URL: https://geocoding-api.open-meteo.com/v1/search
* HTTP method: GET
* Parameters: name, count, and optionally countryCode
* Purpose: Search for a city or location and return the matching locations (names, countries and coordinates).
* Example: https://geocoding-api.open-meteo.com/v1/search?name=Spartanburg&count=5&language=en&format=json

Endpoint 2: Weather Forecast

* URL: https://api.open-meteo.com/v1/forecast
* HTTP method: GET
* Parameters: latitude, longitude, current, daily, and timezone
* Purpose: To get the current weather and the daily forecast for the location you have selected.
* Example: https://api.open-meteo.com/v1/forecast?latitude=34.9496&longitude=-81.9320&current=temperature_2m,relative_humidity_2m,precipitation,weather_code,wind_speed_10m&daily=temperature_2m_max,temperature_2m_min,precipitation_probability_max&timezone=auto

Planned Python Backend Endpoint

Endpoint 3: Save Weather Search

* URL: /api/weather-searches
* HTTP method: POST
* Purpose: Takes a JSON object with the user's selected city, location (lat/long), temperature unit and search timestamp.
* Expected response: A JSON confirmation that the search details have been received.

The POST endpoint will be part of my own Python Flask backend (not the Open-Meteo API). The backend will be planned, it doesn't need to be implemented at all for this assignment.

The endpoint design is RESTful in that it uses resource based paths and standard HTTP methods. GET is used to retrieve a resource, POST to submit a new resource. The app will be stateless and will include all the information it needs in the request rather than relying on information from a prior request. PUT, PATCH and DELETE aren't required for the initial version as we're just going to be doing weather search get and post.

3. Data Flow Diagram (Textual)

The application will follow this process:

1. User opens the weather app and types a city into the search box.
2. The client checks that the city field has a valid value.
3. JavaScript uses the Fetch API to make an asynchronous GET request to the Open-Meteo geocoding API.
4. The API returns a JSON response with the matching locations and their coordinates.
5. The app shows the matching locations so you can pick the right city.
6. Once the user has picked a location, JavaScript makes another GET request to the Open-Meteo Weather Forecast API, using the latitude and longitude of the selected location.
7. The app takes the JSON response and displays the current conditions and the daily forecast in the UI.
8. If the user opts to save the search then the app takes the selected bits and turns them into a JSON payload and fires off a POST to the planned Flask backend.
9. The backend returns a JSON confirmation which the app can then display to the user.

Fetch API and Error Handling

I'll be using JavaScript's fetch() to make our asynchronous GET requests. We'll be checking the response.status via response.ok prior to calling response.json() to convert the response to JSON.

The requests will make use of the appropriate headers where required (e.g. Accept: application/json) and there will be a loading indicator in place whilst waiting for the API response.

If the request fails the app will simply display a helpful error message rather than leave the results area blank. Error handling will be in place for invalid locations, failed HTTP responses, missing data and network connectivity issues. A try/catch will be used to catch the errors and the loading indicator will be removed once the request has completed.

Dynamic Rendering and Conditional Styling

The app will use JavaScript to map the JSON response into HTML elements. The current weather will be displayed in weather cards and the daily forecast in a table or list.

The displayed information will include:

* City and country name
* Current temperature
* Relative humidity
* Precipitation
* Wind speed
* Daily high and low temperatures
* Daily precipitation probability

Conditional CSS classes will make the information far easier to interpret. For instance the app could apply a 'rainy' class when the precip is > 0, a 'hot' class when the temp is >= 85F and a 'cool' class when the temp is < 50F. Those are all application defined thresholds rather than actual warnings.

The interface will also use different styles for loading messages, successful results and error messages.

4. Form Handling

The app will have a search form and a control for selecting the temperature unit.

Form Controls and Validation

1. City Name

* Input type: Text
* Required: Yes
* Validation: The field may not be empty and must contain at least two non-space characters.
* Purpose: To identify the location the user wishes to search.

2. Country Code

* Input type: Text
* Required: No
* Validation: if entered must contain two alpha characters representing an ISO country code.
* Purpose: Helps to narrow down location search results.

3. Temperature Unit

* Input type: Dropdown selection
* Required: Yes
* Allowed values: Celsius or Fahrenheit.
* Validation: The selected value must be one of the two supported options.
* Purpose: Determines what temperature unit the forecast request will be in.

The form will be using JavaScript to validate the input prior to the request being sent. If the field is invalid it will display a message telling you what you need to do to correct it.

JSON Payload for the Python Backend

Following a successful location search the app will build up a JSON object containing the selected location and the search criteria.

Example JSON payload:

{
  "city": "Spartanburg",
  "country_code": "US",
  "latitude": 34.9496,
  "longitude": -81.9320,
  "temperature_unit": "fahrenheit",
  "requested_at": "2026-11-10T17:00:00"
}

The client will be sending this data to the Flask backend via a POST request with a Content-Type of application/json.

The backend contract will accept the JSON fields, validate the information, and return a response such as:

{
  "success": true,
  "message": "Weather search saved successfully."
}

The app will show the returned confirmation or an error message. The backend will revalidate the data that's been submitted rather than just doing client-side validation.

5. Technology Stack

The project will use the following technologies and tools:

* HTML5: Provides the structure of the UI (forms, buttons, tables, weather cards etc.)
* CSS3: Controls the layout, the colors, the responsive design, and the conditional styling.
* JavaScript: form validation, API calls, JSON processing, and dynamic rendering.
* Fetch API: Sends asynchronous HTTP requests to the public API and planned backend.
* Open-Meteo API: location search and weather forecast.
* Python Flask: planned backend framework to receive JSON submissions via a POST endpoint.
* Visual Studio Code: to write and organize the project files.
* Git and GitHub: for version control, project documentation, and repository hosting.
* Browser Developer Tools: for inspecting network requests, responses, and JavaScript errors.
* Postman: Used to test API endpoints and JSON responses.
* pytest: planned for testing the Python backend should it be implemented.

6. Implementation Steps

The project will be completed in the following order:

1. Set up the project: create a project folder, set up the GitHub repository, and create the initial HTML, CSS, and JS files.
2. Review the Open-Meteo Geocoding and Weather Forecast API documentation to establish what the required parameters are and what the response format will be.
3. Implement location search: create the city search form and use JavaScript Fetch to make a GET request to the Geocoding API.
4. Add loading and error states: show a loading message whilst the requests are processing and provide appropriate error messages if the requests fail.
5. Implement weather retrieval: use the selected location's latitude and longitude to make a GET request to the Weather Forecast API.
6. Render dynamic results: turn the JSON response into weather cards and forecast tables.
7. Add the form controls and validation: the country code and the temperature unit controls. Validate user input before submitting the request.
8. Plan the backend integration: define the JSON payload and the POST request contract for the Python Flask backend.
9. Implement conditional styling: add CSS classes that react to the weather values and differentiate successful results, loading states and errors.
10. Test the app: test valid and invalid city searches, API responses, missing data, network errors, form validation, and the backend request as intended using browser dev tools and Postman.
11. Complete final review and documentation: go through the app structure, confirm the API calls work, document the endpoints, and commit the finished planning doc to GitHub.

7. Conclusion

This project will show how a client side app can talk to an independent public API via REST interfaces. It'll do so via standard HTTP methods, resource-oriented endpoints, JSON, and stateless requests. It demonstrates the principles of interoperability.

The project will also demonstrate how a client can retrieve information, validate user input, dynamically display API results, and prepare structured JSON data for a separate Python backend. This design provides a foundation for developing a functional weather application while reinforcing REST API planning, communication, and integration concepts.

References

Open-Meteo. (n.d.). Weather forecast API documentation. https://open-meteo.com/en/docs

Open-Meteo. (n.d.). Geocoding API documentation. https://open-meteo.com/en/docs/geocoding-api

Public APIs. (n.d.). Public APIs. GitHub. https://github.com/public-apis/public-apis
