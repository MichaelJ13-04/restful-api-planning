# restful-api-planning
REST API Planning – Weather Forecast Client

Project Overview

This is the design and development plan for a client side weather app using the public Open-Meteo weather API. The user will be able to search for a city and view the current weather as well as the forecast.

This project demonstrates REST API concepts, client-side communication, JSON data processing, form validation, and dynamic user interface development.

API Selection

The selected API is the Open-Meteo Weather API.

Open-Meteo provides weather forecasts and current weather information in JSON format. It suits this project because it is publicly accessible and does not require an API key for non-commercial use.

The application will use:

* Geocoding API: Searches for a city and retrieves its geographic coordinates.
* Weather Forecast API: Uses latitude and longitude to retrieve current weather conditions and forecasts.

REST Endpoint Mapping

Column 1	Column 2	Column 3
Endpoint	HTTP Method	Purpose
/v1/search	GET	Searches for a city and returns location information.
/v1/forecast	GET	Retrieves current weather and forecast data.
/api/weather-searches	POST	Planned Flask backend endpoint for receiving saved weather-search information.


The design follows REST principles with resource identification, stateless requests, and standard HTTP methods. GET retrieves information, and POST submits information to the planned backend.

Data Flow

1. The user enters a city name into the search form.
2. JavaScript validates the user’s input.
3. The Fetch API sends a GET request to the Open-Meteo Geocoding API.
4. The application receives location information in JSON format.
5. The user selects a location.
6. A second GET request retrieves the weather forecast using the location’s coordinates.
7. JavaScript processes the response and displays the information in weather cards or tables.
8. Loading indicators and error messages provide feedback during API requests.
9. If the user saves a search, the application prepares a JSON payload for the planned Flask backend.

Form Handling and Validation

The application will include the following controls:

* City Name: Required text field with a minimum of two non-space characters.
* Country Code: Optional field that must contain two alphabetic characters if provided.
* Temperature Unit: Required dropdown with Celsius and Fahrenheit options.

JavaScript validates the form before sending requests. The selected location and preferences are transformed into JSON before submission to the planned backend.

Dynamic Rendering and Conditional Styling

Weather information will be displayed dynamically using JavaScript. The interface will include:

* City and country
* Current temperature
* Relative humidity
* Precipitation
* Wind speed
* Daily high and low temperatures
* Precipitation probability

CSS classes provide conditional styling. For example, rainy conditions, hot temperatures, and cool temperatures receive different visual styles.

Technology Stack

* HTML5
* CSS3
* JavaScript
* Fetch API
* Open-Meteo Weather API
* Python Flask (planned backend)
* Visual Studio Code
* Git and GitHub
* Postman
* pytest (planned backend testing)

Implementation Plan

1. Create the project folder and GitHub repository.
2. Review the Open-Meteo API documentation.
3. Create the HTML, CSS, and JavaScript files.
4. Implement the city search using Fetch and GET requests.
5. Add loading indicators and error handling.
6. Retrieve and display weather forecast information.
7. Add form controls and input validation.
8. Define the JSON payload and planned Flask POST endpoint.
9. Add conditional CSS styling.
10. Test API requests, responses, and form validation.
11. Complete documentation and review the project.

Testing Strategy

Testing will include valid and invalid city searches, missing form information, successful API responses, network errors, and JSON response handling.

Browser Developer Tools and Postman will help inspect requests and responses. pytest may be used if the planned Flask backend is implemented.

Project Status

Current status: Planning and documentation.

The client-side application and Flask backend are planned but have not yet been implemented.

References

* Open-Meteo Weather API Documentation
* Open-Meteo Geocoding API Documentation
* Public APIs GitHub Repository
