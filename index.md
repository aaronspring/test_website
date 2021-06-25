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

<img src="https://community.wmo.int/themes/wmo/logo.png" alt="WMO logo" height="35"/> <img src="https://ho9an2-datap1.s3.eu-west-1.amazonaws.com/wmoext/s3fs-public/wwrp_logo_small_002.jpg" alt="WWRP logo" height="35"/> <img src="https://www.wcrp-climate.org/images/logos/WCRP_structured_data.png" alt="WCRP logo" height="35"/> <img src="https://www.wcrp-climate.org/images/logos_icones/logo_S2S.png" alt="S2S logo" height="35"/> <img src="https://datascience.ch/wp-content/uploads/2020/09/logo-SDSC-transparent-300x82.png" alt="SDSC logo" height="35"/> <img src="https://www.ecmwf.int/sites/default/files/ECMWF_Master_Logo_RGB_nostrap.png" alt="ECMWF logo" height="35"/> 


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

#### 2021-06-29:

- More details about the review process added, see [leaderboard](#leaderboard).
- Small change to the [rules](#rules):
    - "The decision about the review grade is final and there will be no correspondence about the review."  
- The organizers freeze the rules.


#### 2021-06-19:

- Small change to the [rules](#rules) after feedback from the town halls:
    - One team can only get one prize. ~~One Person can only join one team.~~ NEW: Teams with with overlapping members must use considerably different methods to be considered for prizes both. One person can only join three teams at maximum.
    - Prizes are only issued if the method beats the re-calibrated ECMWF benchmark AND CLIMATOLOGY.
- Answers to many questions asked in the town hall meetings can be found in the [FAQ](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge/-/wikis/Frequently-Asked-Questions-(FAQ))
- [`climetlab_s2s_ai_challenge.extra.forecast_like_observations`](https://github.com/ecmwf-lab/climetlab-s2s-ai-challenge/blob/main/climetlab_s2s_ai_challenge/extra.py#L40) converts obserations with `time` dimension to the same dimensions as initialized forecasts with dimension `forecast_time` and `lead_time`. This helper function can be used for `training/hindcast-input` with `model='ncep'` and any other initialized forecasts (e.g. from [SubX](http://iridl.ldeo.columbia.edu/SOURCES/.Models/.SubX/) or [S2S](https://iridl.ldeo.columbia.edu/SOURCES/.ECMWF/.S2S/), see [`IRIDL.ipynb` example](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/data_access/IRIDL.ipynb))
    ```python
    from climetlab_s2s_ai_challenge.extra import forecast_like_observations
    import climetlab as cml
    import climetlab_s2s_ai_challenge
    print(climetlab_s2s_ai_challenge.__version__, cml.__version__)  # must be >= 0.7.1, 0.8.0

    forecast = cml.load_dataset('s2s-ai-challenge-training-input',
        date=20100107, origin='ncep', parameter='tp',
        format='netcdf').to_xarray()

    obs_lead_time_forecast_time = cml.load_dataset('s2s-ai-challenge-observations', parameter=['pr', 't2m']).to_xarray(like=forecast)
    # equivalent
    obs_ds = cml.load_dataset('s2s-ai-challenge-observations', parameter=['pr', 't2m']).to_xarray()
    obs_lead_time_forecast_time = forecast_like_observations(forecast, obs_ds)
    obs_lead_time_forecast_time
    <xarray.Dataset>
    Dimensions:        (forecast_time: 12, latitude: 121, lead_time: 44, longitude: 240)
    Coordinates:
        valid_time     (forecast_time, lead_time) datetime64[ns] 1999-01-08 ... 2...
      * latitude       (latitude) float64 90.0 88.5 87.0 85.5 ... -87.0 -88.5 -90.0
      * longitude      (longitude) float64 0.0 1.5 3.0 4.5 ... 355.5 357.0 358.5
      * forecast_time  (forecast_time) datetime64[ns] 1999-01-07 ... 2010-01-07
      * lead_time      (lead_time) timedelta64[ns] 1 days 2 days ... 43 days 44 days
    Data variables:
        t2m            (forecast_time, lead_time, latitude, longitude) float32 ...
        tp             (forecast_time, lead_time, latitude, longitude) float32 na...
    Attributes:
        script:   climetlab_s2s_ai_challenge.extra.forecast_like_observations
    ```
- Town hall [recordings for June 2nd](https://elioscloud.wmo.int/share/s/zsc-ufPvQNqgVca8vF5e9Q) and [June 10th](https://elioscloud.wmo.int/share/s/fHyFoURbQjCebsbQSSVlww) and [slides](https://elioscloud.wmo.int/share/s/82Ug2wVRT5CN2AFGFtTAqQ) are available
- Applicants for EWC compute resources have be contacted June 16th with a concrete proposal of computational resources and asked to provide a reason why they could not participate without these resources if they have not answered the question before. ECMWF can provide 20 machines, please use the resources responsibly and notify us if you do not need them anymore. You can still ask Aaron if some EWC compute instance if currently unused.
- Rok from SDSC offers to deliver a `renku` workshop, please indicate your interest [here](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge/-/issues/7)
- [`s2saichallengescorer`](https://renkulab.io/gitlab/tasko.olevski/s2s-ai-competition-scoring-image/-/blob/master/scoring/scoring_script.py) clips all `RPSS` grid cells to interval [-10, 1]. Where `NaN` provided but number expected, we penalize by `-10`. [!2](https://renkulab.io/gitlab/tasko.olevski/s2s-ai-competition-scoring-image/-/merge_requests/2) [!9](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/merge_requests/9)
- Updated template file for submissions in [`s2s-ai-challenge-template`](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/tree/master/submissions/ML_prediction_2020.nc) and [submissions](#submissions), with coordinate descriptions in the `html` repr.
- Refine description how biweekly aggregates are computed, see [`aggregate_biweekly`](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/scripts.py) and the attributes in the [submissions](#submissions) [template file](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/tree/master/submissions/ML_prediction_2020.nc). Recomputed [biweekly observations](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/data/forecast-like-observations_2020_biweekly_terciled.nc), which will be used by `s2saichallengescorer` [s2s-ai-challenge#22](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge/-/issues/22).
- Added [`IRIDL.ipynb`](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/data_access/IRIDL.ipynb) with server-side preprocessing for `S2S` and `SubX` models as well as setting your IRIDL cookie to access restricted `S2S` output.
- Please remember to `git add current_notebook.ipynb && git commit -m 'message'` before `git tag` to ensure that the uptodate notebook version is also tagged. Please also consider [linting](https://realpython.com/python-code-quality/#linters) your code.
- For changes to [`s2s-ai-challenge-template`](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template), see [CHANGELOG.md](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/CHANGELOG.md) and best use release [`v0.3.1`](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/tags/v0.3.1_release).
- Participants are encouraged to submit their 2020 forecasts ([instructions](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/README.md)) well before the end of the competition to ensure that their submissions are successfully scored by the [`s2saichallengescorer`](https://renkulab.io/gitlab/tasko.olevski/s2s-ai-competition-scoring-image/-/blob/master/scoring/scoring_script.py). Check whether your submission committed in the last 24h was successfully checked by [`s2saichallengescorer`](https://renkulab.io/gitlab/tasko.olevski/s2s-ai-competition-scoring-image/-/blob/master/scoring/scoring_script.py) [here](https://renkulab.io/gitlab/tasko.olevski/s2s-ai-competition-scoring-image/-/blob/master/README.md), where no numerical scores are shown, only `completed` or `failed`. Note that we provide the scoring script as `skill_by_year` and ground truth data for responsible use only. 


#### 2021-05-31: Template repository and scorer bot ready

- We will start accepting submissions from tomorrow June 1st to October 31st 2021.
- The [template](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template) on [renkulab.io](https://renkulab.io/projects/aaron.spring/s2s-ai-challenge-template/environments) is ready to use. You can also work on such a `git` repository locally. Please play around, raise questions or report bugs in the [competition issue tracker](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge/-/issues/new?).
- Follow [these steps](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/README.md) to join the competition.


#### 2021-05-27: Town hall dates and EWC compute deadline changes

The organizers slightly adapted the [rules](#rules):
- The numerical RPSS scores of the training period must be made available in the training notebook, see [example](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/ML_train_and_predict.ipynb).
- The safeguards for reproducibility in the [training and prediction template](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/ML_forecast_template.ipynb) have been adapted:
  - Code to reproduce training and predictions are prefered to run within a day on the described architecture. If the training takes longer than a day, please justify why this is needed. Please do not submit training piplelines, which several take weeks to train. ~~Code to reproduce runs within a day~~ 

The organizers invite everyone to join two town hall meetings:
- Wednesday 2 June 2021 at 14:00 UTC [recording](https://elioscloud.wmo.int/share/s/zsc-ufPvQNqgVca8vF5e9Q)
- Thursday 10 June 2021 at  7:00 UTC [recording](https://elioscloud.wmo.int/share/s/fHyFoURbQjCebsbQSSVlww)  
- [slides](https://elioscloud.wmo.int/share/s/82Ug2wVRT5CN2AFGFtTAqQ)


The meetings will include a 15-minutes presentation on the competition rules and technical aspects, followed by a 45-minutes discussion for Q&A.

A first version of the `s2s-ai-challenge-template` [repository](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template) is released. Please fork again or rebase.

The deadline to apply for EWC compute access is shifted to 15th June 2021. Please use the [competition registration form](https://docs.google.com/forms/d/e/1FAIpQLSe49IleTxqEBFKzLdtkHvFQH0rPR16o6gfOFQ_L6cPzglAc2Q/viewform) to explain why you need compute resources. Please note that ECMWF just provides access to EWC, but not detailed support of how to setup your environments etc.

The RPSS formula has change to incorporate the RPSS with respect to climatology, see [evaluation](#evaluation).


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

Prizes will be awarded to for the top three submissions evaluated by RPSS and peer-review scores and must beat the calibrated ECMWF benchmark and climatology. The ECMWF recalibration has been performed by using the tercile boundaries from the model climatology rather than from observations:

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

The objective of the competition is to improve week 3-4 (weeks 3 plus 4) and 5-6 (weeks 5 plus 6) subseasonal global probabilistic [2m temperature](https://confluence.ecmwf.int/plugins/servlet/mobile?contentId=27394104#content/view/27394104) and [total precipitation](https://confluence.ecmwf.int/plugins/servlet/mobile?contentId=27399606#content/view/27399606) tercile forecasts issued in the year 2020 by using Machine Learning/Artificial Intelligence.

The evaluation will be continuously performed by a [`s2saichallengescorer` bot](https://renkulab.io/gitlab/tasko.olevski/s2s-ai-competition-scoring-image/-/blob/master/scoring/scoring_script.py), following the [verification notebook](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/RPSS_verification.ipynb).
Submissions are evaluated on the Ranked Probability Score (`RPS`) between the ML-based forecasts and ground truth CPC [temperature](http://iridl.ldeo.columbia.edu/SOURCES/.NOAA/.NCEP/.CPC/.temperature/.daily/) and accumulated [precipitation](http://iridl.ldeo.columbia.edu/SOURCES/.NOAA/.NCEP/.CPC/.UNIFIED_PRCP/.GAUGE_BASED/.GLOBAL/.v1p0/.extREALTIME/.rain) [observations based on pre-computed observations-based terciles](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/data/forecast-like-observations_2020_biweekly_terciled.nc) calculated in [renku_datasets_biweekly.ipynb](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/renku_datasets_biweekly.ipynb). This `RPS` is compared to the climatology forecast in the Ranked Probability Skill Score (`RPSS`). The ML-based forecasts should beat the re-calibrated real-time 2020 ECMWF and climatology forecasts to be able to win prizes, see [end of verification notebook](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/RPSS_verification.ipynb).

`RPS` is calculated with the open-source package [xskillscore](https://xskillscore.readthedocs.io/en/latest) over all 2020 `forecast_time`s.
For probabilistic forecasts:
```python
xs.rps(observations, probabilistic_forecasts, category_edges=None, input_distributions='p', dim='forecast_time')
```

See the [`xskillscore.rps` API](https://xskillscore.readthedocs.io/en/latest/api/xskillscore.rps.html) for details.

```python
def RPSS(rps_ML, climatology):
    """Ranked Probability Skill Score with respect to climatology.
  
    +---------+-------------------------------------------+
    |  Score  | Description                               |
    +---------+-------------------------------------------+
    |    1    | maximum, perfect improvement              |
    +---------+-------------------------------------------+
    |  (0,1]  | positive means ML better than climatology |
    +---------+-------------------------------------------+
    |    0    | equal performance                         |
    +---------+-------------------------------------------+
    | (0, -∞) | negative means ML worse than climatology  |
    +---------+-------------------------------------------+

  """
  return 1 - rps_ML / climatology
```

The `RPSS` relevant for the prizes is first calculated on each grid cell over land globally on a 1.5 degree grid. In grid cells where numerical values are expected but `NaN`s are provided, the `RPSS` is [penalized](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/issues/7) with -10. The `RPSS` values are [clipped](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/issues/7) to the interval [-10, 1].
This gridded RPSS is then averaged over all `forecast_time`s, spatially averaged (weighted `(np.cos(np.deg2rad(ds.latitude)))`) over [90N-60S] land points and further averaged over both variables and both `lead_time`s. Please note that the observational probabilities are applied with a dry mask on total precipitation `tp` evaluation as in [Vigaud et al. 2017](https://doi.org/10.1175/MWR-D-17-0092.1), i.e. we exclude grid cells where the observations-based lower tercile edge is below 1 mm/day. Please find the ground truth compared against [here](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/data/forecast-like-observations_2020_biweekly_terciled.nc).

For diagnostics, we will further host leaderboards for the two variables in three regions in November 2021:

- Northern extratropics [90N-30N]
- Tropics (29N-29S)
- Southern extratropics [30S-60S]

Please find more details in the [verification notebook](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/RPSS_verification.ipynb).


## Submissions

We expect submissions to cover all bi-weekly week 3-4 and week 5-6 forecasts issued in 2020, see [timings](#timings). We expect one submission `netcdf` file for all 53 forecasts issued on thursdays in 2020. Submissions must be gridded on a global 1.5 degree grid.

Each submission has to be a netcdf file with the following dimension sizes and coordinates:

{% include_relative submission_template_repr.html %}

This template submissions file is available [here](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/tree/master/submissions/ML_prediction_2020.nc).

Click on 📄 to see the metadata for the coordinates and variables.


We deal with two fundamentally different variables here:
[Total precipitation](https://confluence.ecmwf.int/display/S2S/S2S+Total+Precipitation) is precipitation flux `pr` accumulated over `lead_time` until `valid_time` and therefore describes a point observation.
[2m temperature](https://confluence.ecmwf.int/display/S2S/S2S+Surface+Air+Temperature) is averaged over `lead_time`(`valid_time`) and therefore describes an average observation.
The submission file data model unifies both approaches and assigns `14 days` for week 3-4 and `28 days` for week 5-6 marking the first day of the biweekly aggregate.

Submissions have to be commited in `git` with [`git lfs`](https://git-lfs.github.com/) in a [repository hosted by renkulab.io](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/).

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

| `tag in climetlab (ML community)` | `tag in climetlab (S2S community)` | Description | renku dataset(s) |
| ------ | ------ | ----- | --- |
| `training-input` | `hindcast-input` | deterministic daily `lead_time` reforecasts/hindcasts initialized once per week 2000 to 2019 on dates of 2020 thursdays forecasts from models ECMWF, ECCC, NCEP| biweekly `lead_time`: `{model}_hindcast-input_2000-2019_biweekly_deterministic.zarr` |
| `test-input` | `forecast-input` | deterministic daily `lead_time` real-time forecasts initialized on thursdays 2020 from models ECMWF, ECCC, NCEP| biweekly `lead_time`: `{model}_forecast-input_2020_biweekly_deterministic.zarr` |
| `training-output-reference`| `hindcast-like-observations` | CPC daily observations formatted as 2000-2019 hindcasts with `forecast_time` and `lead_time` | biweekly `lead_time` deterministic: `hindcast-like-observations_2000-2019_biweekly_deterministic.zarr`; probabilistic in 3 categories: `hindcast-like-observations_2000-2019_biweekly_terciled.zarr` |
| `test-output-reference`| `forecast-like-observations` | CPC daily observations formatted as 2020 forecasts with `forecast_time` and `lead_time` | biweekly `lead_time`: `forecast-like-observations_2020_biweekly_deterministic.zarr`; binary in 3 categories: `forecast-like-observations_2020_biweekly_terciled.nc` |
| `training-output-benchmark` | `hindcast-benchmark` | ECMWF week 3+4 & 5+6 re-calibrated probabilistic 2000-2019 hindcasts in 3 categories | - |
| `test-output-benchmark` | `forecast-benchmark` | ECMWF week 3+4 & 5+6 re-calibrated probabilistic real-time 2020 forecasts in 3 categories | `ecmwf_recalibrated_benchmark_2020_biweekly_terciled.nc` |
| - | - | Observations-based tercile category edges calculated from 2000-2019 | `hindcast-like-observations_2000-2019_biweekly_tercile-edges.nc` |

Note that `tercile_edges` separating observations into the `category` `"below normal"` [0.-0.33), `"near normal"` [0.33-0.67) or `"above normal"` [0.67-1.] depend on `longitude` (240), `latitude` (121), `lead_time` (46 days or 2 bi-weekly), `forecast_time.weekofyear` (53) and `category_edge` (2).

We encourage to use subseasonal forecasts from the [`S2S`](https://doi.org/10.1175/BAMS-D-16-0017.1) and [`SubX`](http://journals.ametsoc.org/doi/full/10.1175/BAMS-D-18-0270.1) projects:

- S2S Project
  - [climetlab](https://github.com/ecmwf-lab/climetlab-s2s-ai-challenge) for models ECMWF, ECCC and NCEP from the European Weather Cloud
  - IRI Data Library ([IRIDL](https://iridl.ldeo.columbia.edu/SOURCES/.ECMWF/.S2S)) for all S2S models via [opendap](https://en.wikipedia.org/wiki/OPeNDAP) 
  - [s2sprediction.net](http://s2sprediction.net) for data access from ECMWF and CMA
- SubX Project
  - [IRIDL](http://iridl.ldeo.columbia.edu/SOURCES/.Models/.SubX) all SubX models via [opendap](https://en.wikipedia.org/wiki/OPeNDAP) 

However, any other publicly available data sources (like [CMIP](https://www.wcrp-climate.org/wgcm-cmip), [NMME](http://iridl.ldeo.columbia.edu/SOURCES/.Models/.NMME/), [DCPP](https://www.wcrp-climate.org/modelling-wgcm-mip-catalogue/cmip6-endorsed-mips-article/1065-modelling-cmip6-dcpp) etc.) of dates prior to the `forecast_time` can be used for `training-input` and `forecast-input`.
Also purely empirical methods like persistence or climatology could be used. The only essential data requirement concerns forecast times and dates, see [timings](#timings).

Ground truth sources are [NOAA CPC](https://www.cpc.ncep.noaa.gov/) temperature and total precipitation from [IRIDL](http://iridl.ldeo.columbia.edu/):
- `pr`: [precipitation rate](http://iridl.ldeo.columbia.edu/SOURCES/.NOAA/.NCEP/.CPC/.UNIFIED_PRCP/.GAUGE_BASED/.GLOBAL/.v1p0/.extREALTIME/.rain) to accumulate to `tp`
- `t2m`: [2m temperature](http://iridl.ldeo.columbia.edu/SOURCES/.NOAA/.NCEP/.CPC/.temperature/.daily/)

### Examples

- [Train ML model template](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/ML_forecast_template.ipynb)
- [Train ML model](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/ML_train_and_predict.ipynb)
- [Mean bias reduction](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/mean_bias_reduction.ipynb)
- [Score RPSS ML model vs ECMWF](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge/-/blob/master/notebooks/RPSS_verification.ipynb)

## Join

Follow the steps [in the template renku project](https://renkulab.io/projects/aaron.spring/s2s-ai-challenge-template).

## Training

Where to train?

- [renkulab.io](https://renkulab.io) provides free but limited compute resources. You may use upto 2 CPUs, 8 GB memory and 10 GB disk space.
- As renku projects are `git` repositories under the hood, you can `renku clone` or `git clone` your project onto your own laptop or supercomputer account for the heavy lifting.
- ECMWF may provide limited compute nodes on the European Weather Cloud `EWC` (where large parts of the data is stored) upon request. This opportunity is specifically targeted for participants from developing or least developed country or small island states and/or without institutional computing resources. Please indicate **why** you need compute access and cannot train your model elsewhere in the registration form. To be considered for such computational resources at EWC, you need to register by June 15th 2021. *Please note that we cannot make promises about these resources given the unknown demand.*

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

Submissions have to beat the ECMWF re-calibrated benchmark and climatology while following the [rules](#rules) to qualify for prizes.

The leaderboard will be made public after the submission period ends and submission codes have been made public, i.e. early November 2021.

We will also publish RPSS subleaderboards, that are purely diagnostic and show RPSS for two variables (`t2m`, `tp`), two `lead_time`s (weeks 3-4 & 5-6) and three subregions ([90N-30N], (30N-30S), [30S-60S]).

### Peer review

From November 2021 to January 2022, there will be two peer review processes:

- open peer review for all submissions
- expert peer-reviews for the top ranked submissions

Peer review will evaluate:

- whether the method is well explained
- whether the code is written in a clean and understandable way
- whether the code gives reproducible results by an independent person
- the originality of the method
- whether the safeguards against overfitting and reproducibility have been followed and overfitting has been avoided


#### Open peer review

One goal of this challenge is to foster a conversation about how AI/ML can improve S2S forecasts.
Therefore, we will open the floor for discussions and evaluating all methods submitted in an open peer review process.
The organizers will create a table of all submissions and everyone is invited to comment on submissions,
like in the [EGU's public interactive discussions](https://www.egu.eu/publications/statement/online-open-access-publishing/).
This open peer review will be hosted on renku's gitlab.


#### Expert peer review

The organizers decided that the top four submissions will be evaluated by expert peer review.
This will include 2-3 reviews by experts from the fields of S2S & AI/ML.
Additionally, the organizers will host a public showcase session in January 2022, in which these top four submission can present their method in 10 minutes followed by 15 minutes Q&A. The reviewers will give their review grades after an internal discussion moderated by Andrew Robertson and Frederic Vitart acting as editors.

Based on the criteria above, the expert peer reviewers will give a peer review grade. Comments from the open peer review can be taken into account by the expert peer review grades.


### Final

The review grades will be ranked. The RPSS leaderboard will also be ranked. The final leaderboard will be determined from the average of both rankings.
When two submissions have the same mean ranking, the RPSS ranking counts more.
The top three submissions based on the combined RPSS and expert peer-review score will receive prizes.


## Rules

- One team can only get one prize. Teams with with overlapping members must use considerably different methods to be considered for prizes both. One person can only join three teams at maximum.
- Prizes are only issued if the method beats the re-calibrated ECMWF benchmark and climatology.
- To be eligible for the third prize reserved for submissions from developing or least developed country or small island states, all team members must be resident in such countries. 
- Model training is not allowed to use the ground truth/observations data after forecast was issued, see [Data Timings](#timings).
<!-- - [Data leakage](https://en.wikipedia.org/wiki/Leakage_(machine_learning)?wprov=sfti1) is not allowed, i.e. do not use `lead_time=0 days` as predictor. -->
- Do not [overfit](https://en.wikipedia.org/wiki/Overfitting?wprov=sfti1), a credible model is one that continues to perform similarly on new unseen data.
- Also the RPSS score (gridded not required) over the training period (e.g. 2000-2019 or what is available for your inputs) must be provided in the submissions.
- The codes used must be fully documented, with details of the safeguards enacted to prevent overfitting, see checked safeguards in [template](https://renkulab.io/gitlab/aaron.spring/s2s-ai-challenge-template/-/blob/master/notebooks/ML_forecast_template.ipynb).
- The RPSS leaderboard will be made public in early November 2021, once all submissions are public.
- The submitted codes and gridded results to all submissions must be made public on November 5th 2021 to be accessible for open peer review, and for expert peer review of the top submissions, which will last until January 2022. Submissions, which are not made public on November 1st 2021, will be removed from the leaderboard. 
- The decision about the review grade is final and there will be no correspondence about the review. 
- The organizers reserve the right to disqualify submissions if overfitting is suspected.
- Prizes will be issued in early February 2022.


## Organizers

- [WMO](https://public.wmo.int/en)/[WWRP](https://community.wmo.int/activity-areas/wwrp): Estelle De Coning, Wenchao Cao
- [WCRP](https://www.wcrp-climate.org/): Nico Caltabiano, Michel Rixen
- [S2S Project](http://s2sprediction.net/): Frederic Vitart, Andy Robertson
- [ECMWF](https://www.ecmwf.int/): Florian Pinault, Baudouin Raoult
- [SDSC](https://datascience.ch/renku/): Rok Roskar
- WMO contractor/main contact: [Aaron Spring](mailto:aaron.spring@mpimet.mpg.de) [@aaronspring](https://github.com/aaronspring/) [@realaaronspring](https://twitter.com/realaaronspring/)

<img src="https://community.wmo.int/themes/wmo/logo.png" alt="WMO logo" height="35"/> <img src="https://ho9an2-datap1.s3.eu-west-1.amazonaws.com/wmoext/s3fs-public/wwrp_logo_small_002.jpg" alt="WWRP logo" height="35"/> <img src="https://www.wcrp-climate.org/images/logos/WCRP_structured_data.png" alt="WCRP logo" height="35"/> <img src="https://www.wcrp-climate.org/images/logos_icones/logo_S2S.png" alt="S2S logo" height="35"/> <img src="https://datascience.ch/wp-content/uploads/2020/09/logo-SDSC-transparent-300x82.png" alt="SDSC logo" height="35"/> <img src="https://www.ecmwf.int/sites/default/files/ECMWF_Master_Logo_RGB_nostrap.png" alt="ECMWF logo" height="35"/> 

<!---
Todo:
- make leaderboard dynamic with bokeh: https://p-mckenzie.github.io/2017/12/01/embedding-bokeh-with-github-pages/ https://github.com/bokeh/bokeh/tree/branch-2.4/examples/app/export_csv
-->
