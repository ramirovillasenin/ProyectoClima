# 🌤️ WEATHER APP API

A REST API built with **Spring Boot** that returns the current weather for any city in the world, combining the geocoding API and the meteorological API from [Open-Meteo](https://open-meteo.com/).

---

## 📋 Description

The user sends a city name and the API returns the current temperature and wind speed. Internally, the system makes two calls:

1. **Geocoding API** → converts the city name into coordinates (latitude and longitude)
2. **Open-Meteo API** → fetches the current weather based on those coordinates

---

## 🚀 Tech Stack

- Java 17+
- Spring Boot 3.x
- Spring Web (RestTemplate)
- Maven
- [Open-Meteo API](https://open-meteo.com/) *(free, no API key required)*

---

## 📁 Project Structure

```
src/main/java/org/ramiro/clima/
├── controller/
│   └── ClimaController.java       # Exposes the GET /clima endpoint
├── model/
│   ├── ClimaDTO.java              # Response returned to the client
│   ├── ClimaResponse.java         # Auxiliary model
│   ├── CurrentWeather.java        # Current weather data
│   ├── GeocodingResponse.java     # Geocoding API response
│   ├── OpenMeteoResponse.java     # Open-Meteo API response
│   └── ResultadoCiudad.java       # City coordinates
├── service/
│   └── ClimaService.java          # Business logic
└── ProyectoCLimaApplication.java  # Entry point
```

---

## ⚙️ Installation & Setup

### Prerequisites

- Java 17 or higher
- Maven installed (or use the included wrapper `./mvnw`)

### Steps

1. Clone the repository:

```bash
git clone https://github.com/ramirovillasenin/ProyectoCLima.git
cd ProyectoCLima
```

2. Build and run:

```bash
./mvnw spring-boot:run
```

> On Windows: `mvnw.cmd spring-boot:run`

3. The API will be available at `http://localhost:8080`

---

## 📡 API Usage

### Endpoint

```
GET /clima?ciudad={city_name}
```

### Examples

```bash
curl "http://localhost:8080/clima?ciudad=Buenos Aires"
curl "http://localhost:8080/clima?ciudad=Madrid"
curl "http://localhost:8080/clima?ciudad=Tokyo"
```

### Successful response

```json
{
  "city": "Buenos Aires",
  "temperature": 24.5,
  "wind": 12.3
}
```

| Field | Type | Description |
|---|---|---|
| `city` | String | Name of the queried city |
| `temperature` | double | Current temperature in °C |
| `wind` | double | Wind speed in km/h |

### Error — city not found

```json
{
  "status": 500,
  "message": "Ciudad no encontrada"
}
```

---

## 🔄 Internal Flow

```
Client
  └─► GET /clima?ciudad=Buenos Aires
        └─► ClimaController
              └─► ClimaService
                    ├─► Geocoding API  →  lat, lon
                    └─► Open-Meteo API →  temperature, wind speed
                          └─► ClimaDTO (JSON)
```

---

## 📌 Notes

- The Open-Meteo API is **free and requires no API key**.
- Temperature is returned in **°C** and wind speed in **km/h** (Open-Meteo default settings).
- If the city is not found by the geocoding API, a `RuntimeException` is thrown with the message `"Ciudad no encontrada"`.

---

## 👤 Author

**Ramiro** — [@ramirovillasenin](https://github.com/ramirovillasenin)
