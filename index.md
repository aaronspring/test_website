<!-- `s2s-ai-challenge` -->
<!-- nice banner instead of just url: subseasonal-to-seasonal prediction AI challenge -->

## Competition Overview <!-- tl;dr -->

- Goal: Improve global temperature and total precipitation subseasonal-to-seasonal predictions with Machine Learning/Artificial Intelligence
- [Flyer](https://www.wcrp-climate.org/documents/news-highlights/S2S_AI_Challenge_Flyer_final.pdf)
- Competition runs on platform [https://renkulab.io/](https://renkulab.io/)
- How to join: [https://renkulab.io/projects/aaron.spring/s2s-ai-challenge-template/](https://renkulab.io/projects/aaron.spring/s2s-ai-challenge-template/)
- Timeline: 1st June 2021 - 31st October 2021
- Data: via (climetlab](https://github.com/ecmwf-lab/climetlab-s2s-ai-challenge), see [Data](#data)
- Organized by: [WMO](https://public.wmo.int/en)/[WWRP](https://community.wmo.int/activity-areas/wwrp), [WCRP](https://www.wcrp-climate.org/), [S2S Project](http://s2sprediction.net/) in collaboration with [SDSC](https://datascience.ch/renku/) and [ECMWF](https://www.ecmwf.int/)  <!-- add logos maybe -->
- Website with leaderboard: [https://s2s-ai-challenge.github.io](https://s2s-ai-challenge.github.io)

<!-- once competition starts move leaderboards to the top and add announcements below, like a blog -->

## Table of Contents
0. [Announcements](#announcements)
1. [Description](#description)
2. [Timeline](#timeline)
3. [Prize](#prize)
4. [Predictions](#predictions)
5. [Evaluation](#evaluation)
6. [Data](#data)
7. [Training](#training)
8. [Discussions](#discussion)
9. [Leaderboard](#leaderboard)
10. [Rules](#rules)
11. [Organizers](#organizers)


## Announcements

#### 2021-05-10: Rules adapted endiscouraging overfitting

The organizers modified the [rules](#rules):
- The codes used must be fully documented, with details of the safeguards enacted to prevent overfitting, see checked safeguards in [template](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/tree/master/notebooks/ML_prediction.ipynb).
- Code to reproduce submissions must be made available after the competition ends to enable open peer-review.
- The leaderboard will be hidden until November 2021 and made public once all participants made their code public.
- The final leaderboard will be ranked based on the RPSS and a grade from peer-review.
- To have more time for this review we shift the annoucement of prizes into February 2022.
- Methods to create the 2020 forecasts must perform similar on new, unseen data. Therefore do not overfit.
- The organizers reserve the right to disqualify submissions if overfitting is suspected.
- To be considered for computational resources at EWC, you need to register until June 1st 2021 and explain why you cannot train your ML model elsewhere.

The organizers are aware that overfitting is an issue if the ground truth is accessible. A more robust verification would be about predicting future states with weekly submissions over a year, which would take much more time until one year of new observations is available. Therefore, we decided against a real-time competition to shorten the project length and keep momentum high. Over time we will all see which methods genuinely have skill and which overfitted their available data. 


## Description
The World Meteorological Organization (WMO) is launching an open prize challenge to improve current forecasts of precipitation and temperature from today’s best computational fluid dynamical models 3 to 6 weeks  into the future using Artificial Intelligence and/or Machine Learning techniques. The challenge is organised by the World Weather Research Programme ([WWRP](https://community.wmo.int/activity-areas/wwrp))/World Climate Research Programme ([WCRP](https://www.wcrp-climate.org/)) Subseasonal-to-Seasonal Prediction Project ([S2S Project](http://s2sprediction.net/)), in collaboration with Swiss Data Science Center ([SDSC](https://datascience.ch/renku/)) and European Centre for Medium-Range Weather Forecasts ([ECMWF](https://www.ecmwf.int/)).

Improved sub-seasonal to seasonal (S2S) forecast skill would benefit multiple user sectors immensely, including water, energy, health, agriculture and disaster risk reduction. The creation of an extensive database of S2S model forecasts has provided a new opportunity to apply the latest developments in machine learning to improve S2S prediction of temperature and total precipitation forecasts up to 6 weeks ahead, with focus on biweekly averaged conditions around the globe. 

The competition will be implemented on the platform of Renkulab which hosts all the codes and scripts. The training and verification data will be easily accessible from the [European Weather Cloud](https://github.com/ecmwf-lab/climetlab-s2s-ai-challenge) and relevant access scripts will be provided to the participants. All the codes and forecasts of the challenge will be made open access after the end of the competition.

This is the landing page of the competition presenting static information about the competition
and a continously updating leaderboard. For code examples and how to contribute, please visit the contribution template repository [renkulab.io](https://renkulab.io/projects/aaron.spring/s2s-ai-challenge-template/).

## Timeline

- 4th May 2021: Announcement of the competition
- 1st June 2021: Start of the competition (First date for submissions) <!-- registration: https://docs.google.com/forms/d/e/1FAIpQLSe49IleTxqEBFKzLdtkHvFQH0rPR16o6gfOFQ_L6cPzglAc2Q/viewform form available -->
- June/July: 2 Town hall for Q&A meetings for different time zones
- 1st July 2021: Freeze the rules for the competition
- 31st October 2021: End of the competition (Final date for submissions)
- 1st November 2021: participants make their submission codes public
- 10th November 2021: make leaderboard public
- 1st November 2021 - January 2022: open peer review and expert peer review, everyone can comment on submissions/codes
- February 2022: Announcement of the winners


## Prize

Prizes are issued for the top three submissions beating the re-calibrated ECMWF benchmark:

- Winner team: 15000 CHF
- 2nd team: 10000 CHF
- 3rd team: 5000 CHF

The 3rd prize is reserved for the top submission from developing or least developed country or small island states as per the [UN list (see table C, F, H p.166ff)](https://www.un.org/development/desa/dpad/wp-content/uploads/sites/45/WESP2020_Annex.pdf). If such a submissions is already among the top 2, any third submission will get the 3rd prize.


## Predictions

The organizers see two different approaches for Machine Learning-based predictions, which may be combined. Predict the week 3-4 & 5-6 state based on:

- I. the initial conditions (observations at `forecast_time`)
- II. the week 3-4 & 5-6 S2S forecasts

![ML-based predictions schematic](ML_model_schematic.jpeg?raw=true "ML-based predictions")

For the exact `valid_time`s to predict, see [timings](#timings). For the data to use us for training, see [data sources](#sources). Comply to the [rules](#rules).


## Evaluation

The objective of the competition is to improve week 3+4 and 5+6 subseasonal global probabilistic [2m temperature](https://confluence.ecmwf.int/plugins/servlet/mobile?contentId=27394104#content/view/27394104) and [total precipitation](https://confluence.ecmwf.int/plugins/servlet/mobile?contentId=27399606#content/view/27399606) forecasts issued in the year 2020 by using Machine Learning/Artificial Intelligence.

The evaluation will be continuously performed by a `scorer` bot on renkulab.io, following [verification notebook](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/). <!-- verification_RPSS.ipynb -->
Submissions are evaluated on the Ranked Probability Score (`RPS`) between the ML-based forecasts and ground truth CPC [temperature](http://iridl.ldeo.columbia.edu/SOURCES/.NOAA/.NCEP/.CPC/.temperature/.daily/) and accumulated [precipitation](http://iridl.ldeo.columbia.edu/SOURCES/.NOAA/.NCEP/.CPC/.UNIFIED_PRCP/.GAUGE_BASED/.GLOBAL/.v1p0/.extREALTIME/.rain) observations based on pre-computed observations-based terciles. This `RPS` is compared to the re-calibrated real-time 2020 ECMWF forecasts into the Ranked Probability Skill Score (`RPSS`).

`RPS` is calculated with the open-source package [xskillscore](https://xskillscore.readthedocs.io/en/latest) over all 2020 `forecast_time`s.
For deterministic forecasts:
```python
xs.rps(observations, deterministic_forecasts, category_edges=precomputed_tercile_edges, dim='forecast_time')
```

For probabilistic forecasts:
```python
xs.rps(observations, probabilistic_forecasts, category_edges=None, input_distributions='p', dim='forecast_time')
```

See the [`xskillscore.rps` API](https://xskillscore.readthedocs.io/en/latest/api/xskillscore.rps.html) for details.

```python
def RPSS(rps_ML, rps_benchmark):
    """Ranked Probability Skill Score. Compares two RPS.
  
    +---------+-----------------------------------------+
    |  Score  | Description                             |
    +---------+-----------------------------------------+
    |    1    | maximum, perfect improvement            |
    +---------+-----------------------------------------+
    |  (0,1]  | positive means ML better than benchmark |
    +---------+-----------------------------------------+
    |    0    | equal performance                       |
    +---------+-----------------------------------------+
    | (0, -∞) | negative means ML worse than benchmark  |
    +---------+-----------------------------------------+

  """
  return 1 - rps_ML / rps_benchmark  # positive means ML better than ECMWF benchmark
```

The `RPSS` relevant for the prizes is first calculated on each grid cell globally on a 1.5 degree grid.
This gridded RPSS is spatially averaged (weighted `(np.cos(np.deg2rad(ds.latitude)))`) over [90N-60S] land points and futher averaged over both variables and both `lead_time`s.

For diagnostics, we will further host leaderboards for the two variables in three regions:

- Northern extratropics (90N-30N)
- Tropics (29N-29S)
- Southern extratropics (30S-60S)

Please find more details in the [verification notebook](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/) (coming soon...). <!-- verification_RPSS.ipynb -->

<!-- Note: We are currently discussing how to avoid overfitting in this [tweet and the comments below](https://twitter.com/fpappenberger/status/1389501396043669511?s=21). -->


## Submissions

We expect submissions to cover all bi-weekly week 3-4 and week 5-6 forecasts issued in 2020, see [timings](#timings). We expect one submission `netcdf` file for all 53 weekly forecasts issued in 2020. Submission have to be gridded on a global 1.5 degree grid.

Each submission is a netcdf file with the folloing dimension sizes and coordinates:

```
>>> # in xarray
>>> ML_forecasts.sizes
Frozen(SortedKeysDict({'forecast_time': 53, 'latitude': 121, 'longitude': 240, 'lead_time': 2, 'category': 3}))

>>> ML_forecasts.coords
Coordinates:
  * latitude                 (latitude) float64 90.0 88.5 87.0 ... -88.5 -90.0
  * longitude                (longitude) float64 0.0 1.5 3.0 ... 357.0 358.5
  * forecast_time  (forecast_time) datetime64[ns] 2020-01...
  * lead_time                (lead_time) timedelta64[ns] 14 days 28 days
  * category                 (category) <U11 '[0., 0.33)' '[0.33, 0.66)' '[0.66, 1.]'
    valid_time               (lead_time, forecast_time) datetime64[ns] 2...
```
A template file for submissions can soon be found [here](http://to.do). <!-- TODO -->

Such submissions need to be commited in git with [`git lfs`](https://git-lfs.github.com/).

After the competition, the code for training must be made public, so the organizers and peer review can check the [rules](#rules).
Please indicate the resources used (number of CPUs/GPUs, memory, platform; see [examples](#examples)) in your scripts/notebooks to allow reproducibility. Submissions, which cannot be independently reproduced by the organizers after the competition ends, cannot win prizes, see [rules](#rules).


## Data

### Timings

The organizers explicitly choose to run this competition on past 2020 observations. We are aware of the dangers of overfitting (see [rules](#rules)), if the ground truth data is accessible. We decided against a real-time competition to shorten the project length and keep momentum high.

Please find here an explicit list of the forecast dates required.

1) Which forecast starts/target periods (weeks 3-4 & 5-6) to require to be submitted?
- 53 forecasts issued on Thursdays in 2020 (since there are available from all S2S models, including our ECMWF benchmark)
- In that case, the first forecast is issued S=2 Jan 2020, for the week 3-4 target 16-29 Jan.

Please find a list of the dates when forecasts are issued `forecast_reference_time` and corresponding start and end in `valid_time` for week 3-4 and week 5-6.

{% include_relative timings_2020.html %}
 
2) Which data to “allow” to be used to make a specific ML forecast?
- for the first forecast issued S=2 Jan 2020, any observational data up to the day of the the forecast start, ie 2 Jan 2020
- any S2S forecasts up to and including S=2 Jan 2020

### Sources

Main datasets for this competition are already available as [renku datasets](https://renku.readthedocs.io/en/latest/user/data.html) for both variables temperature and precipitation:

| `tag in climetlab` | Description | renku dataset |
| ------ | ------ | ----- |
| `forecast-benchmark` | ECMWF week 3+4 & 5+6 re-calibrated real-time 2020 forecasts | missing |
| `observations` | CPC daily observations interpolated on a 1.5 degree grid | missing |
| `training-input` | daily real-time initialized on thursdays 2020 forecasts from models ECMWF, ECCC, NCEP| missing |
| `forecast-input` | daily reforecasts initialized once per week until 2019 from models ECMWF, ECCC, NCEP| missing |
| `tercile_edges` | Observations-based tercile category_edges | missing |

<!--Not all available yet, also not yet cleaned.-->

We encourage to use subseasonal forecasts from the S2S and SubX projects:

- S2S
  - [European Weather Cloud via climetlab](https://github.com/ecmwf-lab/climetlab-s2s-ai-challenge)
  - [IRIDL](https://iridl.ldeo.columbia.edu/SOURCES/.ECMWF/.S2S)
  - [s2sprediction.net](http://s2sprediction.net)
- SubX
  - [IRIDL](http://iridl.ldeo.columbia.edu/SOURCES/.Models/.SubX)

However, any other publicly available data sources (like CMIP, NMME, etc.) of dates prior the forecast_reference_time can be used for `training-input` and `forecast-input`.
Also purely empirical methods like persistence or climatology could be used. The only strong data requirement concerns time, see [timings](#timings)

Ground truth sources are CPC temperature and accumulated precipitation from [IRIDL](http://iridl.ldeo.columbia.edu/):
- `pr`: [precipitation rate](http://iridl.ldeo.columbia.edu/SOURCES/.NOAA/.NCEP/.CPC/.UNIFIED_PRCP/.GAUGE_BASED/.GLOBAL/.v1p0/.extREALTIME/.rain) to accumulate
- `t2m`: [2m temperature](http://iridl.ldeo.columbia.edu/SOURCES/.NOAA/.NCEP/.CPC/.temperature/.daily/)

### Examples

In progress...

- [Train ML model](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/ML_forecast.ipynb).
- [Score RPSS ML model vs ECMWF](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge/-/blob/master/notebooks).

## Join

Follow the steps [in the template renku project](https://renkulab.io/projects/aaron.spring/s2s-ai-challenge-template).

## Training

Where to train?

- [renkulab.io](https://renkulab.io) provides free but limited compute resources. You may use upto 2 CPUs, 8 GB memory and 10 GB disk space.
- As renku projects are `git` repositories under the hood, you can `renku clone` or `git clone` your project onto your own laptop or supercomputer account for the heavy lifting.
- ECMWF will provide limited compute nodes on the European Weather Cloud `EWC` (where large parts of the data is stored) upon request. This opportunity is specifically targeted for participants from developing or least developed country or small island states and/or without institutional computing resources. Please indicate **why** you need compute access and cannot train your model elsewhere in the registration form. To be considered for such computational resources at EWC, you need to register until June 1st 2021. *Please note that we cannot make promises about these resources given the unknown demand.*

How to train?

We are looking for your smart solutions here. Find a quick start template [here](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/ML_forecast.ipynb).

## Discussion

Please use the issue tracker in the renkulab [`s2s-ai-challenge` gitlab repository](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge/-/issues) for 
[questions](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge/-/issues/new) to the [organizers](#organizers), [discussions](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge/-/issues/new?issuable_template=discussion),
[bug reports](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge/-/issues/new?issuable_template=bug).
We set up a [![Gitter chat room](https://badges.gitter.im/pangeo-data/s2s-ai-challenge.svg)](https://gitter.im/pangeo-data/s2s-ai-challenge?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge) for informal communication.

## FAQ

Answered questions from the issue tracker are regularly transferred to the [FAQ](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge/-/wikis/Frequently-Asked-Questions-(FAQ)).

## Leaderboard

### RPSS

Submissions have to beat the ECMWF re-calibrated benchmark and following the [rules](#rules) to qualify for prizes.

The leaderboard will be made available after the submission period ends, i.e. November 1st 2021. The prizes will be announced after the submissions have been reviewed on the compliance of the [rules](#rules), i.e. December 15th 2021.

We will also publish subleaderboards, that are purely diagnostic and show RPSS for two variables (`t2m`, `tp`), two `lead_time`s (weeks 3-4 & 5&6) and three subregions ([90N-30N], (30N-30S), [30S-60S]).

### Peer-review

From November 2021 to January 2022, there will be peer-reviews for the top ranked submission. Peer review will evaluate:

- whether the code is written in a clean and understandable way
- whether the code is reproducible by an independent person
- the originality of the method
- whether the safeguards against overfitting and reproducibility have been followed
- whether overfitting has been avoided
- tbd

(Still under discussion) Based on these criteria, there will be a peer-review grade from bad (-1) towards brilliant (+1)

### Final

The top submissions on the combined RPSS and Peer-review score will determine the prizes.
(Still under discussion whether the scores are averaged or ranked and the two ranks averaged.)

## Rules

- One team can only get one prize. One Person can only join one team.
- Prizes are only issued if the method beats the re-calibrated ECMWF benchmark.
- To be eligible for the third prize reserved for submissions from developing or least developed country or small island states, all team members must be resident in such countries. 
- Model training is not allowed to use ground truth/observations data after forecast was issued, see [Data Timings](#timings).
- [Data leakage](https://en.wikipedia.org/wiki/Leakage_(machine_learning)?wprov=sfti1) is not allowed, i.e. do not use `lead_time=0 days` as predictor.
- Do not [overfit](https://en.wikipedia.org/wiki/Overfitting?wprov=sfti1), a creditble model continues to perform similar on new unseen data.
- The codes used must be fully documented, with details of the safeguards enacted to prevent overfitting, see checked safeguards in [template](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/ML_forecast.ipynb).
- The codes to all submissions must be made public on November 1st 2021 to be readable for open peer review. Submissions, which are not made public on November 1st 2021, will be removed from the leaderboard. 
- The leaderboard will be made public in early November 2021, once all submissions are public.
- The organizers reserve the right to disqualify submissions if overfitting is suspected.
- Prizes will be issued on February 10th 2022.
- These rules may be changed by the organizers until 1st July 2021. <!-- Under which circumstances are organizers allowed to change rules later on? -->


## Organizers

- [WMO](https://public.wmo.int/en)/[WWRP](https://community.wmo.int/activity-areas/wwrp): Estelle De Coning, Wenchao Cao
- [WCRP](https://www.wcrp-climate.org/): Michel Rixen
- [S2S Project](http://s2sprediction.net/): Frederic Vitart, Andy Robertson
- [ECMWF](https://www.ecmwf.int/): Florian Pinault, Baudouin Raoult
- [SDSC](https://datascience.ch/renku/): Rok Roskar
- WMO contractor/main contact: [Aaron Spring](mailto:aaron.spring@mpimet.mpg.de) [@aaronspring](https://github.com/aaronspring/) [@realaaronspring](https://twitter.com/realaaronspring/)

<!---
Todo:
- make leaderboard dynamic with bokeh: https://p-mckenzie.github.io/2017/12/01/embedding-bokeh-with-github-pages/ https://github.com/bokeh/bokeh/tree/branch-2.4/examples/app/export_csv
-->
