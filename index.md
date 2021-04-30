# `s2s-ai-challenge`

## tl;dr

- Goal: Improve global temperature and total precipitation subseasonal forecasts with ML/AI
- [Flyer](https://todo)
- Competition runs on [https://renkulab.io/](https://renkulab.io/)
- How to join: [https://renkulab.io/projects/aaron.spring/s2s-ai-challenge-template/](https://renkulab.io/projects/aaron.spring/s2s-ai-challenge-template/)
- Time: 1st June 2021 - 31st October 2021
- Hosted by: [WMO](https://public.wmo.int/en), [WWRP](https://community.wmo.int/activity-areas/wwrp), [WCRP](https://www.wcrp-climate.org/), [S2S](http://s2sprediction.net/), [SDSC](https://datascience.ch/renku/)
- Website with leaderboard: [https://s2s-ai-challenge.github.io](https://s2s-ai-challenge.github.io)


## Table of Contents
1. [Description](#description)
2. [Timeline](#timeline)
3. [Prize](#prize)
4. [Evaluation](#evaluation)
5. [Data](#data)
6. [Training](#training)
7. [Discussions](#discussion)
8. [Leaderboard](#leaderboard)
9. [Rules](#rules)
10. [Organizers](#organizers)


## Description

The World Meteorological Organisation (WMO) Science and Innovation Department, in collaboration with the Services and Infrastructure Departments, plans to hold an open competition to explore new services based on AI methods and applied to the WWRP/WCRP subseasonal to seasonal (S2S) project database. Moreover, as the  newly formed Research Board of WMO identified Artificial Intelligence (AI) as a key research topic in weather and climate science for the upcoming years, this competition will foster this approach by specifically encouraging the use of AI tools to extract valuable information from the S2S database. The innovation coming out of this competition supports the goals and action areas of the S2S and WWRP implementation plan as well as the WCRP Strategic Plan. The winning entries will push the frontiers of weather and climate science within the WMO framework and as such support the United Nations Sustainable Development Goals (UN SDGs).

This is the landing page of the competition presenting static information about the competition
and a continously updating leaderboard. For code examples and how to contribute, please visit the contribution template repository [renkulab.io](https://renkulab.io/projects/aaron.spring/s2s-ai-challenge-template/).

## Timeline

- 29th April 2021: Announcement of the competition
- 1st June 2021: Start of the competition (First contributions are accepted) <!-- registration: https://docs.google.com/forms/d/e/1FAIpQLSe49IleTxqEBFKzLdtkHvFQH0rPR16o6gfOFQ_L6cPzglAc2Q/viewform form available -->
- 31st October 2021: End of the competition (Last date for contributions)
- November 2021: every participant makes repository public
- early December 2021: Announcement of the winners


## Prize

Prizes are issued for the top three contributions beating the re-calibrated ECMWF benchmark:

1. 15000 CHF
2. 10000 CHF
3.  5000 CHF

The 3rd prize is reserved for the top contribution from developing or least developed country or small island states as per the [UN list (see table C, F, H p.166ff)](https://www.un.org/development/desa/dpad/wp-content/uploads/sites/45/WESP2020_Annex.pdf). If such a contribution is already among the top 2, any third contribution will get the 3rd prize.

The organizers thank [WWRP](https://community.wmo.int/activity-areas/wwrp) and the S2S Trust Fund for providing the money of the prize.

## Evaluation

The objective of the competition is to improve week 3+4 and 5+6 subseasonal global probabilistic 2m temperature and total precipitation forecasts issued in the year 2020 by using Machine Learning/Artificial Intelligence.

The evaluation will be continuously performed by a `scorer` bot on renkulab.io, following [verification notebook](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/verification_RPSS.ipynb).
Submissions are evaluated on the Ranked Probability Score (`RPS`) between the ML-based forecasts and ground truth CPC [temperature](http://iridl.ldeo.columbia.edu/SOURCES/.NOAA/.NCEP/.CPC/.temperature/.daily/) and accumulated [precipitation](http://iridl.ldeo.columbia.edu/SOURCES/.NOAA/.NCEP/.CPC/.UNIFIED_PRCP/.GAUGE_BASED/.GLOBAL/.v1p0/.extREALTIME/.rain) observations based on pre-computed observations-based terciles. This `RPS` is compared to the re-calibrated real-time 2020 ECMWF forecasts into the Ranked Probability Skill Score (`RPSS`).

`RPS` is calculated with the open-source package [xskillscore](https://xskillscore.readthedocs.io/en/latest) over all 2020 `forecast_reference_time`s:
[xs.rps(observations, forecasts, terciles, dim='forecast_reference_time')](https://xskillscore.readthedocs.io/en/latest/api/xskillscore.rps.html)

```python
def RPSS(rps_ML, rps_benchmark):
  return 1 - rps_ML / rps_benchmark  # positive means ML better than ECMWF benchmark
```

The final `RPSS` relevant for the prizes is calculated globally with spatial weighting and averaged over the two variables and two steps. For diagnostics, we host leaderboards for the two variables in three regions:

- Northern extratropics (90N-30N)
- Tropics (29N-29S)
- Southern extratropics (30S-60S)

Please find more details in the [verification notebook](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/verification_RPSS.ipynb).

## Submissions

We expect submissions to cover all bi-weekly week 3-4 and week 5-6 forecasts issued in 2020, see [timings](#timings). We expect one submission netcdf file for all 53 weekly forecasts issued in 2020. Submission have to be gridded on a global 1.5 degree grid.

Each submission is a netcdf file with the folloing dimension sizes and coordinates:

```
>>> # in xarray
>>> ML_forecasts.sizes # todo: add category dim
Frozen(SortedKeysDict({'forecast_reference_time': 53, 'latitude': 121, 'longitude': 240, 'step': 2}))

>>> ML_forecasts.coords  # time is optional # todo: add category coord
Coordinates:
  * latitude                 (latitude) float64 90.0 88.5 87.0 ... -88.5 -90.0
  * longitude                (longitude) float64 0.0 1.5 3.0 ... 357.0 358.5
  * forecast_reference_time  (forecast_reference_time) datetime64[ns] 2020-01...
  * step                     (step) timedelta64[ns] 21 days 35 days
    time                     (step, forecast_reference_time) datetime64[ns] 2...
```
A template file for submissions can be found [here](http://to.do).

Such submissions need to be commited in git/renku with [`git lfs`](https://git-lfs.github.com/).

After the competition, the code for training must be made public, so the competition maintainers will check the requirements of data timing use. The prizes will be distributed for the top 3 requirements-complying contributions at the end of the competition.
During the competition the organizers may ask top listed participants to provide access to their training pipeline.
Please indicate the resources used (number of CPUs/GPUs, memory, platform; see examples) in your scripts/notebooks to allow reproducibility. Contributions which cannot independently reproduced cannot win prizes.

## Data

### Timings

1) Which forecast starts/target periods (weeks 3-4 & 5-6) to require to be submitted?
- 53 forecasts issued on Thursdays in 2020 (since there are available from all S2S models, including our ECMWF benchmark)
- In that case, the first forecast is issued S=2 Jan 2020, for the week 3-4 target 16-29 Jan.

Please find a list of the dates when forecasts are issued `forecast_reference_time` and corresponding start and end in `valid_time` for week 3-4 and week 5-6.

{% include_relative timings_2020.html %}
 
2) Which data to “allow” to be used to make a specific ML forecast?
- for the first forecast issued S=2 Jan 2020, any observational data up to the day of the the forecast start, ie 2 Jan 2020
- any S2S forecasts up to and including S=2 Jan 2020

### Data Sources

Main datasets for this competition are already available as [renku datasets](https://renku.readthedocs.io/en/latest/user/data.html) for both variables temperature, precipitation:
- `tag in climetlab`: description (link in renku)
- `forecast-benchmark`: ECMWF week 3+4 & 5+6 re-calibrated real-time 2020 forecasts (missing)
- `observations`: CPC daily observations interpolated on 1.5 degree grid (missing)
- `training-input`: daily real-time initialized on thursdays 2020 forecasts from models ECMWF, ECCC, NCEP (missing)
- `forecast-input`: daily reforecasts initialized once per week until 2019 from models ECMWF, ECCC, NCEP (missing)
- `terciles` Observations-based tercile category_edges (missing)

<!--Not all available yet, also not yet cleaned.-->

We encourage to use subseasonal forecasts from the S2S and SubX projects.

- S2S
  - [European Weather Cloud via climetlab](https://github.com/ecmwf-lab/climetlab-s2s-ai-challenge)
  - [IRIDL](https://iridl.ldeo.columbia.edu/SOURCES/.ECMWF/.S2S)
  - s2sprediction.net
- SubX
  - [IRIDL](http://iridl.ldeo.columbia.edu/SOURCES/.Models/.SubX)

However, any other publicly available data sources (like CMIP, NMME, etc.) of dates prior the forecast_reference_time can be used for `training-input` and `forecast-input`.
Also purely empirical methods like persistence or climatology could be used. The only strong data requirement concerns time, see [timings](#timings)


### Examples

coming soon

- [Train ML model](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge/-/blob/master/notebooks).
- [Score RPSS ML model vs ECMWF](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge/-/blob/master/notebooks).

## Join

Follow the steps [in the template renku project](https://renkulab.io/projects/aaron.spring/s2s-ai-challenge-template).

## Training

Where to train?

- renkulab.io provides free but limited compute resources. You may use upto 2 CPUs, 8 GB memory and 10 GB disk space.
- as renku projects are git repositories under the hood, you can `renku clone` or `git clone` your project onto your own laptop or supercomputer account for the heavy lifting
- ECMWF will provide limited compute nodes on the European Weather Cloud `EWC` (where large parts of the data is stored) upon request. This opportunity is specifically targeted for participants from developing countries and/or without institutional computing resources. Please get in touch with [Aaron](mailto:aaron.spring@mpimet.mpg.de) for access. Please note that we cannot make promises about these resources given the unknown demand.

How to train?

We are looking for smart solutions here. Find a quick start [here](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/ML_forecast.ipynb).

## Discussion

Please use the issue tracker in the renkulab [`s2s-ai-challenge` gitlab repository](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge/-/issues) for discussions.

Answered questions from the issue tracker will be transferred to the [FAQ](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge/-/wikis/Frequently-Asked-Questions-(FAQ)).

## Leaderboard

### RPSS averaged over all variables, regions and lead times
[Leaderboard from repository](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge/-/blob/master/leaderboard.html)

{% include_relative leaderboard.html %}


### RPSS temperature

#### RPSS temperature Northern Extratropics [90N-30N)

#### RPSS temperature Tropics [30N-30S]

#### RPSS temperature Southern Extratropics (30S-60S]


### RPSS temperature

#### RPSS precipitation Northern Extratropics [90N-30N)

#### RPSS precipitation Tropics [30N-30S]

#### RPSS precipitation Southern Extratropics (30S-60S]


## Rules

- These rules may be modified by the organizers until the start of the competition. <!-- Under which circumstances are organizers allowed to change rules later on? -->
- One team can only get one prize.
- One Person can only join one team.
- Model training is not allowed use ground truth/observations data after forecast was issued, see Data Timings.
- All taxes imposed on prizes are the sole responsibility of the winners. 
- By joining the competition (see steps https://renkulab.io/projects/aaron.spring/s2s-ai-challenge-template), participants agree that they will make their private repositories on renkulab.io public after the competition ends (31st October 2021) regardless whether their contributions are among the top 3 for prizes. All repositories must be made availbale under the tbd licence. 
<!--- warranty? 
- law? 
- some WMO liability statement? -->

## Organizers

- Estelle De Coning, Michel Rixen, Wenchao Cao (WMO)
- Frederic Vitart, Florian Pinault, Baudouin Raoult (ECMWF)
- Andy Robertson (IRI)
- Rok Roskar, Tasko Olevski (SDSC)
- [Aaron Spring](mailto:aaron.spring@mpimet.mpg.de) [@aaronspring](https://github.com/aaronspring/) (main contact, WMO contractor)

<!---
Todo:
- take top 5 of leaderboard only, should we have sub-leaderboards for different variables, regions?
- implement discussion forum? github discussions?
- add link to flyer
- define which countries are developing countries? link above is just a suggestion
- `category` dimension
-->
