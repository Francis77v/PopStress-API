PopStress API 🌍💥

PopStress API is a backend service that estimates the maximum sustainable population of countries based on factors like land area, food production, water availability, infrastructure, and climate. It also evaluates population stress levels to indicate how close a country is to reaching its carrying capacity.

This project showcases data ingestion, backend analytics, and REST API design — perfect for a portfolio demonstrating real-world backend skills.

Table of Contents

Features
Data Sources
Database Schema
Calculation Methodology
API Endpoints
Tech Stack
Getting Started


Features

Estimate maximum sustainable population for each country
Determine stress levels: Low / Moderate / High / Critical
Factor breakdown (food, water, infrastructure)
Optional historical simulation for future population trends
Expose a REST API for querying data

| Factor                    | Source                                                                                                                                                     |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Population & Land Area    | [REST Countries API](https://restcountries.com/), [UN WPP](https://population.un.org/wpp/), [World Bank](https://data.worldbank.org/indicator/SP.POP.TOTL) |
| Food & Arable Land        | [FAO](https://www.fao.org/faostat/en/#data/FBS)                                                                                                            |
| Freshwater                | [FAO Aquastat](https://www.fao.org/aquastat/en/), [World Bank](https://data.worldbank.org/indicator/ER.H2O.INTR.PC)                                        |
| Economic / Infrastructure | [World Bank](https://data.worldbank.org/), [UN HDI](https://hdr.undp.org/data-center/documentation-and-downloads)                                          |
| Climate                   | [NASA Earth Data](https://earthdata.nasa.gov/), [World Bank Climate Data](https://climatedataapi.worldbank.org/)                                           |
| Hazard Risk (Optional)    | [UNDRR](https://www.undrr.org/data), [GDIS](https://www.nature.com/articles/s41597-021-00846-6)                                                            |

Database Schema

countries

Column	Type	Description
Id	INT	Primary key
Name	VARCHAR	Country name
LandAreaKm2	FLOAT	Total land area
CurrentPopulation	BIGINT	Current population
GDPPerCapita	FLOAT	Economic capacity
ArableLandKm2	FLOAT	Arable land area
FreshwaterKm3	FLOAT	Renewable freshwater
UrbanizationPct	FLOAT	Percent urban population

population_estimates

Column	Type	Description
CountryId	INT	FK to countries
MaxPopulation	BIGINT	Estimated max population
StressLevel	VARCHAR	Low / Moderate / High / Critical
DateCalculated	DATETIME	Timestamp
Calculation Methodology

The maximum population is estimated as the minimum of multiple constraints:

MaxPopulation = MIN(
    FoodCapacity,
    WaterCapacity,
    LandDensityCapacity,
    InfrastructureCapacity
)


StressLevel is assigned:

Low: Current population < 50% of max

Moderate: 50–75% of max

High: 75–100% of max

Critical: Current population > max capacity

Factor Examples:

FoodCapacity = TotalCaloriesProduced / AvgCaloriesPerPerson

WaterCapacity = FreshwaterAvailable / AvgWaterPerPerson

LandDensityCapacity = LandArea * MaxDensityPerKm2

InfrastructureCapacity = CurrentPopulation * InfrastructureFactor

API Endpoints
Endpoint	Method	Description
/api/population-estimate/{countryCode}	GET	Returns estimated max population and stress level for a country
/api/population-estimate	GET	Returns estimates for all countries
/api/population-estimate/simulate-growth	GET	Simulates stress levels for future years
/api/population-estimate/top-stress	GET	Lists countries under highest stress

Example Response:

{
  "Country": "India",
  "CurrentPopulation": 1400000000,
  "EstimatedMaxPopulation": 1750000000,
  "StressLevel": "High",
  "Breakdown": {
    "FoodLimit": 1800000000,
    "WaterLimit": 1600000000,
    "InfrastructureLimit": 1500000000
  }
}

Tech Stack

Backend: C# / ASP.NET Core

Database: PostgreSQL (PostGIS optional for geospatial data)

Data Ingestion: HttpClient, CSV/JSON parsers, scheduled background service

API Design: RESTful endpoints, DTOs for responses

Optional: Docker for deployment

Getting Started

Clone the repo:

git clone https://github.com/yourusername/popstress-api.git


Set up PostgreSQL database and apply migrations

Download datasets from sources above and import into database

Run the API:

dotnet run


Access Swagger UI (if configured) to test endpoints
