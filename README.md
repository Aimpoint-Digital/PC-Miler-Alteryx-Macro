# PC Miler Truck Mileage Alteryx Macro

- [PC Miler Truck Mileage Alteryx Macro](#pc-miler-truck-mileage-alteryx-macro)
  - [Introduction](#introduction)
  - [Getting Started](#getting-started)
  - [Input](#input)
  - [Outputs](#outputs)
    - [Route / Journey level data (R)](#route--journey-level-data-r)
    - [Leg level data (L)](#leg-level-data-l)
    - [Stop level data (S)](#stop-level-data-s)
    - [Raw JSON Output (O)](#raw-json-output-o)
  - [Sample Inputs & Outputs](#sample-inputs--outputs)
  - [Demo Workflow](#demo-workflow)
  - [Credit](#credit)


## Introduction
The PC Miler Truck Mileage tool is an Alteryx Macro designed to return truck mileage, costs and transit time for given routes. It provides these details at a route and leg level and provides additional stop level information. The PC Miler API has the ability to support other regions, but this release of the macro supports only the default region, North America. Future releases may expand this functionality.

<p align="center">
<img src="https://github.com/Aimpoint-Digital/PC-Miler-Alteryx-Macro/blob/8e7eb769e80f62dfc28c2e127dc47a5de1e7a69f/Images/pc_miler.png">
</p>

Documentation for the API being used can be found here: https://developer.trimblemaps.com/restful-apis/routing/route-reports/overview/

## Getting Started
1. Have a vaild Alteryx desktop installation (must be atleast version 2020.3).
2. [**Download the yxi**](https://github.com/Aimpoint-Digital/PC-Miler-Alteryx-Macro/blob/e2ef1cb580100da329664a26f30fc58db1584783/Installer/PC%20Miler%20Truck%20Mileage.yxi) and install the macro (double click the yxi file once downloaded to install).
3. Close and reopen Alteryx for the macro to be added to the Connectors tab.
4. Drag the macro in and provide:
   1. A valid PC Miler API Token Key (need to have purchased PC Miler Web Services Premium).
   2. Input data; structure must be as indicated below.

## Input
Input data must contain a row per stop per journey and contain the following columns: 

Input Column | Description
--- | ---
RouteID | Identifier for a unique route journey.
SeqID | Identifier for the sequence that stop is in the journey.
Lat | Latitude of the stop.
Lon | Longitude of the stop.


## Outputs
There are four outputs of this macro: **Route**, **Leg**, **Stop**, **Raw**.
  

### Route / Journey level data (R)
  
Contains information regarding the entire journey provided. 

Output Column | Description
--- | ---
RouteID | Provided RouteID
CostMile | Calculated route cost in dollars (based upon fuel cost, labour cost and other misc costs).
Estghg | Estimated greenhouse gas emmission for the journey.
Hours | Estimated journey duration in a timestamp format, XX:YY where XX = Hours and YY = Minutes.
Miles | Estimate mileage of the journey.
Tolls | Estimated toll cost in dollars for the journey.


### Leg level data (L)

Contains information regarding the legs (journey segment of one stop to another) of the journeys provided. 

Output Column | Description
--- | ---
RouteID | Provided RouteID
LegPartID | Field indicating which leg of the route the record relates to. Denoted as X-Y where X = FromStop and Y = ToStop. Stops indexed/starting from 0.
CostMile | Calculated leg cost in dollars (based upon fuel cost, labour cost and other misc costs).
Hours | Estimated leg duration in a timestamp format, XX:YY where XX = Hours and YY = Minutes.
Miles | Estimate mileage of the leg.
Tolls | Estimated toll cost in dollars for the leg.


### Stop level data (S)

Contains information regarding the stops of the journeys provided. If a road/address is not found for the lat-lon combination provided, PC Miler will look within a certain distance to try and find the next nearest road it can use for routing.

Output Column | Description
--- | ---
RouteID | Provided RouteID
StopID | Sequential ID for each stop of the journey, indexed/starting from 0.
Address_AbbreviationFormat | Code indicating address abbreviation. Refer to API docs.
Address_City | City of the stop location.
Address_Country | Full country name of the stop location.
Addresss_CountryAbbreviation | Country abbreviation of the stop location.
Address_CountryPostalFilter | Code indicating postal filler. Refer to API docs.
Address_County | Full county name of the stop location.
Address_SPLC | Refer to API docs.
Address_State | State abbreviation of the stop location.
Address_StateAbbreviation | State abbreviation of the stop location. Is a duplicate of Address_State.
Address_StateName | Full state name of the stop location.
Address_StreetAddress | Street address of the stop location.
Address_Zip | Zip code of the stop location.
ConfidenceLevel | Confidence of the address compared to the latitude and longitude provided. The confidence will be lower the further (distance-wise) PC Miler had to look to find a legitimate road/address from the lat-lon provided.
Coords_Lat | Latitude coordinate of the stop location.
Coords_Lon | Longitude coordinate of the stop location.
CrossStreet | Refer to API docs.
DistanceFromRoad | How far the lat-lon provided for the stop location was from a road (in Miles). 
Label | Refer to API docs.
PlaceName | Refer to API docs.
Region | Code representing the region of the stop location. Refer to API docs.
SpeedLimitInfo | Refer to API docs.
TimeZone | Timezone of the stop location.


### Raw JSON Output (O)

Contains the raw API call output in case any additional JSON parsing is needed or debugging is required. 

Output Column | Description
--- | ---
RouteID | Provided RouteID
Concat_latlon | Concatenated lat-lon sequence for the route/journey which is provided to the API.
Mileage_API_Call | API call being made to PC Miler.
DownloadData | Raw JSON response from the PC Miler API call.
DownloadHeaders | Response header from PC Miler API call; will indicate if the API call was successful or not, and the error code for failure.

## Sample Inputs & Outputs

**Input (I)**

![Input Structure](https://github.com/Aimpoint-Digital/PC-Miler-Alteryx-Macro/blob/8e7eb769e80f62dfc28c2e127dc47a5de1e7a69f/Images/PC%20MILER%20Input.PNG)


**Output: Route / Journey (R)**

![Route Level Output](https://github.com/Aimpoint-Digital/PC-Miler-Alteryx-Macro/blob/8e7eb769e80f62dfc28c2e127dc47a5de1e7a69f/Images/PC%20MILER%20Route%20Output.PNG)


**Output: Leg (L)**

![Leg Level Output](https://github.com/Aimpoint-Digital/PC-Miler-Alteryx-Macro/blob/8e7eb769e80f62dfc28c2e127dc47a5de1e7a69f/Images/PCMILER%20Leg%20Data.PNG)


**Output: Stop (S)**
Column subset shown

![Stop Level Output](https://github.com/Aimpoint-Digital/PC-Miler-Alteryx-Macro/blob/8e7eb769e80f62dfc28c2e127dc47a5de1e7a69f/Images/PC%20MILER%20Stop%20data.PNG)

**Output: Raw JSON (O)**

![Raw JSON Output](https://github.com/Aimpoint-Digital/PC-Miler-Alteryx-Macro/blob/8e7eb769e80f62dfc28c2e127dc47a5de1e7a69f/Images/PC%20MILER%20Raw%20Output.PNG)


## Demo Workflow
A demo workflow can be found within this project to better assist in getting started: 
[Demo Workflow](https://github.com/Aimpoint-Digital/PC-Miler-Alteryx-Macro/blob/8e7eb769e80f62dfc28c2e127dc47a5de1e7a69f/Example%20Workflow/PC%20Miler%20Example%20Workflow%201.yxzp)


## Credit
Credit to Mina Ozgen and Bona Wang for the development of the PC Miler Alteryx Macro.