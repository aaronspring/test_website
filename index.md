<!-- `s2s-ai-challenge` -->
<!-- nice banner instead of just url: subseasonal-to-seasonal prediction AI challenge -->

## Competition Overview <!-- tl;dr -->

- Goal: Improve subseasonal-to-seasonal precipitation and temperature forecasts with Machine Learning/Artificial Intelligence
- [Flyer](https://www.wcrp-climate.org/documents/news-highlights/AI%20Prize%20Challenge_17%20May%202021_1740.pdf)
- Competition runs on platform [https://renkulab.io/](https://renkulab.io/)
- How to join: [https://renkulab.io/projects/aaron.spring/s2s-ai-challenge-template/](https://renkulab.io/projects/aaron.spring/s2s-ai-challenge-template/)
- Contributions accepted: 1st June 2021 - 31st October 2021
- Data: via [climetlab](https://github.com/ecmwf-lab/climetlab-s2s-ai-challenge), see [Data](#data)
- Website: [https://s2s-ai-challenge.github.io](https://s2s-ai-challenge.github.io)
- Organized by: [WMO](https://public.wmo.int/en)/[WWRP](https://community.wmo.int/activity-areas/wwrp), [WCRP](https://www.wcrp-climate.org/), [S2S Project](http://s2sprediction.net/) in collaboration with [SDSC](https://datascience.ch/renku/) and [ECMWF](https://www.ecmwf.int/)

<img src="https://community.wmo.int/themes/wmo/logo.png" alt="WMO logo" height="50"/> <img src="https://ho9an2-datap1.s3.eu-west-1.amazonaws.com/wmoext/s3fs-public/wwrp_logo_small_002.jpg" alt="WWRP logo" height="50"/> <img src="https://www.wcrp-climate.org/images/logos/WCRP_structured_data.png" alt="WCRP logo" height="50"/> <img src="https://www.wcrp-climate.org/images/logos_icones/logo_S2S.png" alt="S2S logo" height="50"/> <img src="https://datascience.ch/wp-content/uploads/2020/09/logo-SDSC-transparent-300x82.png" alt="SDSC logo" height="50"/> <img src="https://www.ecmwf.int/sites/default/files/ECMWF_Master_Logo_RGB_nostrap.png" alt="ECMWF logo" height="50"/> 

<!-- ![ML-based predictions schematic](ML_model_schematic.jpeg?raw=true "ML-based predictions")-->

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

#### 2021-05-27:

The organizers slightly adapted the [rules](#rules):
- The numerical RPSS scores of the training period (2000-2019) must be made available in the training notebook, see [example](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/ML_train_and_predict.ipynb).
- The safeguards for reproducibility in the [training and prediction template](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/ML_forecast_template.ipynb) have been adapted:
  - Code to reproduce training and predictions should run within a day on the described architecture. If the training takes longer than a day, please justify why this is needed. Please do not submit training piplelines, which take weeks to train. ~~Code to reproduce runs within a day~~ 

The organizers invite everyone to join two town hall meetings:
- Wednesday 2 June 2021 at 14:00 UTC [link](https://apcc.webex.com/apcc/j.php?MTID=m1e19150b271170bffa5d9ef307e96605) Meeting number: 184 987 2121 Password: 1234
- Thursday 10 June 2021 at  7:00 UTC [link](https://apcc.webex.com/apcc/j.php?MTID=m05c8900fd832be40b3d36a4b2df441c9) Meeting number: 184 416 1520 Password: 1234 
The meetings will include a 15-minutes presentation on the competition rules and technical aspects, followed by a 45-minutes discussion for Q&A.

A first version of the `s2s-ai-challenge-template` [repository](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template) is released. Please fork again or rebase. <!-- check if works -->

#### 2021-05-10: Rules adapted to discourage overfitting

The organizers modified the [rules](#rules):
- The codes used must be fully documented, with details of the safeguards enacted to prevent overfitting, see checked safeguards in [template](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/tree/master/notebooks/ML_forecast.ipynb).
- Code to reproduce submissions must be made available between November 1st and November 5th 2021 to enable open peer-review.
- The RPSS leaderboard will be hidden until early November 2021 once all participants made their code public.
- The final leaderboard will be ranked based on the numerical RPSS and a grade from expert peer-review for the top submissions.
- To have more time for this review, we shift the announcement of prizes into February 2022.
- Methods to create the 2020 forecasts must perform similar on new, unseen data. Therefore do not overfit.
- The organizers reserve the right to disqualify submissions if overfitting is suspected.
- To be considered for computational resources at EWC, you need to register until July 1st 2021 and explain why you cannot train your ML model elsewhere.

The organizers are aware that overfitting is an issue if the ground truth is accessible. A more robust verification would require predicting future states with weekly submissions over a year, which would take much more time until one year of new observations is available. Therefore, we decided against a real-time competition to shorten the project length and keep momentum high. Over time we will all see which methods genuinely have skill and which overfitted their available data. 


## Description
The World Meteorological Organization (WMO) is launching an open prize challenge to improve current forecasts of precipitation and temperature from today’s best computational fluid dynamical models 3 to 6 weeks  into the future using Artificial Intelligence and/or Machine Learning techniques. The challenge is part of the the Subseasonal-to-Seasonal Prediction Project ([S2S Project](http://s2sprediction.net/)), coordinated by the World Weather Research Programme ([WWRP](https://community.wmo.int/activity-areas/wwrp))/World Climate Research Programme ([WCRP](https://www.wcrp-climate.org/)), in collaboration with Swiss Data Science Center ([SDSC](https://datascience.ch/renku/)) and European Centre for Medium-Range Weather Forecasts ([ECMWF](https://www.ecmwf.int/)).

Improved sub-seasonal to seasonal (S2S) forecast skill would benefit multiple user sectors immensely, including water, energy, health, agriculture and disaster risk reduction. The creation of an extensive database of S2S model forecasts has provided a new opportunity to apply the latest developments in machine learning to improve S2S prediction of temperature and total precipitation forecasts up to 6 weeks ahead, with focus on biweekly averaged conditions around the globe. 

The competition will be implemented on the platform of Renkulab at the Swiss Data Science Center ([SDSC](https://datascience.ch/renku/)), which hosts all the codes and scripts. The training and verification data will be easily accessible from the [European Weather Cloud](https://github.com/ecmwf-lab/climetlab-s2s-ai-challenge) and relevant access scripts will be provided to the participants. All the codes and forecasts of the challenge will be made open access after the end of the competition.

This is the landing page of the competition presenting static information about the competition. For code examples and how to contribute, please visit the contribution template repository [renkulab.io](https://renkulab.io/projects/aaron.spring/s2s-ai-challenge-template/).

## Timeline

- 4th May 2021: Announcement of the competition
- 1st June 2021: Start of the competition (First date for submissions) <!-- registration: https://docs.google.com/forms/d/e/1FAIpQLSe49IleTxqEBFKzLdtkHvFQH0rPR16o6gfOFQ_L6cPzglAc2Q/viewform form available -->
- Town hall meetings for Q&A:
  - Wednesday 2 June 2021 at 14:00 UTC [link](https://apcc.webex.com/apcc/j.php?MTID=m1e19150b271170bffa5d9ef307e96605) Meeting number: 184 987 2121 Password: 1234
  - Thursday 10 June 2021 at  7:00 UTC [link](https://apcc.webex.com/apcc/j.php?MTID=m05c8900fd832be40b3d36a4b2df441c9) Meeting number: 184 416 1520 Password: 1234 
- 1st July 2021: Freeze the rules of the competition
- 31st October 2021: End of the competition (Final date for submissions)
- 1st November 2021: Participants make their code public
- 10th November 2021: Organizers make RPSS leaderboard public
- 1st November 2021 - January 2022: open peer review and experts peer review the top submissions
- February 2022: Release final leaderboard (based on RPSS and review grades); Announcement of the prizes


## Prize

Prizes will be awarded to for the top three submissions evaluated by RPSS and peer-review scores and must beat the re-calibrated ECMWF benchmark:

- Winning team: 15000 CHF
- 2nd team: 10000 CHF
- 3rd team: 5000 CHF

The 3rd prize is reserved for the top submission from developing or least developed country or small island state as per the [UN list (see table C, F, H p.166ff)](https://www.un.org/development/desa/dpad/wp-content/uploads/sites/45/WESP2020_Annex.pdf). If such a submissions is already among the top 2, the third submission will get the 3rd prize.


## Predictions

The organizers envisage two different approaches for Machine Learning-based predictions, which may be combined. Predict the week 3-4 & 5-6 state based on:

- I. the initial conditions (observations at `forecast_time`)
- II. the week 3-4 & 5-6 S2S forecasts

![ML-based predictions schematic](ML_model_schematic.jpeg?raw=true "ML-based predictions")

For the exact `valid_time`s to predict, see [timings](#timings). For the data to use us for training, see [data sources](#sources). Comply with the [rules](#rules).


## Evaluation

The objective of the competition is to improve week 3-4 (weeks 3 plus 4) and 5-6 (weeks 5 plus 6) subseasonal global probabilistic [2m temperature](https://confluence.ecmwf.int/plugins/servlet/mobile?contentId=27394104#content/view/27394104) and [total precipitation](https://confluence.ecmwf.int/plugins/servlet/mobile?contentId=27399606#content/view/27399606) forecasts issued in the year 2020 by using Machine Learning/Artificial Intelligence.

The evaluation will be continuously performed by a `scorer` bot on renkulab.io, following the [verification notebook](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/RPSS_verification.ipynb).
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

The `RPSS` relevant for the prizes is first calculated on each grid cell over land globally on a 1.5 degree grid.
This gridded RPSS is spatially averaged (weighted `(np.cos(np.deg2rad(ds.latitude)))`) over [90N-60S] land points and further averaged over both variables and both `lead_time`s. Furthermore, we apply a dry mask on total precipitation `tp` evaluation as in [Vigaud et al. 2017](https://doi.org/10.1175/MWR-D-17-0092.1), i.e. we exclude grid cells where the observations-based lower tercile edge is below 1 mm/day.

For diagnostics, we will further host leaderboards for the two variables in three regions in November 2021:

- Northern extratropics [90N-30N]
- Tropics (29N-29S)
- Southern extratropics [30S-60S]

Please find more details in the [verification notebook](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/RPSS_verification.ipynb).


## Submissions

We expect submissions to cover all bi-weekly week 3-4 and week 5-6 forecasts issued in 2020, see [timings](#timings). We expect one submission `netcdf` file for all 53 weekly forecasts issued in 2020. Submission must be gridded on a global 1.5 degree grid.

Each submission has to be a netcdf file with the folloing dimension sizes and coordinates:

```
>>> # in xarray
>>> ML_prediction_2020.sizes
Frozen(SortedKeysDict({'forecast_time': 53, 'latitude': 121, 'longitude': 240, 'lead_time': 2, 'category': 3}))

>>> ML_prediction_2020.coords
Coordinates:
  * latitude                 (latitude) float64 90.0 88.5 87.0 ... -88.5 -90.0
  * longitude                (longitude) float64 0.0 1.5 3.0 ... 357.0 358.5
  * forecast_time            (forecast_time) datetime64[ns] 2020-01...
  * lead_time                (lead_time) timedelta64[ns] 14 days 28 days
  * category                 (category) <U12 'below normal' 'near normal' 'above normal'
    valid_time               (lead_time, forecast_time) datetime64[ns] 2...
```
A template file for submissions is available [here](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/tree/master/submissions/ML_prediction_2020.nc). <!-- check -->

The submissions have to be commited in `git` with [`git lfs`](https://git-lfs.github.com/) in a [repository hosted by renkulab.io](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/).

After the competition, the code for training together with the gridded results must be made public, so the organizers and peer review can check adherence to the [rules](#rules).
Please indicate the resources used (number of CPUs/GPUs, memory, platform; see [safeguards in examples](#examples)) in your scripts/notebooks to allow reproducibility and document them fully to enable easy interpretation of the codes. Submissions, which cannot be independently reproduced by the organizers after the competition ends, cannot win prizes, please see [rules](#rules).


## Data

### Timings

The organizers explicitly choose to run this competition on past 2020 data, instead of a real-time competition to enable a much shorter competition period and to keep momentum high. We are aware of the dangers of [overfitting](https://en.wikipedia.org/wiki/Overfitting?wprov=sfti1) (see [rules](#rules)), if the ground truth data is accessible.

Please find here an explicit list of the forecast dates required.

1) Which forecast starts/target periods (weeks 3-4 & 5-6) to require to be submitted?
- 53 forecasts issued on Thursdays in 2020 (since there are available from all S2S models, including our ECMWF benchmark)
- In that case, the first forecast is issued S=2 Jan 2020, for the week 3-4 target 16-29 Jan.

Please find a list of the dates when forecasts are issued (`forecast_time` with CF standard_name `forecast_reference_time`) and corresponding start and end in `valid_time` for week 3-4 and week 5-6.

{% include_relative timings_2020.html %}
 
2) Which data to “allow” to be used to make a specific ML forecast?
- for the first forecast issued S=2 Jan 2020, any observational data up to the day of the forecast start (`forecast_time`), i.e. 2 Jan 2020
- any S2S forecast initialized up to and including S=2 Jan 2020

### Sources

Main datasets for this competition are already available as [renku datasets](https://renku.readthedocs.io/en/latest/user/data.html) and in [climetlab](https://github.com/ecmwf-lab/climetlab-s2s-ai-challenge) for both variables temperature and total precipitation. In [climetlab](https://github.com/ecmwf-lab/climetlab-s2s-ai-challenge), we have one dataset lab for the Machine Learning community and S2S forecasting community, which both lead to the same datasets:

| `tag in climetlab (ML community)` | `tag in climetlab (S2S community)` | Description | renku dataset |
| ------ | ------ | ----- | --- |
| `training-input` | `hindcast-input` | deterministic daily `lead_time` reforecasts/hindcasts initialized once per week 2000 to 2019 on dates of 2020 thursdays forecasts from models ECMWF, ECCC, NCEP| biweekly `lead_time`: `{model}_hindcast-input_2000-2019_biweekly_deterministic` |
| `test-input` | `forecast-input` | deterministic daily `lead_time` real-time forecasts initialized on thursdays 2020 from models ECMWF, ECCC, NCEP| biweekly `lead_time`: `{model}_forecast-input_2020_biweekly_deterministic` |
| `training-output-reference`| `hindcast-like-observations` | CPC daily observations formatted as 2000-2019 hindcasts with `forecast_time` and `lead_time` | biweekly `lead_time` deterministic: `hindcast-like-observations_2000-2019_biweekly_deterministic`; probabilistic in 3 categories: `hindcast-like-observations_2000-2019_biweekly_terciled` |
| `test-output-reference`| `forecast-like-observations` | CPC daily observations formatted as 2020 forecasts with `forecast_time` and `lead_time` | biweekly `lead_time`: `forecast-like-observations_2020_biweekly_deterministic`; binary in 3 categories: `forecast-like-observations_2020_biweekly_terciled` |
| `forecast-benchmark` | `forecast-benchmark` (not complete yet) | ECMWF week 3+4 & 5+6 re-calibrated probabilistic real-time 2020 forecasts in 3 categories | missing |
| `tercile_edges` | `tercile_edges` | Observations-based tercile category edges calculated from 2000-2019 | `hindcast-like-observations_2000-2019_biweekly_tercile-edges` |

Note that `tercile_edges` separating observations into the `category` below normal [0.-0.33), normal [0.33-0.67) or above normal [0.67-1.] depend on `longitude` (240), `latitude` (121), `lead_time` (46 days or 2 bi-weekly), `forecast_time.weekofyear` (53) and `category_edge` (2).
<!--Not all available yet, also not yet cleaned.-->

We encourage to use subseasonal forecasts from the [`S2S`](https://doi.org/10.1175/BAMS-D-16-0017.1) and [`SubX`](http://journals.ametsoc.org/doi/full/10.1175/BAMS-D-18-0270.1) projects:

- S2S Project
  - [climetlab](https://github.com/ecmwf-lab/climetlab-s2s-ai-challenge) for models ECMWF, ECCC and NCEP from the European Weather Cloud
  - IRI Data Library ([IRIDL](https://iridl.ldeo.columbia.edu/SOURCES/.ECMWF/.S2S)) for all S2S models via [opendap](https://en.wikipedia.org/wiki/OPeNDAP) 
  - [s2sprediction.net](http://s2sprediction.net) for data access from ECMWF and CMA
- SubX Project
  - [IRIDL](http://iridl.ldeo.columbia.edu/SOURCES/.Models/.SubX) all SubX models via [opendap](https://en.wikipedia.org/wiki/OPeNDAP) 

However, any other publicly available data sources (like CMIP, NMME, etc.) of dates prior to the `forecast_time` can be used for `training-input` and `forecast-input`.
Also purely empirical methods like persistence or climatology could be used. The only essential data requirement concerns forecast times and dates, see [timings](#timings).

Ground truth sources are [NOAA CPC](https://www.cpc.ncep.noaa.gov/) temperature and total precipitation from [IRIDL](http://iridl.ldeo.columbia.edu/):
- `pr`: [precipitation rate](http://iridl.ldeo.columbia.edu/SOURCES/.NOAA/.NCEP/.CPC/.UNIFIED_PRCP/.GAUGE_BASED/.GLOBAL/.v1p0/.extREALTIME/.rain) to accumulate to `tp`
- `t2m`: [2m temperature](http://iridl.ldeo.columbia.edu/SOURCES/.NOAA/.NCEP/.CPC/.temperature/.daily/)

### Examples

- [Train ML model template](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/ML_forecast_template.ipynb).
- [Train ML model](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/ML_train_and_predict.ipynb).
- [Mean bias reduction](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/mean_bias_reduction.ipynb)
- [Score RPSS ML model vs ECMWF](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge/-/blob/master/notebooks/RPSS_verification.ipynb).

## Join

Follow the steps [in the template renku project](https://renkulab.io/projects/aaron.spring/s2s-ai-challenge-template).

## Training

Where to train?

- [renkulab.io](https://renkulab.io) provides free but limited compute resources. You may use upto 2 CPUs, 8 GB memory and 10 GB disk space.
- As renku projects are `git` repositories under the hood, you can `renku clone` or `git clone` your project onto your own laptop or supercomputer account for the heavy lifting.
- ECMWF may provide limited compute nodes on the European Weather Cloud `EWC` (where large parts of the data is stored) upon request. This opportunity is specifically targeted for participants from developing or least developed country or small island states and/or without institutional computing resources. Please indicate **why** you need compute access and cannot train your model elsewhere in the registration form. To be considered for such computational resources at EWC, you need to register by June 1st 2021 or July 1st 2021. *Please note that we cannot make promises about these resources given the unknown demand.*
<!-- TODO -->
How to train?

We are looking for your smart solutions here. Find a quick start template [here](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/ML_forecast_template.ipynb).

## Discussion

Please use the issue tracker in the renkulab [`s2s-ai-challenge` gitlab repository](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge/-/issues) for 
[questions](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge/-/issues/new) to the [organizers](#organizers), [discussions](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge/-/issues/new?issuable_template=discussion),
[bug reports](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge/-/issues/new?issuable_template=bug).
We have set up a [![Gitter chat room](https://badges.gitter.im/pangeo-data/s2s-ai-challenge.svg)](https://gitter.im/pangeo-data/s2s-ai-challenge?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge) for informal communication.

## FAQ

Answered questions from the issue tracker are regularly transferred to the [FAQ](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge/-/wikis/Frequently-Asked-Questions-(FAQ)).

## Leaderboard

### RPSS

Submissions have to beat the ECMWF re-calibrated benchmark and following the [rules](#rules) to qualify for prizes. <!-- description of ECMWF benchmark missing -->

The leaderboard will be made public after the submission period ends and submission codes have been made public, i.e. early November 2021.

We will also publish RPSS subleaderboards, that are purely diagnostic and show RPSS for two variables (`t2m`, `tp`), two `lead_time`s (weeks 3-4 & 5-6) and three subregions ([90N-30N], (30N-30S), [30S-60S]).

### Peer-review

From November 2021 to January 2022, there will be open peer review and expert peer-reviews for the top ranked submissions. Peer review will evaluate:

- whether the code is written in a clean and understandable way
- whether the code gives reproducible results by an independent person
- the originality of the method
- whether the safeguards against overfitting and reproducibility have been followed and overfitting has been avoided
- tbd

(Still under discussion) Based on these criteria, there will be a peer-review grade from poor (-1) towards brilliant (+1).

### Final

The top three submissions based on the combined RPSS and expert peer-review score will receive prizes.
(Still under discussion whether the scores are averaged or ranked and the two ranks averaged.)

## Rules

- One team can only get one prize. One Person can only join one team.
- Prizes are only issued if the method beats the re-calibrated ECMWF benchmark.
- To be eligible for the third prize reserved for submissions from developing or least developed country or small island states, all team members must be resident in such countries. 
- Model training is not allowed to use the ground truth/observations data after forecast was issued, see [Data Timings](#timings).
<!-- - [Data leakage](https://en.wikipedia.org/wiki/Leakage_(machine_learning)?wprov=sfti1) is not allowed, i.e. do not use `lead_time=0 days` as predictor. -->
- Do not [overfit](https://en.wikipedia.org/wiki/Overfitting?wprov=sfti1), a credible model is one that continues to perform similarly on new unseen data.
- Also the RPSS score (gridded not required) over the training period 2000-2019 must be provided in the submissions.
- The codes used must be fully documented, with details of the safeguards enacted to prevent overfitting, see checked safeguards in [template](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/ML_forecast_template.ipynb).
- The RPSS leaderboard will be made public in early November 2021, once all submissions are public.
- The submitted codes and gridded results to all submissions must be made public on November 5th 2021 to be accessible for open peer review, and for expert peer review of the top submissions, which will last until January 2022. Submissions, which are not made public on November 1st 2021, will be removed from the leaderboard. 
- The organizers reserve the right to disqualify submissions if overfitting is suspected.
- Prizes will be issued in early February 2022.
- These rules may be changed by the organizers until 1st July 2021. <!-- Under which circumstances are organizers allowed to change rules later on? -->


## Organizers

- [WMO](https://public.wmo.int/en)/[WWRP](https://community.wmo.int/activity-areas/wwrp): Estelle De Coning, Wenchao Cao
- [WCRP](https://www.wcrp-climate.org/): Michel Rixen
- [S2S Project](http://s2sprediction.net/): Frederic Vitart, Andy Robertson
- [ECMWF](https://www.ecmwf.int/): Florian Pinault, Baudouin Raoult
- [SDSC](https://datascience.ch/renku/): Rok Roskar
- WMO contractor/main contact: [Aaron Spring](mailto:aaron.spring@mpimet.mpg.de) [@aaronspring](https://github.com/aaronspring/) [@realaaronspring](https://twitter.com/realaaronspring/)

<img src="https://community.wmo.int/themes/wmo/logo.png" alt="WMO logo" height="50"/> <img src="https://ho9an2-datap1.s3.eu-west-1.amazonaws.com/wmoext/s3fs-public/wwrp_logo_small_002.jpg" alt="WWRP logo" height="50"/> <img src="https://www.wcrp-climate.org/images/logos/WCRP_structured_data.png" alt="WCRP logo" height="50"/> <img src="https://www.wcrp-climate.org/images/logos_icones/logo_S2S.png" alt="S2S logo" height="50"/> <img src="https://datascience.ch/wp-content/uploads/2020/09/logo-SDSC-transparent-300x82.png" alt="SDSC logo" height="50"/> <img src="https://www.ecmwf.int/sites/default/files/ECMWF_Master_Logo_RGB_nostrap.png" alt="ECMWF logo" height="50"/> 

<!---
Todo:
- make leaderboard dynamic with bokeh: https://p-mckenzie.github.io/2017/12/01/embedding-bokeh-with-github-pages/ https://github.com/bokeh/bokeh/tree/branch-2.4/examples/app/export_csv
-->
