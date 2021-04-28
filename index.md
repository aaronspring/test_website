## tl;dr

- Goal: Improve global temperature and precipitation subseasonal predictions with ML/AI
- [Flyer](https://todo)
- Competition runs on https://renkulab.io/
- How to join: https://renkulab.io/projects/aaron.spring/s2s-ai-competition-bootstrap/
- Time: 1st June 2021 - 31st October 2021
- Hosted by: [WMO](https://public.wmo.int/en), [WWRP](https://community.wmo.int/activity-areas/wwrp), [WCRP](https://www.wcrp-climate.org/), [S2S](http://s2sprediction.net/), [SDSC](https://datascience.ch/renku/)
- Website with leaderboard: https://s2s-ai-challenge.github.io

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

This is the landing page of the competition presenting static information about the competition
and a continously updating leaderboard. For code examples and how to contribute, please visit [renkulab.io](https://renkulab.io/projects/aaron.spring/s2s-ai-competition-bootstrap/).

## Timeline

- 29th April 2021: Announcement of the competition
- 1st June 2021: Start of the competition (First contributions are accepted)
- 31st October 2021: End of the competition (Last date for contributions)
- November 2021: every participant makes code public
- early December 2021: Announcement of the winners


## Prize

1. 15K CHF
2. 10K CHF
3. 5K CHF

The 3rd prize is reserved for the top contribution from developing countries (see [Table C p.166](https://www.un.org/development/desa/dpad/wp-content/uploads/sites/45/WESP2020_Annex.pdf) for country list). If such a contribution is already among the top 2, any thrid contribution will get the 3rd prize.

The organizers thank WWRP for providing the money of the prize. missing some org here?

## Evaluation

The objective of the competition is to improve week 3+4 and 5+6 subseasonal global probabilistic temperature and precipitation predictions issued in the year 2020 by using Machine Learning/Artificial Intelligence.

The evaluation will be continuously performed by a `scorer` bot on renkulab.io, following [verification notebook](https://renkulab.io/gitlab/aaron.spring/s2s-ai-competition-bootstrap/-/blob/master/notebooks/verification_RPSS.ipynb).
Submissions are evaluated on the Ranked Probability Score (`RPS`) between the ML-based forecasts and ground truth CPC observations based on pre-computed observations-based terciles. This `RPS` is compared to the re-calibrated real-time 2020 ECMWF forecasts into the Ranked Probability Skill Score (`RPSS`).

`RPS` is calculated with the open-source package [xskillscore](https://xskillscore.readthedocs.io/en/latest) over all 2020 `forecast_reference_time`s:
[xs.rps(observations, forecasts, terciles, dim='forecast_reference_time')](https://xskillscore.readthedocs.io/en/latest/api/xskillscore.rps.html)

```python
def RPSS(rps_ML, rps_benchmark):
  return 1 - rps_ML / rps_benchmark  # positive means ML better than ECMWF benchmark
```

The `RPSS` is calculated globally, and aggregated in three regions: Northern extratropics (90N-30N), tropics (29N-29S) and Southern extratropics (30S-90S). The final score is averaged over all 2 variables, 2 steps and 3 regions.
Please find more details in the [verification notebook](https://renkulab.io/gitlab/aaron.spring/s2s-ai-competition-bootstrap/-/blob/master/notebooks/verification_RPSS.ipynb).

Each submission is a netcdf file with the folloing dimension sizes:

```
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

Such submissions need to be commited in git/renku with [`git lfs`](https://git-lfs.github.com/).

After the competition, the code for training must be made public, so the competition maintainers will check the requirements of data timing use. During the competition the organizers may ask top listed participants to provide access to their training pipeline.
Please indicate the resources used (number of CPUs/GPUs, memory, platform; see examples) in your scripts/notebooks to allow reproducibility.

## Data

### Timings

1) Which forecast starts/target periods (weeks 3-4 & 5-6) to require to be submitted?
- It may make sense to take all Thursday starts in 2020 since there are available from all S2S models, including our ECMWF benchmark
- In that case, the first forecast will start S=2 Jan 2020, for the week 3-4 target 16-29 Jan.
- The list of 52 forecasts and targets could be tabulated for clarity
 
2) Which data to “allow” to be used to make a specific ML forecast?
- for the first forecast S=2 Jan 2020, any observational data up to the day before (or of?) the forecast start, ie 1 Jan 2020 
- any S2S forecasts up to and including S=2 Jan 2020
- daily data

### Data Sources

- S2S
  - [European Weather Cloud via climetlab](https://github.com/ecmwf-lab/climetlab-s2s-ai-challenge)
  - [IRIDL](https://iridl.ldeo.columbia.edu/SOURCES/.ECMWF/.S2S)
  - s2sprediction.net
- SubX
  - [IRIDL](http://iridl.ldeo.columbia.edu/SOURCES/.Models/.SubX)
- other sources allowed? CMIP daily?

Main datasets for this competition are already available as [renku datasets](https://renku.readthedocs.io/en/latest/user/data.html) for both variables temperature, precipitation:
- `tag in climetlab`: description (link in renku)
- `forecast-benchmark`: ECMWF week 3+4 & 5+6 re-calibrated real-time 2020 forecasts
- `observations`: CPC daily observations interpolated on 1.5 degree grid:
- `training-input`: daily real-time initialized on thursdays 2020 forecasts from models ECMWF, ECCC, NCEP
- `forecast-input`: daily reforecasts initialized once per week until 2019 from models ECMWF, ECCC, NCEP
- Observations-based tercile category_edges

Not all available yet, also not yet cleaned.

### Examples

[verification notebook CHANGE LINK](https://renkulab.io/gitlab/aaron.spring/s2s-ai-competition-bootstrap/-/blob/master/notebooks/verification_RPSS.ipynb).

## Training

Where to train?

- renkulab.io provides free but limited compute resources. You may use upto 2 CPUs, 8 GB memory and 10 GB disk space.
- as renku projects are git repositories under the hood, you can `renku clone` or `git clone` your project onto your own laptop or supercomputer account for the heavy lifting
- ECMWF will provide limited compute nodes on the European Weather Cloud `EWC` (where large parts of the data is stored) upon request. This opportunity is specifically targeted for participants from developing countries and/or without institutional computing resources. Please get in touch with [Aaron](mailto:aaron.spring@mpimet.mpg.de) for access.

How to train?

We are looking for smart solutions here. Find a quick start [here](https://renkulab.io/gitlab/aaron.spring/s2s-ai-competition-bootstrap/-/blob/master/notebooks/ML_forecast.ipynb).

## Discussion

Please use the issue tracker in the renkulab gitlab repository
[to be added](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge/-/issues) for discussions.

Answered questions from the issue tracker will be transferred to the [FAQ](https://renkulab.io/gitlab/aaron.spring/s2s-ai-competition-bootstrap/-/wikis/Frequently-Asked-Questions-(FAQ)).

## Leaderboard
[Leaderboard on renku in html](https://renkulab.io/gitlab/aaron.spring/s2s-ai-competition-bootstrap/-/blob/master/leaderboard.html)

{% include_relative leaderboard.html %}


## Rules

- These rules may be modified by the organizers until the start of the competition. Under which circumstances are organizers allowed to change these?
- One team can only get one prize.
- One Person can only join on team.
- Model training is not allowed use ground truth/observations data after forecast was issued, see Data Timings.
- All taxes imposed on prizes are the sole responsibility of the winners.
- By joining the competition (see steps https://renkulab.io/projects/aaron.spring/s2s-ai-competition-bootstrap), participants agree that they will make their private repositories on renkulab.io public after the competition ends (31st October 2021) regardless whether their contributions are among the top 3 for prizes. All repositories must be made availbale under the xyz-tbd licence.
- warranty?
- law?
- some WMO liability statement?

## Organizers

- Estelle De Coning, Michel Rixen, Wenchao Cao (WMO)
- Frederic Vitart, Florian Pinault, Baudouin Raoult (ECMWF)
- Andy Robertson (IRI)
- Rok Roskar, Tasko Olevski (SDSC)
- Aaron Spring (main contract, WMO contractor)

### End
```markdown
Todo:

- take top 5 of leaderboard only, should we have sub-leaderboards for different variables?
- implement discussion forum? github discussions?
- add link to flyer
- define which countries are developing countries? link above is just a suggestion
- `category` dimension
```
