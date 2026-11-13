# Solar Flare Class Prediction from SHARP and GOES Data

DS440 Capstone · Domenic Cabibbo, Tjoet Lakmana, Ayanna Norfleet, Darshan Sivarajah, Nur Sakinah Binti Mohammad Zaini

We predict the **largest solar flare an active region will produce in the next 24 hours** (none, C, M or X) from hourly magnetic field measurements, comparing five baseline models from prior studies with four new models.

**Report:** [link] (Still being worked on)

## Data

Our dataset combines two free public sources:

- **SHARP magnetic field data (inputs):** hourly magnetic measurements of each solar active region from NASA's Solar Dynamics Observatory, series `hmi.sharp_720s`, from Stanford's [JSOC](http://jsoc.stanford.edu) (free [registration](http://jsoc.stanford.edu/ajax/register_email.html) required), downloaded with the [`drms`](https://docs.sunpy.org/projects/drms/) Python package.
- **GOES X-ray flare catalog (labels):** NOAA's list of every solar flare with its class, times and active region. We used the XRS Flare Report from this NCEI directory: <https://data.ngdc.noaa.gov/platforms/solar-space-observing-satellites/goes/multi/l2/data/xrsf-l2-flrpt_science/>

Flares were matched to SHARP regions by NOAA region number, using JSOC's [HARP-to-NOAA table](http://jsoc.stanford.edu/doc/data/hmi/harpnum_to_noaa/all_harps_with_noaa_ars.txt). Each hour was labeled with the largest flare the region produced in the next 24 hours: none (including A and B flares), C, M or X. Gaps of up to 3 hours were forward-filled. Data was split by region in time order: train 2010–2021, validation 2021–2023, test 2023–2026.

**Final dataset:** `clean_flares_dataaset.csv` (the typo in the name is intentional, since the notebooks use it). It has 675,750 hourly rows from May 2010 to August 2026. [Download link]

**Data dictionary:** `data/Flare_Data_Dictionary.docx`. `label` is the target and describes the **next 24 hours**, so it is never used as an input.

## Models

| Model | Notebook | Input |
|---|---|---|
| XGBoost | `Flare_XGBoost_CleanData_Colab.ipynb` | Current hour + 24-hour trend features |
| LSTM | `Flare_LSTM_CleanData_Colab.ipynb` | Past 48 hours |
| DLSTM | `Flare_DLSTM_CleanData_Colab.ipynb` | Past 48 hours |
| 1D CNN | `Flare_1DCNN_CleanData_Colab.ipynb` | Past 24 hours |
| MiniRocket | `Flare_MiniRocket_CleanData_Colab.ipynb` | Past 24 hours |
| EBM | [notebook] | Current hour + 3- and 6-hour changes |
| Mamba | [notebook] | Past 48 hours |
| ViT / Swin | [notebook] | Magnetogram images (in progress) |

## How to run

1. Put `clean_flares_dataaset.csv` in Google Drive at `MyDrive/DS440/`. To keep it somewhere else, change `DRIVE_PATH` in the settings cell of each notebook.
2. Open a notebook in Colab with a **GPU** runtime and use **Run all**.
3. Predictions are saved to a `predictions/` folder next to the data. `clean_flares_with_predictions.csv` holds every model's predictions next to the true label.

## Reproducibility

- Seed 42 for all baselines.
- XGBoost and MiniRocket reproduce exactly. The neural networks can vary slightly between GPU runs, especially on the rare X class.
- All tuning used the validation set. The test set was used only for final scores.
- Full results are in the report.

## References

References for the baseline models and data are listed in the report.

## Acknowledgments

SHARP data courtesy of NASA/SDO and the HMI science team via JSOC. GOES flare data courtesy of NOAA NCEI. Parts of the code were written with the help of Claude (Anthropic), as allowed for this course.

## License

MIT. See `LICENSE`.
