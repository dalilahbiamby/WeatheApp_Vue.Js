<template>
  <div id="app">
    <main>

      <!-- search bar section -->
      <div class="searchBox">
        <input
          type="text"
          class="searchBar" 
          placeholder="Search city..." 
          v-model="query"
          @keypress.enter="fetchWeather"
        />
      </div>

      <!-- front page app welcome message (only visible before searching), which can be translated between english and german -->
      <div v-if="!weather.main && !error" class="welcomePage">
        <div class="welcomeMessage"> {{ translations[language].welcomeMessage }} </div>
        <!-- the unit switch button -->
        <button @click="toggleUnit">
          {{ translations[language].switchUnit }} {{ unit === 'metric' ? '°F' : '°C' }}
        </button>

        <!-- the language translation button -->
        <button @click="toggleLanguage">
          {{ language === 'en' ? 'DE' : 'EN' }}
        </button>
      </div>

      <!-- error message when city is not found -->
      <div v-if="error" class="errorMessage">{{ error }}</div>

  

      <!-- the main weather display which is shown when the weather data is available -->
      <div v-if="weather.main" class="weatherWrap">

        <!-- location element which includes the city, country abbreviation and the day's date -->
        <div class="locationBox">
          <div class="location">
            <span class="city">{{ weather.name }}, </span>
            <span class="country">{{ weather.sys.country }}</span>
          </div>
        </div>

        <!-- section with all of the weather details-->
        <div class="weatherBox">

          <!--the temprature and weather description. displays the rounded temperature with the unit -->
          <div class="temp">
          {{ Math.round(weather.main.temp) }}<span class="unit">{{ unitLabel }}</span>
          </div>

        <div class="mainDetails">
              <!--high and low temperature for the day. arrows to symbolise the highest and lowest temperature of the day-->
            <div class="highLow">
             ↑ {{ Math.round(weather.main.temp_max) }}{{ unitLabel }} 
             ↓ {{ Math.round(weather.main.temp_min) }}{{ unitLabel }}
            </div>

            <!-- the translated weather condition-->
            <div class="weather">{{ translatedWeather }}</div>
        </div>

        <!-- the additional weather details -->
        <div class="detailsContainer"> 

          <!--Forecast details-->
          <!--precipitation in case that it rains or snows. disapeares from weather details if there is no rain and or snow-->
          <div v-if="weather.rain" class="precipitation">Rain: {{ weather.rain['1h'] || weather.rain['3h'] }} mm</div>
          <div v-if="weather.snow" class="precipitation">Snow: {{ weather.snow['1h'] || weather.snow['3h'] }} mm</div>

          <!--Details that stay permanenly-->
  
          <!-- feels like temperature -->
          <div class="feelsLike">
            {{ translations[language].feelsLike }}: {{ Math.round(weather.main.feels_like) }}{{ translations[language].unitLabel }}
          </div>

          <!-- the wind speed -->
          <div class="wind">
            {{ translations[language].wind }}: {{ weather.wind.speed }} {{ unit === 'metric' ? 'km/h' : 'mph' }}
          </div>

          <!-- the humidity percentage -->
          <div class="humidity">
            {{ translations[language].humidity }}: {{ weather.main.humidity }}%
          </div>
       
        <!--Sunset and sunrise times + translation element-->
        <div class="sunTimes">
          <p>{{ translations[language].sunrise }}: {{ weather.sunrise }}</p>
          <p>{{ translations[language].sunset }}: {{ weather.sunset }}</p>
        </div> 
    
      </div> <!-- end of the detailsContainer -->
 
    </div> <!-- the end of the weatherBox -->
 
      <div class="buttonWeatherPage">
        <!-- display the unit switch button for unit of temperature and language -->
        <button @click="toggleUnit">
          {{ translations[language].switchUnit }} {{ unit === 'metric' ? '°F' : '°C' }}
        </button>

        <!--The language translation button-->
        <button @click="toggleLanguage">
          {{ language === 'en' ? 'DE' : 'EN' }}
        </button>
      </div>

    </div> <!-- the end of the weatherWrap -->
      
    </main>
  </div>
</template>

<script>
// the component metadata. the component name is defined as WeatherApp
export default {
  name: "WeatherApp",
  // the defined data properties
  data() {
    return {
      apiKey: "33e017daa0b61e156809875c4f353ba1", // the API key from OpenWeather 
      urlBase: "https://api.openweathermap.org/data/2.5/", // the base URL for the API requests
      query: "", // allows for city names user input 
      weather: {}, // the object that allows for the storage of the weather data
      error: "",
      unit: "metric", // the default unit of measurements
      unitLabel: "°C", // the default unit of temperature
      language: "en", // the default language
      
      // the object that contains the translations from english to german
      translations: {
        // english translation
        en: {
          temp: "Temperature",
          error: "City not found. Please try again.",
          sunrise: "Sunrise",
          sunset: "Sunset",
          feelsLike:"Feels Like",
          wind:"Wind",
          humidity:"Humidity",
          welcomeMessage: "Welcome to my weather app!",
          weatherConditions: {
            Clear: "Clear",
            Clouds: "Cloudy",
            Rain: "Rain",
            Snow: "Snow",
            Drizzle: "Drizzle",
            Thunderstorm: "Thunderstorm",
            Mist: "Mist",
            Fog: "Fog",
            Dust: "Dust",
            Sand: "Sand",
            Tornado: "Tornado",
            }, 
        },
        // german translation
        de: {
          temp: "Temperatur",
        error: "Stadt nicht gefunden. Bitte versuchen Sie es erneut.",
        sunrise: "Sonnenaufgang",
        sunset: "Sonnenuntergang",
        feelsLike: "Gefühlt",
        wind: "Wind",
        humidity: "Luftfeuchtigkeit",
        welcomeMessage: "Willkommen bei meiner Wetter-App!",
        weatherConditions: {
          Clear: "Klar",
          Clouds: "Bewölkt",
          Rain: "Regen",
          Snow: "Schnee",
          Drizzle: "Nieselregen",
          Thunderstorm: "Gewitter",
          Mist: "Nebel",
          Fog: "Nebel",
          Dust: "Staub",
          Sand: "Sand",
          Tornado: "Tornado",
          },
       
        }
      },
    };
  },


  // the computed properties (what are computed properties)
 computed: {

  // this formats the date based on the rules of the chosen language. options are en - enlgish and de - german (deutsch)
  formattedDate() {
    return new Date().toLocaleDateString(this.language === "en" ? "en-US" : "de-DE", {
      day: "numeric",
      month: "short",
      year: "numeric",
    });
  },

  // this translates the weather data if it is fetched (if it exists). it calls on the data object to translate the from english to german and from german to english.
translatedWeather() {
    if (this.weather.weather && this.weather.weather[0])  {
      const weatherMain = this.weather.weather[0].main;
      return this.translations[this.language].weatherConditions[weatherMain] || weatherMain; 
    }
    return "";
  },

  
},


// these are the methods (what are methods?)
  methods: {

    // this fetches the weather data
  async fetchWeather() {

    // checks the user input - ensures valid input, sends an error message if it is not valid to avoid unnecessary api calls.
  if (!this.query.trim()) {
    this.error = this.translations[this.language].error;
    return;
  }

  // makes the API request - searches weather (C or F) through city name. sends the api key for authenticity, the waits for the response. 
  try {
    const res = await fetch(`${this.urlBase}weather?q=${this.query}&units=${this.unit}&APPID=${this.apiKey}`);
    
    // handles the API errors. checks if api request is successful. throws an error if the request fails. 
    if (!res.ok) {
      throw new Error("City not found");
    }
    // this parses the JSON response - from raw data fromt he API into. calls a method which processes and stores the weather data
    const data = await res.json();
    this.setResults(data);

    // this handles errors that occur (due to invalid input, network errors etc.) clears the weather data. displays an error message in en or de. and lastly logs the error for debugging.
  } catch (err) {
    this.weather = {};
    this.error = this.translations[this.language].error;
    console.error(err);
  }
},

  // this converts the sunrise and sunset time into a readable time format (hour and minutes, 12 hour format with am and pm).
 formatTime(timestamp, timezoneOffset) {
  const date = new Date((timestamp + timezoneOffset) * 1000);
  return date.toLocaleTimeString([], { 
    hour: "numeric", 
    minute: "2-digit", 
    hour12: true 
  });
},


  // translate the app between english and german.
   toggleLanguage() {
  this.language = this.language === "en" ? "de" : "en";
  if (this.weather.name) {
    this.weather = { ...this.weather };
  }
},


// switches between the unit of measurements between celcius and farenheit
toggleUnit() {
  if (this.unit === "metric") {
    this.unit = "imperial";
    this.unitLabel = "°F";
  } else {
    this.unit = "metric";
    this.unitLabel = "°C";
  }
  if (this.weather.name) {
    this.fetchWeather(); // Re-fetch weather data with the new unit
  }
},

// translates the welcome message between English and German
toggleLanguage() {
  this.language = this.language === "en" ? "de" : "en";
},


  // this updates the weather data after recieving the results from the api
   setResults(results) {
  if (results.sys && results.sys.sunrise && results.sys.sunset) {
    this.weather = {
      ...results,
      sunrise: this.formatTime(results.sys.sunrise, results.timezone),
      sunset: this.formatTime(results.sys.sunset, results.timezone)
    };
  } 
  else {
    this.weather = results;
  }
  this.error = "";
},

  }
};







</script>

<style>

/* base border and margin styling for the entire page */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

/* styling that affects the entire web application page */
body {
  font-family: 'Inter', sans-serif;
  background-color: hsl(0, 0%, 100%);
  margin: 0;
  height: 100vh; 
  display: flex;
  align-items: center; 
  justify-content: center; 
    padding-left: 30px;
  padding-right: 30px;
}


/* styles of the main container for the app */
#app {
  width: 100%; 
  max-width: 1200px; 
  padding: 20px;
   align-items: center; 
  justify-content: center;
  text-align: center; 
  background-color: rgb(232, 232, 229); 
  border-radius: 8px; 
}

/* container for the search bar section */

.searchBox {
  width: 100%;
 
  padding-top: 50px;
}

/* styling for the search inout field  */
.searchBox .searchBar {
  width: 100%;
  padding: 10px;
  font-size: 18px;
  border-radius: 12px;
  border: 1px solid #ccc;
  transition: 0.3s;
}

/* this is a focus effect for the search bar, which chnages the color when clicked */
.searchBox .searchBar:focus {
  border-color: #cebac9;
  background-color: #ffffff;
}

/* styling for the welcome section */
.welcomePage .welcomeMessage {
  font-size: 24px;
  margin: 20px 0;
  text-align: center;
  padding-top: 100px;
  padding-bottom: 100px;
}

/* this adds spacing above the weather information section */
.weatherWrap {
  padding-top: 30px;
}

/* styling for the location display, which includes the city and country */
.locationBox .location {
  font-size: 24px;
  font-weight: bold;
}

/* styling for the city name */
.city {
  font-size: 24px;
  font-weight: bold;
}

/* stylign for the country name abbreviation */
.country {
  font-size: 20px;
  font-weight: bold;
}

/* styling for the temperature display */
.weatherBox .temp {
  font-size: 80px;
  font-weight: bold;
  color: #000000;
  line-height: 1.5;
}

/* styling for both units of temperature */
.unit {
  font-size: 30px;
  vertical-align: baseline;
}

/* styling for the highest and lowest temperature */
.highLow {
  font-size: 12px;
  color: #000000;

}

/* styling for the weather condition text */
.weatherBox .weather {
  font-size: 28px;
  font-weight: bold;
  color: #000000;
;
}

/* styling for the additional weather information */
.weatherBox .feelsLike,
.weatherBox .wind,
.weatherBox .humidity {
  font-size: 14px;
}

/* styling for all the buttons, both for the languages and the units of temperature */
button {
  margin: 5px;
  padding: 5px;
  border: none;
  cursor: default;
  border-radius: 8px;
  background-color: #cba9c3;
  color: black;
  font-size: 10px;
}

/* styling for the button hover effect, it changes color when hovered over */
button:hover {
  background-color: #e7cbe2;
}

/* adds spacing above the buttons */
.buttonWeatherPage{
  padding-top: 40px;
}

/* styling for the error message */
.errorMessage {
  color: rgb(0, 0, 0);
  font-size: 16px;
  margin-top: 10px;
}

/* styling for the additional weather details container */
.detailsContainer {
font-size: 14px;
line-height: 1.5;
}

/* styling for the main weather details section */
.mainDetails {
  line-height: 1.5;
}

</style>

