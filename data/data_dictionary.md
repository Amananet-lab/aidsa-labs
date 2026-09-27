# Data dictionary: notional airlift and tanker group

All data is synthetic and unclassified. Bases, tail numbers and every figure are invented;
aircraft designations are public, their performance figures here are notional.

## departures.csv

One row per departure over one calendar year. Encoding UTF-8, comma-separated, header row.

| Field | Type | Units | Valid domain | Meaning |
|---|---|---|---|---|
| mission_id | text | | `M` followed by five digits; unique | Departure identifier |
| tail_id | text | | `T-` followed by four digits | Aircraft identifier; key to aircraft_history.json |
| aircraft_type | text | | exactly one of `C-17A`, `C-5M`, `KC-135R`, `KC-46A` | Aircraft type |
| home_base | text | | `Halstead AFB`, `Carver Field`, `Dunmore AFB` | Base the departure flew from |
| mission_type | text | | `channel`, `SAAM`, `air refueling`, `aeromedical`, `training` | Mission category |
| departure_date | date | | ISO `YYYY-MM-DD`, within the year | Scheduled departure date |
| aircraft_age_years | number | years | 0 to 70 | Airframe age at the start of the year |
| total_flight_hours | number | hours | positive | Airframe flight hours at the start of the year |
| days_since_inspection | integer | days | 1 to 200 | Days since the aircraft's last scheduled inspection |
| ground_time_hours | number | hours | 1 to 150 | Time on the ground before this departure |
| cargo_weight_klb | number | thousands of pounds | 0 to the type's cargo limit (below); 0 is valid, for example on air refueling | Cargo carried; empty when the load plan was not recorded |
| temperature_c | number | degrees Celsius | -35 to 50 | Temperature at departure |
| sortie_duration_hours | number | hours | 1 to 16 | Planned sortie length |
| maint_delay | integer | | 0 or 1 | 1 if the departure was delayed for a maintenance reason |

Cargo limits by type (notional): C-17A 170, C-5M 280, KC-135R 80, KC-46A 65 thousand pounds.

## aircraft_history.json

A list with one record per tail, as recorded on 31 December of the year before the departures.

| Field | Type | Meaning |
|---|---|---|
| tail_id | text | Aircraft identifier |
| aircraft_type | text | Aircraft type |
| home_base | text | Home base |
| inspections | list | Inspections during the prior year, oldest first |
| inspections[].inspection_id | text | Inspection identifier |
| inspections[].date | date | Inspection date, ISO format |
| inspections[].kind | text | `home station check`, `isochronal` or `phase` |
| inspections[].findings | list | Discrepancies written up at the inspection; may be empty |
| findings[].finding_id | text | Finding identifier |
| findings[].system | text | Aircraft system affected |
| findings[].severity | text | `major` or `minor` |
| findings[].status | text | `open` (not yet fixed on 31 December) or `closed` |
