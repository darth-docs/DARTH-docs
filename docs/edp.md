# EDP (Energy Data Platform)

## Database

The Energy Data Platform (EDP) is a comprehensive, research-grade dataset of high-resolution (5-minute) energy data from 951 Australian sites, spanning 2018–2025. It integrates:

- **Energy measurements:** solar PV generation, household consumption, battery charging/discharging, voltage, and current.

- **Site metadata:** system specifications, inverter/subarray details, battery capacity, and connection type.

- **Survey responses:** household characteristics, occupancy, appliances, heating/cooling, and energy-related behaviors.

### Key Research Applications

1. **Energy Behavior Analysis**

    - Study consumption patterns, appliance usage, and load profiles.

    - Investigate temporal trends, daily routines, and peak demand behavior.

2. **Distributed Energy Resources (DER) Research**

    - Evaluate solar PV generation and battery storage performance.

    - Model and optimize energy systems at household or community scale.

3. **Socio-Technical Insights**

    - Link energy behavior to household demographics, dwelling type, and occupancy.

    - Explore the influence of lifestyle and building characteristics on energy use.

### Structure & Data 

It consists of 5-minute energy data for 951 sites, spanning from 2018 to the end of October 2025. The database includes four types of tables, with one set being monthly partitions of all the data.

|  Database Table | Columns | 
|-------------------|--------------------------------------------------|
| edp_data_{year}_{month} | edp_site_id, unix_time, datetime, edp_device_and_circuit, circuit_label, edp_circuit_label, real_energy, real_energy_negative, real_energy_positive, current_avg, current_min, current_max, voltage_avg, voltage_min, voltage_max  |

The data is provided by Solar Analytics and Wattwatchers and de-identified by UNSW. Solar Analytics site IDs start with “S”, while Wattwatchers site IDs start with “W”.

Due to the large volume of time-series data, energy tables are partitioned by month (except for *edp_data_2018*). From 2019 onwards, data is stored in monthly partitions such as *edp_data_2019_01* for January 2019, with each partition ranging between 1–2 GB (2018 has ~4 GB). Partitions are accessible via the data downloader as regular tables.

Each site has different temporal coverage. For the latest updates on first and last available dates, refer to the table*edp_data_first_and_last_dates*

|  Database Table | Columns | 
|-------------------|--------------------------------------------------|
| edp_data_first_and_last_dates  |edp_site_id, date_of_first_data, date_of_last_data |
| edp_sites_metadata | first_date_metadata_received, last_date_metadata_received, edp_site_id, state, site_time_zone, monitoring_hardware, inverter_manufacturer, inverter_model, subarray_manufacturer, subarray_model, inverter_ac_rating_kw, subarray_orientation, subarray_tilt, subarray_strings, subarray_modules_per_string, subarray_dc_rating_kw, islandable, has_battery, battery_size_make_model, limit_enabled, limit_amount, limit_applied |
| edp_survey_answers | edp_site_id, survey_date, consent_edp, consent_spt, consent_contact, consent_contact_method, over_18, postcode, state, account_holder, property_use, property_year, property_construction, property_construction_other, property_star_rating, num_floors, dwelling_type, dwelling_type_other, num_bedrooms, tenure, tenure_not_occupied, tenure_other, tenure_rented_from, tenure_rented_other, num_occupants, num_children, num_occupants_70plus, has_pets, num_type_pets, has_livestock, num_type_livestock, num_days_occupied, income_weekly, has_gas, has_gas_heating, has_gas_cooking, has_gas_hot_water, has_aircon, aircon_type, num_rooms_aircon, num_rooms_heated, num_refrigerators, has_pool_pump, dryer_usage, has_ev, ev_type, ev_charger_type, significant_loads_other, commercial_loads, business_hours, connection_type, has_controlled_load, islandable, property_power_outage_types, property_power_outage_other, area_power_outage_types, area_power_outage_other, power_outage_latest, hot_water_heat_type, hot_water_heat_type_other, rooms_heat_type, rooms_heat_type_other, has_battery, battery_size_make_model |


## Appendix A: Original Data and Preparation 

EDP consists of 5-minute energy data from Solar Analytics and Wattwatchers. Both provide energy data through their APIs and send us the survey responses and some metadata as excel files. 

### 1. Solar Analytics 

The first batch of Solar Analytics customers that participate in the EDP project includes 129 sites from all around the country. The first survey for this group was performed in December 2021. The second batch we received data for completed the survey in April 2022 (a few in June 2022) and included 485 sites. For each batch, Solar Analytics has provided us with two excel files, one the survey responses, and one the system details (sites metadata). A few sites from each batch opted out soon after the project commenced, leaving us with a total of 599 sites. 

#### 1.1. Survey Responses 

The survey questions and the answers for all sites were provided in an excel file. Complete questions were the headers of the columns and answers for each site were provided as a row. We have mapped each survey question to a column name. Below is the list of column mapping used for the survey questions. The actual questions and the answer options for each can be found on the Column Mapping spreadsheet. 

| #  | Field Name |   | #  | Field Name |   | #  | Field Name |
|----|-------------|---|----|-------------|---|----|-------------|
| 1  | CONSENT_EDP |   | 23 | DWELLING_TYPE |   | 45 | AIRCON_TYPE |
| 2  | CONSENT_SOLA |   | 24 | DWELLING_TYPE_OTHER |   | 46 | NUM_ROOMS_AIRCON |
| 3  | CONSENT_SPT |   | 25 | NUM_BEDROOMS |   | 47 | NUM_ROOMS_HEATED |
| 4  | CONSENT_CONTACT |   | 26 | TENURE |   | 48 | NUM_REFRIGERATORS |
| 5  | CONSENT_CONTACT_METHOD |   | 27 | TENURE_NOT_OCCUPIED |   | 49 | HAS_POOLPUMP |
| 6  | NAME |   | 28 | TENURE_OTHER |   | 50 | DRYER_USAGE |
| 7  | EMAIL |   | 29 | TENURE_RENTED_FROM |   | 51 | HAS_EV |
| 8  | PHONE |   | 30 | TENURE_RENTED_OTHER |   | 52 | EV_TYPE |
| 9  | OVER_18 |   | 31 | NUM_OCCUMPANTS |   | 53 | EV_CHARGER_TYPE |
| 10 | SOLA_SITE_ID |   | 32 | NUM_CHILDREN |   | 54 | SIGNIFICANT_LOADS_OTHER |
| 11 | SOLA_SITE_NAME |   | 33 | NUM_OCCUPANTS_70PLUS |   | 55 | COMMERCIAL_LOADS |
| 12 | STREET |   | 34 | HAS_PETS |   | 56 | BUSINESS_HOURS |
| 13 | SUBURB |   | 35 | NUM_TYPE_PETS |   | 57 | CONNECTION_TYPE |
| 14 | POSTCODE |   | 36 | HAS_LIVESTOCK |   | 58 | HAS_CONTROLLED_LOAD |
| 15 | STATE |   | 37 | NUM_TYPE_LIVESTOCK |   | 59 | ISLANDABLE |
| 16 | ACCOUNT_HOLDER |   | 38 | NUM_DAYS_OCCUPIED |   | 60 | PROPERTY_POWER_OUTAGE_TYPES |
| 17 | PROPERTY_USE |   | 39 | INCOME_WEEKLY |   | 61 | PROPERTY_POWER_OUTAGE_OTHER |
| 18 | PROPERTY_YEAR |   | 40 | HAS_GAS |   | 62 | AREA_POWER_OUTAGE_TYPES |
| 19 | PROPERTY_CONSTRUCTION |   | 41 | HAS_GAS_HEATING |   | 63 | AREA_POWER_OUTAGE_OTHER |
| 20 | PROPERTY_CONSTRUCTION_OTHER |   | 42 | HAS_GAS_COOKING |   | 64 | POWER_OUTAGE_LATEST |
| 21 | PROPERTY_STAR_RATING |   | 43 | HAS_GAS_HOT_WATER |   |   |   |
| 22 | NUM_FLOORS |   | 44 | HAS_AIRCON |   |   |   |

Questions 31 to 38 were asked twice; the second set (all having if known) had no answers, hence was not included. 

#### 1.2. Systems Metadata 

Solar Analytics has provided us with a second excel sheet containing the sites’ metadata. The list of the column names is as follows: 

| #  | Field Name |
|----|-------------|
| 1  | Site ID |
| 2  | State |
| 3  | Monitoring Hardware |
| 4  | Inverter Manufacturer |
| 5  | Inverter Model |
| 6  | Inverter AC Rating (kW) |
| 7  | Subarray Manufacturer |
| 8  | Subarray Model |
| 9  | Subarray Orientation |
| 10 | Subarray Tilt |
| 11 | Subarray Strings |
| 12 | Subarray Modules per String |
| 13 | Subarray DC Rating (kW) |
| 14 | Has Battery |
| 15 | Opted in |
| 16 | Survey email |

Each site has had one or more rows in this file. The data was mostly clean, most answers to each question in a unified format, number values in numeric format, etc. 

#### 1.3. Energy Data 

Energy timeseries data with the granularity of 5 minutes has been retrieved from Solar Analytic API. Then for each circuit we extract device name, circuit label, and data which includes the following fields: 

- time 

- site id 

- energy 

- negative energy 

- positive energy 

- reactive energy 

- power 

- power factor 

- reactive power 

- apparent power 

- voltage 

- current 

The returned timestamp values are in Unix format in site’s local time. We convert the time to datetime format and keep both the original Unix time and the corresponding datetime. Also renamed the fields as below: 

| #  | Field Name |
|----|-------------|
| 1  | SolA Field Name |
| 2  | EDP Field Name |
| 3  | energy |
| 4  | realEnergy |
| 5  | energyNeg |
| 6  | realEnergyNegative |
| 7  | energyPos |
| 8  | realEnergyPositive |
| 9  | power |
| 10 | realPower |

#### 1.4. Deidentification 

We de-identify all the sites and energy data. Site data includes site metadata and survey. We de-identify site data by removing name, full address, email, phone, and Solar Analytics customer ID. From the address fields we only keep postcode and state as they are required for some analyses but by themselves are not identifying. To de-identify the energy data we remove site ID and device names. For each site we generate and allocate a unique EDP site ID. This new id is then attached to all the data of the site, i.e., site metadata, survey, and energy timeseries data. Since we remove device name to deidentify the energy data, for each device we allocate a letter which we concatenate at the end of circuit label and add channel number after that. For example, if in the original data we have (typical for Solar Analytics energy data): 

| Site ID | Device Name | Circuit Label |
|---------|-------------|-------------|
| 98765 | D111122233_1 | ac_net_load |

After the process of deidentification we will have: 


| edp_site_id | edp_device_and_circuit | edp_circuit_label |
|--------|---------------|------------|
| S1234 | A1 | ac_net_load_A1 |

In this example, device name D111122233 is replaced with letter A and together with the channel number becomes A1, is named *edp_device_and_circuit*, and also added to the circuit label to differentiate it from the data of other devices/circuits (becoming *edp_circuit_label*). 

### 2. Wattwatchers 

We have been receiving data and metadata from 352 Wattwatchers sites, each site having 6 devices. This section will be completed with data extraction and de-identification processes, however, the major differences between data from Solar Analytics and Wattwatchers are explained in the next section. 

## 3. Unifying Data from Solar Analytics and Wattwatchers Sites 

The aim has been to have the data from Solar Analytics and Wattwatchers in the same database tables: one table for all the energy data, one table for metadata, and one table for survey responses. 

### 3.1. Unifying Data Fields 

#### Energy data 

There were no power columns in Wattwatchers data. Additionally, power fields (real power, apparent power, reactive power, power factor) are calculated from other fields and do not add any new information. Therefore, it was decided not to have those fields in the database, that is not to upload power fields of the Solar Analytics energy timeseries to the database. However, the deidentified csv files still include those fields. Additionally, Wattwatchers data had minimum and maximum voltage and current, while Solar Analytics data had average voltage and average current. It was decided to have three fields for each measure (min, max, average), fill the ones that have available values and keep the rest empty. 

#### Metadata and survey 

To be completed. 

### 3.2. Unifying Energy Units 

The energy data units were different for the two companies. Solar Analytics energy data unit was Wh while Wattwatchers energy data was in Joules. Therefore, we convert energy values of Wattwatchers from Joules to Wh. 

### 3.3. Unifying Time 

The Solar Analytics data is in local time but as UTC. We have converted it to local date and time and stored both values in the database (as *timestamp* and *datetime*). According to Solar Analytics because it is in local time, daylight saving is applied. The data for the one hour that the clocks are wound back is lost at Solar Analytics’ end and currently there is not a way to get that data. Because it happens overnight though, we would expect no production and (they expect) very little constant consumption. For analysis, the data could probably be interpolated to re-create that hour if needed (or just be ignored if appropriate). Wattwatchers data comes as Unix timestamps in local time. We store timezone of each site in the metadata and survey tables. 

#### SolA

| Timestamp  | Date & Time       |
|------------|-----------------|
| 1648950900 | 3/04/2022 1:55  |
| 1648951200 | 3/04/2022 2:00  |
| 1648951500 | 3/04/2022 2:05  |
| ...        | ...             |
| 1648954200 | 3/04/2022 2:50  |
| 1648954500 | 3/04/2022 2:55  |
| 1648954800 | 3/04/2022 3:00  |
| 1648955100 | 3/04/2022 3:05  |

#### WW
| Timestamp  | Date & Time       |
|------------|-----------------|
| 1648911300 | 3/04/2022 1:55  |
| 1648911600 | 3/04/2022 2:00  |
| 1648911900 | 3/04/2022 2:05  |
| ...        | ...             |
| 1648914600 | 3/04/2022 2:50  |
| 1648914900 | 3/04/2022 2:55  |
| 1648915200 | 3/04/2022 2:00  |
| 1648915500 | 3/04/2022 2:05  |
| 1648915800 | 3/04/2022 2:10  |
| ...        | ...             |
| 1648918500 | 3/04/2022 2:55  |
| 1648918800 | 3/04/2022 3:00  |
| 1648919100 | 3/04/2022 3:05  |


#### SolA
| Timestamp  | Date & Time       |
|------------|-----------------|
| 1664675700 | 2/10/2022 1:55  |
| 1664676000 | 2/10/2022 2:00  |
| 1664676300 | 2/10/2022 2:05  |
| ...        | ...             |
| 1664679000 | 2/10/2022 2:50  |
| 1664679300 | 2/10/2022 2:55  |
| 1664679600 | 2/10/2022 3:00  |
| 1664679900 | 2/10/2022 3:05  |

#### WW

| Timestamp  | Date & Time       |
|------------|-----------------|
| 1664639700 | 2/10/2022 1:55  |
| 1664640000 | 2/10/2022 3:00  |
| 1664640300 | 2/10/2022 3:05  |
