# WeatherApp_Vue.Js (MyWeatherApp) Installation & Run Manual

## 1.	Introduction & Purpose

MyWeatherApp is a web-based application that provides users with access to reliable real-time weather forecasts for all cities around the world, with the integration of the OpenWeather API. This documentation’s purpose is to inform the public on how to install and run the application through a frictionless process.

## 2.	Installation Prerequisites

  ### 2.1	System Requirements
  
  - Operating System: Windows, macOS, or Linux
  
  - Internet Connection is required for the API to return data and for the package installation
  
  ### 2.2. Software Requirements
  
  - Install VS Code as the code editor (or any preferred code editor)
  
  - Install Node.js due to Vue.js development tools’ requirements

    - Verify the installation in the command prompt or terminal using the following scripts:

    - (If correctly installed version numbers will be returned)

      - node -v
    
      - npm - v
  
  - Install Git as for version control 

    - Verify the installation in the command prompt or terminal using the following scripts:

    - (If correctly installed version number will be returned)

      - git –version
  
  - Install Chrome or Firefox as the web browser
  
## 2.3 Programming Languages Used  
  
  - HTML: Web page structure
  
  - CSS: Web page styling
  
  - Vue.js: Web page logic - JavaScript based framework

## 3.	The Installation & Run Steps
  
  ### 3.1. Clone the Repository via the command prompt or the terminal with script:
  
  - git clone https://github.com/dalilahbiamby/WeatheApp_Vue.Js
  
  - cd WeatherApp_Vue.Js
  
  ### 3.2. 	Install Dependencies via the command prompt or the terminal with script:
  
  - npm install
  
  ### 3.5. Run the application via the command prompt or the terminal with script:
  
  - npm run dev
  
    - Open the localhost url that gets returned in the command prompt/terminal.
    
      - The application will open in default browser

      - Tip: press command and left click the localhost url to open it in the browser

## 4.	Testing 
  
  - Once the application is live, type a city name into the search bar

  - Ensure that all necessary weather data are displayed correctly

  - Once the test is past, the weather application is ready for use, enjoy!

## 5.	Troubleshooting

  - A common issue is Node.js version incompatibility, the script npm install will fail, open the command prompt or terminal to make sure to check its current version and update it on from the Node.js website.
  
  - Another common issue is Vue.js not being recognised, the script vue –version will fail, open the command prompt or terminal to install Vue Vite globally using the script: npm install -g @vite.
  
  -  If the localhost port is no longer updating and fetching data, clear the commmand prompt or terminal (press on the three dots on the right side of the terminal, then press on clear terminal), and run the application again.
