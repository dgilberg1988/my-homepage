# Open-Meteo Weather API Implementation

## Brief Overview

**Open-Meteo** is an excellent choice for your weather widget! It's a completely **free, open-source weather API** that requires **no API key**, no registration, and has no rate limits for reasonable use. It provides high-quality weather data from national weather services worldwide.

### Key Advantages:
- ✅ **No API key required**
- ✅ **Completely free**
- ✅ **No registration needed**
- ✅ **High-quality data** from official weather services
- ✅ **Fast and reliable**
- ✅ **Well-documented API**

## Step-by-Step Implementation Plan

### Step 1: Understand the API Structure
Open-Meteo uses a simple REST API with these main endpoints:
- **Current weather**: `https://api.open-meteo.com/v1/forecast`
- **Parameters**: latitude, longitude, current weather variables
- **Response**: JSON format with weather data

### Step 2: Get Location Coordinates
You'll need latitude and longitude for your desired location:
- Use a geocoding service (also free from Open-Meteo)
- Or manually find coordinates for your city

### Step 3: Build the API Request
Structure your request with the required parameters for current weather data.

### Step 4: Create the HTML Structure
Build a simple container for your weather widget.

### Step 5: Implement JavaScript to Fetch Data
Write the code to call the API and display the results.

### Step 6: Style Your Widget
Add CSS to make it look good on your website.

## Complete Implementation

Here's a full working example:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Open-Meteo Weather Widget</title>
    <style>
        .weather-widget {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 25px;
            border-radius: 15px;
            max-width: 350px;
            margin: 20px auto;
            text-align: center;
            font-family: 'Arial', sans-serif;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
        }
        
        .location {
            font-size: 1.3em;
            font-weight: bold;
            margin-bottom: 15px;
            opacity: 0.9;
        }
        
        .temperature {
            font-size: 3em;
            font-weight: bold;
            margin: 15px 0;
        }
        
        .weather-info {
            display: flex;
            justify-content: space-around;
            margin-top: 20px;
            font-size: 0.9em;
        }
        
        .weather-item {
            text-align: center;
        }
        
        .weather-label {
            opacity: 0.8;
            font-size: 0.8em;
            margin-bottom: 5px;
        }
        
        .weather-value {
            font-weight: bold;
        }
        
        .loading {
            opacity: 0.7;
        }
        
        .error {
            background: #e74c3c;
            color: white;
        }
    </style>
</head>
<body>
    <div class="weather-widget" id="weather-widget">
        <div class="location" id="location">Seattle, WA</div>
        <div class="temperature loading" id="temperature">Loading...</div>
        <div class="weather-info" id="weather-info">
            <div class="weather-item">
                <div class="weather-label">Humidity</div>
                <div class="weather-value" id="humidity">--%</div>
            </div>
            <div class="weather-item">
                <div class="weather-label">Wind Speed</div>
                <div class="weather-value" id="windspeed">-- mph</div>
            </div>
            <div class="weather-item">
                <div class="weather-label">Feels Like</div>
                <div class="weather-value" id="apparent-temp">--°</div>
            </div>
        </div>
    </div>

    <script>
        // Configuration - Change these coordinates for your city
        const LATITUDE = 47.6062;  // Seattle latitude
        const LONGITUDE = -122.3321; // Seattle longitude
        const CITY_NAME = "Seattle, WA";
        
        async function getWeatherData() {
            try {
                // Open-Meteo API call - no API key needed!
                const response = await fetch(
                    `https://api.open-meteo.com/v1/forecast?latitude=${LATITUDE}&longitude=${LONGITUDE}&current=temperature_2m,relative_humidity_2m,apparent_temperature,wind_speed_10m&temperature_unit=fahrenheit&wind_speed_unit=mph&timezone=auto`
                );
                
                if (!response.ok) {
                    throw new Error('Weather data not available');
                }
                
                const data = await response.json();
                const current = data.current;
                
                // Update the display
                document.getElementById('location').textContent = CITY_NAME;
                document.getElementById('temperature').textContent = `${Math.round(current.temperature_2m)}°F`;
                document.getElementById('humidity').textContent = `${current.relative_humidity_2m}%`;
                document.getElementById('windspeed').textContent = `${Math.round(current.wind_speed_10m)} mph`;
                document.getElementById('apparent-temp').textContent = `${Math.round(current.apparent_temperature)}°F`;
                
                // Remove loading class
                document.getElementById('temperature').classList.remove('loading');
                
            } catch (error) {
                console.error('Error fetching weather data:', error);
                
                // Show error state
                const widget = document.getElementById('weather-widget');
                widget.classList.add('error');
                document.getElementById('temperature').textContent = 'Error';
                document.getElementById('location').textContent = 'Unable to load weather';
            }
        }
        
        // Load weather data when page loads
        getWeatherData();
        
        // Optional: Refresh data every 10 minutes
        setInterval(getWeatherData, 10 * 60 * 1000);
    </script>
</body>
</html>
```

## Customization Steps

### Step 1: Change Location
Find your city's coordinates and update these variables:
```javascript
const LATITUDE = 40.7128;   // New York latitude
const LONGITUDE = -74.0060; // New York longitude  
const CITY_NAME = "New York, NY";
```

### Step 2: Add More Weather Data
Open-Meteo supports many parameters. Add to the API URL:
- `precipitation` - Current precipitation
- `weather_code` - Weather condition code
- `cloud_cover` - Cloud coverage percentage

### Step 3: Get Coordinates for Any City
Use Open-Meteo's geocoding API:
```javascript
// Example: Get coordinates for any city
async function getCityCoordinates(cityName) {
    const response = await fetch(
        `https://geocoding-api.open-meteo.com/v1/search?name=${cityName}&count=1`
    );
    const data = await response.json();
    return data.results[0]; // Returns {latitude, longitude, name}
}
```

## Benefits of This Implementation

1. **No API key hassles** - Works immediately
2. **Reliable data** - From official weather services
3. **Fast loading** - Optimized API responses
4. **Customizable** - Easy to modify and extend
5. **Mobile-friendly** - Responsive design included

Would you like me to help you customize this further or add any specific features like a 7-day forecast or weather icons?