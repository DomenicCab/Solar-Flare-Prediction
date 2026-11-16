# Solar Flare Class Prediction from SHARP and GOES Data

DS440 Capstone · Domenic Cabibbo, Tjoet Lakmana, Ayanna Norfleet, Darshan Sivarajah, Nur Sakinah Binti Mohammad Zaini

We predict the **largest solar flare an active region will produce in the next 24 hours** (none, C, M or X) from hourly magnetic field measurements, comparing five baseline models from prior studies with four new models.

**Report:** [link] (Still being worked on)

## Data

Our dataset combines two free public sources:

- **SHARP magnetic field data (inputs):** hourly magnetic measurements of each solar active region from NASA's Solar Dynamics Observatory, series `hmi.sharp_720s`, from Stanford's [JSOC](http://jsoc.stanford.edu) (free [registration](http://jsoc.stanford.edu/ajax/register_email.html) required), downloaded with the [`drms`](https://docs.sunpy.org/projects/drms/) Python package.
- **GOES X-ray flare catalog (labels):** NOAA's list of every solar flare with its class, times and active region. We used the XRS Flare Report dating 2010-2026 from this NCEI directory: <https://data.ngdc.noaa.gov/platforms/solar-space-observing-satellites/goes/multi/l2/data/xrsf-l2-flrpt_science/>

Flares were matched to SHARP regions by NOAA region number, using JSOC's [HARP-to-NOAA table](http://jsoc.stanford.edu/doc/data/hmi/harpnum_to_noaa/all_harps_with_noaa_ars.txt). Each hour was labeled with the largest flare the region produced in the next 24 hours: none, C (including A and C class flares), M or X. Gaps of up to 3 hours were forward-filled. Data was split by region in time order: train 2010–2021, validation 2021–2023, test 2023–2026.

**Final dataset:** `flares_dataset.csv` ([https://drive.google.com/file/d/1ChTuN72NsGU-3hWdJ1gqfW-UhLBS-95U/view?usp=sharing](https://drive.google.com/file/d/1RGRjS7M97Uedr1gxe5Ksk4278T2MyxTZ/view?usp=sharing))

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

1. Put `flares_dataset.csv` in Google Drive. Change `DRIVE_PATH` in the settings cell of each notebook to where you have the dataset.
2. Open a notebook in Colab and run.

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
