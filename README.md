# Weather

The common weather vocabulary -- current conditions, forecasts, historical observations, severe-weather alerts, air quality and the stations that measure it -- distilled from the APIs that already exist. Part of [Schema Commons](https://github.com/api-commons/schema-commons),
published by the [API Commons](https://apicommons.org).

Built from the providers tagged Weather, Meteorology, Weather Data, Weather API, Weather Forecasting in [APIs.io](https://apis.io) that
publish their own OpenAPI. Only harvested contracts count -- nothing API Evangelist
generated. Every number below is a count of providers.

## What the market already agrees on

<!-- commons:summary -->
| Concept | Expose it | Readable schema | Core properties (share) | Collection named |
| --- | ---: | ---: | --- | --- |
| current_conditions | 12 | 11 |  | current 10, now 1, current-conditions 1, latest 1 |
| forecast | 14 | 12 |  | forecast 11, hourly 5, daily 4, forecasts 2 |
| historical_weather | 9 | 7 | time 57% | history 7, historical 2, observations 1, timemachine 1 |
| weather_alert | 9 | 7 |  | alerts 8, alert 1 |
| air_quality | 11 | 10 |  | air-quality 4, airquality 4, pollen 1, air_quality 1 |
| station | 9 | 8 | name 75%, id 50% | stations 8, station 2 |

Cohort: 54 providers with harvested OpenAPI, 27 crosswalks. Of the providers exposing a current_conditions: 8% create, 92% list, 8% fetch, 0% update, 0% delete. Generated 2026-11-10 by build.py.
<!-- /commons:summary -->

Shares are of the providers with a readable resource schema for that concept. The
extended tier and the long tail of every concept are in [profile.yml](profile.yml).

## What the measurement shows

- Weather agrees on less than any other data context. Current conditions come closest -- a condition, a temperature and a time, each at 36-45% -- and forecasts agree on nothing: every provider nests readings in time-series arrays (daily, hourly, list, timelines) wrapped in its own location metadata.
- Counted per listing, alerts looked standardised: four National Weather Service listings return the Common Alerting Protocol. Counted once, alerts agree only on id, status and geometry at 43%.
- The physical quantities are shared even where the shapes are not: temperature, humidity, wind and pressure collapse to one name each through the aliases, so the vocabulary is usable as a dictionary even though no resource reaches core.

## Artifacts

- [openapi.yml](openapi.yml) -- OpenAPI 3.1 of the shared operations. No one serves it; it
  is the shape to map to. Lints clean against the
  [naming ruleset](https://github.com/api-commons/schema-commons/blob/main/ruleset/schema-commons-naming.yaml).
- [schema/](schema) -- JSON Schema 2020-12 per concept; each property carries
  `x-commons-tier`, `x-commons-share`, `x-commons-providers` and the spellings seen.
- [crosswalk/](crosswalk) -- one per provider, common name -> their name, with the harvested
  spec and operations each mapping was read from.
- [adopters.yml](adopters.yml) -- who exposes which concept, linked to their APIs.io page.
- [concepts.yml](concepts.yml) -- the only hand-written input: concepts, path segments and
  their qualifiers, aliases, reviewed exclusions, merged duplicates, and the decisions below.
- [census/](census) -- the cohort and the raw census the profile is built from.

## Decisions

These are calls, not measurements, recorded so they can be revisited as the method matures.

- Forecasting is not a cohort tag: it covers demand and financial forecasting.
- Current conditions are `/current`, history `/history` and alerts `/weather-alerts` (bare `/alerts` is observability's).
- NOAA, noaa.gov, weather.gov and the National Weather Service are one publisher and count once; OpenWeather's two listings likewise.
- Pollen counts as air quality.

## Rebuild

```bash
../schema-commons/scripts/run.sh .
```

## License

Artifacts CC BY-NC-SA 4.0 ([LICENSE](LICENSE)); code Apache 2.0 ([LICENSE-CODE](LICENSE-CODE)).
