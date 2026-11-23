# WeatherGUI

A Java Swing desktop app called **WeatherHeart**. Type in a location and it shows the current weather, a multi-day forecast and any active weather alerts, using data from [WeatherAPI](https://www.weatherapi.com/).

## Features

- Welcome screen with an **ENTER** button that opens the weather view
- Location search (city name, postcode, etc.), submitted with the **SAVE** button
- Current conditions: temperature (°C), condition text, humidity (%) and wind speed (km/h)
- Day or night indicator with a matching sun or moon icon
- Forecast cards for the days ahead: date, max and min temperature, wind speed and precipitation (mm)
- Weather alerts panel (headline, description, instructions, severity, effective and expiry times), or "No alerts found."
- Home button to return to the welcome screen

## Project Structure

```
WeatherGUI-main/
├── WeatherUI.java     # Main class: Swing UI, API request and JSON parsing
├── WeatherUI.form     # NetBeans GUI Builder form
├── weather_ui.xml     # Ant build file (compile / run / clean)
└── Images/            # Icons and background images used by the UI
```

## Requirements

- Java JDK 8 or later (the code uses lambdas)
- The [org.json](https://github.com/stleary/JSON-java) library (`json-*.jar`)
- A free API key from [weatherapi.com](https://www.weatherapi.com/)
- An internet connection

## Setup

1. **Get an API key.** Sign up at weatherapi.com and copy your key.
2. **Add your key.** In `WeatherUI.java`, the `main` method currently contains an API key as a string. Replace it with your own key.
3. **Add the JSON library.** Download the org.json jar and put it in a `lib/` folder, or add it to your IDE's libraries.

> **Security note:** Do not commit a real API key to a public repository. If a key is already in your commit history, revoke it in your WeatherAPI dashboard and generate a new one. A safer approach is to read it from an environment variable, for example `System.getenv("WEATHER_API_KEY")`.

## Running

### NetBeans (recommended)

1. Open the folder as a project in NetBeans (or create a new Java project and add the files).
2. Add the org.json jar to the project libraries.
3. Make sure the `Images` folder is on the classpath, because the app loads icons with `getResource("/Images/...")`.
4. Run `WeatherUI.java`.

### Command line

From the project root, with the jar in `lib/`:

```bash
mkdir -p build/classes
javac -cp "lib/json-2024303.jar" -d build/classes WeatherUI.java
cp -r Images build/classes/
java -cp "build/classes:lib/json-2024303.jar" WeatherUI
```

On Windows, use `;` instead of `:` in the classpath.

The included `weather_ui.xml` Ant script expects sources in `src/` and the jar at `lib/json-2024303.jar`. Adjust the paths or move the files to match that layout.

## How It Works

1. The user enters a location and clicks **SAVE**.
2. `WeatherUI.run()` URL-encodes the location and calls the WeatherAPI forecast endpoint with `days=7` and `alerts=yes`.
3. The JSON response is parsed with `org.json`, and the labels on screen are updated for temperature, condition, humidity, wind, day or night, forecast and alerts.

## Known Issues

- The API key is hard-coded (see the security note above).
- The request uses `http://`. Switching to `https://` is recommended.
- Errors such as an invalid location or a failed request are only printed to the console, not shown in the UI.
- The alert code reads `instruction` and other fields directly, which can throw an exception if the API leaves them out.
- The last forecast card has a label typo ("Min Temperatue") and shows the max temperature under the min label.
- "Temperatue" and "loctaion" are misspelled in some UI labels.

## Tech Stack

- Java
- Java Swing (NetBeans GUI Builder)
- org.json
- WeatherAPI

## License

No license has been specified yet. Consider adding one, such as MIT.
