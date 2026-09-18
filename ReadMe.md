# Bias-Corrected CMIP6 Climate Data Processing for SWAT+ Modeling

**Author:** Imran Ul Haq
**Climate Analyst — Weather and Climate Services, Islamabad, Pakistan**

---

## 🌍 Overview

This repository provides a reproducible workflow for **downloading, preprocessing, bias-correcting, and analyzing CMIP6 climate projections for hydrological modeling using SWAT+**.

The workflow connects global climate model (GCM) projections with hydrological modeling by transforming raw CMIP6 climate data into standardized, bias-corrected, and SWAT+-compatible climate inputs.

The overall framework consists of:

```text
CMIP6 Climate Data
        │
        ▼
Download & Spatial Clipping
        │
        ▼
Preprocessing & Quality Control
        │
        ▼
Unit / Time Standardization
        │
        ▼
Quantile Mapping Bias Correction
        │
        ▼
SWAT+ Climate Inputs
        │
        ▼
Hydrological Simulation
        │
        ▼
Extreme Event Analysis
        │
        ▼
Future Climate & Hydrological Risk Assessment
```

The workflow is designed to support research on **climate change impacts on hydrology, extreme precipitation, water resources, flood risk, and future streamflow conditions**.

---

# 🎯 Research Objectives

The workflow is designed to address several research objectives:

* Process high-resolution CMIP6 climate projections for hydrological applications.
* Reduce systematic biases in climate-model precipitation using observational data.
* Generate climate inputs compatible with SWAT+.
* Investigate the influence of future climate scenarios on hydrological processes.
* Analyze changes in extreme precipitation and streamflow.
* Estimate return periods of extreme hydrological events.
* Quantify uncertainty associated with extreme-value estimation.
* Compare historical and future climate conditions.
* Support climate-impact and water-resource assessments.
* Provide a reproducible framework for climate-to-hydrology research.

---

# 📚 Scientific Background

Global Climate Models (GCMs) provide projections of future climate under different greenhouse-gas emission pathways. However, raw GCM outputs may contain systematic biases in variables such as precipitation and temperature.

These biases can affect downstream hydrological simulations.

Therefore, a typical climate-impact workflow consists of:

```text
GCM Simulation
      │
      ▼
Bias Assessment
      │
      ▼
Bias Correction
      │
      ▼
Hydrological Model
      │
      ▼
Streamflow Simulation
      │
      ▼
Extreme Event Analysis
      │
      ▼
Climate Risk Assessment
```

In this project, **Quantile Mapping (QM)** is used to adjust the statistical distribution of model precipitation using observational precipitation data.

The resulting bias-corrected climate data can then be used as input to SWAT+.

---

# 🧩 Workflow Overview

The project is organized into five main Jupyter notebooks.

| No. | Notebook                       | Main Function                                    |
| --- | ------------------------------ | ------------------------------------------------ |
| 1   | `Download_clip_CMIP6.ipynb`    | Download and spatially clip CMIP6 data           |
| 2   | `Preprocessing.ipynb`          | Standardize climate data                         |
| 3   | `Quantile_mapping.ipynb`       | Perform Quantile Mapping bias correction         |
| 4   | `SWAT_inputs_Format.ipynb`     | Generate SWAT+ climate inputs                    |
| 5   | `Extreme_event_analysis.ipynb` | Analyze hydrological extremes and return periods |

---

# 1️⃣ Download and Clip CMIP6 Data

### Notebook

```text
Download_clip_CMIP6.ipynb
```

### Purpose

Downloads daily CMIP6 climate data and spatially clips the datasets to the study region using a geographic boundary/shapefile.

The workflow is designed for climate variables required by hydrological modeling.

### Supported Variables

Examples include:

* Precipitation
* Maximum temperature
* Minimum temperature
* Relative humidity
* Wind speed
* Solar radiation

### Climate Scenarios

The workflow can be adapted for different CMIP6 Shared Socioeconomic Pathway (SSP) scenarios, including:

* `ssp245`
* `ssp370`
* `ssp585`

Additional scenarios can be incorporated depending on dataset availability.

### Main Processing Steps

```text
CMIP6 Dataset
      │
      ▼
Select Model
      │
      ▼
Select Scenario
      │
      ▼
Select Variable
      │
      ▼
Download Daily Data
      │
      ▼
Apply Spatial Mask
      │
      ▼
Save Clipped Dataset
```

### Output

```text
workingfolder/
└── clipped_data/
    ├── model_1/
    ├── model_2/
    └── ...
```

---

# 2️⃣ Preprocessing and Standardization

### Notebook

```text
Preprocessing.ipynb
```

### Purpose

The preprocessing stage ensures that CMIP6 datasets have consistent temporal formats, coordinate systems, variables, and physical units before bias correction.

### Main Processing Tasks

* Validate NetCDF structure.
* Standardize time coordinates.
* Check latitude and longitude.
* Handle missing values.
* Check variable names.
* Check metadata.
* Convert climate variables to appropriate units.
* Prepare datasets for statistical bias correction.

### Unit Conversion

Typical conversions include:

| Variable          | Original Unit | Processing / SWAT Unit           |
| ----------------- | ------------- | -------------------------------- |
| Precipitation     | kg m⁻² s⁻¹    | mm/day                           |
| Temperature       | K             | °C                               |
| Relative Humidity | %             | fraction / model-specific format |
| Solar Radiation   | W/m²          | MJ/m²/day                        |
| Wind Speed        | m/s           | m/s                              |

### Output

```text
workingfolder/
└── standardized_data/
```

---

# 3️⃣ Bias Correction Using Quantile Mapping

### Notebook

```text
Quantile_mapping.ipynb
```

### Purpose

This notebook applies **Quantile Mapping (QM)** to correct systematic biases in CMIP6 precipitation using observed precipitation data.

For the example workflow:

```text
Observed Dataset
      │
      │
      ▼
   CHIRPS
      │
      │
      ├───────────────┐
      │               │
      ▼               ▼
Historical CMIP6   Observations
      │               │
      └───────┬───────┘
              ▼
       Quantile Mapping
              │
              ▼
     Bias-Correction Function
              │
              ▼
      Future CMIP6 Data
              │
              ▼
      Corrected Projection
```

### Reference Period

The example workflow uses:

```text
Historical calibration period:
2015–2024
```

and applies the derived correction to future projections:

```text
2025–2100
```

These periods can be modified according to the study design and availability of observations.

### Why Quantile Mapping?

Quantile Mapping attempts to correct differences between the statistical distributions of observed and simulated climate variables.

It can adjust:

* Mean bias
* Variance
* Distributional differences
* Percentile-dependent biases

### Important Consideration

Bias correction does not automatically make climate projections "observed truth."

The method depends on:

* Reference observations
* Calibration period
* Statistical assumptions
* Model representation
* Stationarity assumptions
* Spatial resolution
* Extrapolation behavior

Therefore, bias-corrected results should be evaluated against independent observations whenever possible.

### Output

```text
workingfolder/
└── bias_corrected/
    ├── model_1/
    ├── model_2/
    └── ...
```

---

# 4️⃣ SWAT+ Climate Input Formatting

### Notebook

```text
SWAT_inputs_Format.ipynb
```

### Purpose

Converts bias-corrected climate data into climate input files suitable for **SWAT+ hydrological simulations**.

Climate information is extracted for SWAT+ weather stations using station/centroid locations.

### Typical Inputs

The workflow can generate SWAT-compatible files for parameters such as:

* Precipitation
* Temperature
* Relative humidity
* Wind speed
* Solar radiation

### Processing

```text
Bias-Corrected NetCDF
          │
          ▼
     Station Locations
          │
          ▼
   Spatial Extraction
          │
          ▼
   Daily Time Series
          │
          ▼
 SWAT+ Input Formatting
          │
          ▼
   SWAT+ Climate Files
```

### Output

```text
workingfolder/
└── SWAT_INPUT/
    ├── pcp.txt
    ├── tmp.txt
    ├── rh.txt
    ├── wnd.txt
    └── slr.txt
```

The exact files and naming conventions depend on the SWAT+ project configuration.

---

# 5️⃣ Extreme Event Analysis

### Notebook

```text
Extreme_event_analysis.ipynb
```

### Purpose

This notebook analyzes extreme hydrological events derived from SWAT+ simulations and estimates their statistical return periods.

The workflow can be applied to **Annual Maximum Series (AMS)** extracted from simulated streamflow.

### Analysis Workflow

```text
SWAT+ Streamflow
       │
       ▼
Annual Maximum Series
       │
       ▼
Extreme Value Analysis
       │
       ├───────────────┐
       ▼               ▼
      GEV           Gumbel
       │               │
       └───────┬───────┘
               ▼
        Return Periods
               │
               ▼
       Uncertainty Analysis
               │
               ▼
        Historical vs Future
```

### Probability Distributions

The workflow can evaluate extreme-value distributions such as:

* Generalized Extreme Value (GEV)
* Gumbel
* Pearson Type III
* L-Moments based approaches

### Return Periods

Return-period analysis can be used to estimate the magnitude of events associated with recurrence intervals such as:

* 2 years
* 5 years
* 10 years
* 25 years
* 50 years
* 100 years

Return periods should be interpreted as statistical characteristics of the analyzed distribution, not as guarantees that an event will occur exactly once during that interval.

### Bootstrap Uncertainty

Bootstrap resampling can be used to estimate uncertainty around fitted extreme-value statistics.

Example:

```text
Observed / Simulated AMS
          │
          ▼
Bootstrap Resampling
          │
     ┌────┼────┐
     ▼    ▼    ▼
Sample Sample Sample ...
     │    │    │
     └────┼────┘
          ▼
Distribution Fitting
          │
          ▼
Confidence / Uncertainty Intervals
```

The example implementation uses:

```text
1000 bootstrap iterations
```

which can be adjusted according to computational requirements.

### Output

```text
workingfolder/
└── Results_Plots/
    ├── return_periods/
    ├── distributions/
    ├── uncertainty/
    └── figures/
```

---

# 🔬 Research Applications

This workflow can support several areas of climate-impact and hydrological research.

## 1. Climate Change Impact on Water Resources

Evaluate how projected changes in precipitation and temperature may influence:

* River discharge
* Water availability
* Seasonal runoff
* Basin hydrology
* Water-resource planning

---

## 2. Flood Risk Assessment

Extreme streamflow analysis can be used to investigate potential changes in:

* Flood magnitude
* Flood frequency
* Return-period discharge
* Extreme precipitation
* Hydrological risk

---

## 3. Drought Assessment

The framework can be extended to analyze:

* Precipitation deficits
* Low-flow conditions
* Drought duration
* Drought frequency
* Hydrological drought

---

## 4. Climate Extremes

The CMIP6 datasets can be analyzed for changes in:

* Extreme precipitation
* Heavy rainfall events
* Maximum temperature
* Minimum temperature
* Heat extremes
* Dry spells
* Wet spells

Climate-extreme indices can be incorporated into future versions of the workflow.

---

## 5. Multi-Model Climate Uncertainty

Different CMIP6 models can produce different projections.

A multi-model framework can therefore be used to investigate:

```text
CMIP6 Model 1 ─┐
CMIP6 Model 2 ─┤
CMIP6 Model 3 ─┤
CMIP6 Model 4 ─┼──► Multi-Model Analysis
CMIP6 Model 5 ─┤
CMIP6 Model 6 ─┘
```

This allows researchers to examine:

* Model spread
* Ensemble mean
* Projection ranges
* Inter-model variability
* Scenario uncertainty

---

## 6. Climate Risk and Adaptation

The resulting climate and hydrological projections can contribute to assessments of:

* Flood risk
* Water-security risk
* Infrastructure vulnerability
* Agricultural water demand
* Reservoir management
* Watershed planning
* Climate adaptation strategies

---

# 📈 Recommended Research Evaluation

For scientific applications, model outputs should be evaluated before and after bias correction.

Useful performance metrics include:

### Mean Bias

```text
Bias = Mean(Model) − Mean(Observation)
```

### Root Mean Square Error

```text
RMSE = √[mean(Model − Observation)²]
```

### Mean Absolute Error

```text
MAE = mean(|Model − Observation|)
```

### Correlation

Evaluate temporal agreement between modeled and observed climate variables.

### Distribution Comparison

Compare:

* Mean
* Variance
* Quantiles
* CDFs
* Probability density
* Extreme percentiles

A useful validation workflow is:

```text
Raw CMIP6
    │
    ├──► Historical Observation
    │
    ▼
Performance Assessment
    │
    ▼
Quantile Mapping
    │
    ▼
Bias-Corrected CMIP6
    │
    ▼
Performance Assessment
    │
    ▼
Hydrological Modeling
```

---

# ⚠️ Important Scientific Considerations

## Bias Correction

Bias correction should be treated as a statistical transformation rather than a physical correction of the climate model.

The resulting projections remain dependent on the underlying climate model.

---

## Reference Dataset

The quality of bias correction depends strongly on the quality, spatial resolution, temporal resolution, and characteristics of the observational dataset.

In this workflow, **CHIRPS precipitation** is used as an observational reference for the precipitation bias-correction example.

---

## Non-Stationarity

Many bias-correction methods rely on assumptions about the relationship between the model and observational distributions.

Researchers should carefully consider whether the correction remains appropriate under future climate conditions.

---

## Extreme Events

Extreme precipitation and streamflow are particularly sensitive to:

* Distribution choice
* Sample size
* Temporal aggregation
* Spatial resolution
* Bias-correction method
* Hydrological-model structure
* Climate-model uncertainty

Therefore, extreme-event results should be interpreted together with uncertainty estimates.

---

# 🛠️ Technologies

The workflow uses the following scientific Python ecosystem:

* Python
* Jupyter Notebook
* Xarray
* NumPy
* Pandas
* SciPy
* Matplotlib
* GeoPandas
* Rioxarray
* NetCDF4
* R
* rpy2
* L-Moments

---

# 📦 Dependencies

Install the main Python dependencies with:

```bash
pip install xarray numpy pandas matplotlib scipy rioxarray geopandas netCDF4
```

If a `requirements.txt` file is provided:

```bash
pip install -r requirements.txt
```

---

# 🔧 R and rpy2 Setup for L-Moments Analysis

Some extreme-value analyses may use R packages through `rpy2`.

## Requirements

* R 4.4.1 or compatible version
* Python
* `rpy2`
* R package `lmom`

### Install rpy2

```bash
pip install --no-cache-dir rpy2
```

### Configure R

Example:

```python
import os

os.environ["R_HOME"] = "C:/Program Files/R/R-4.4.1"
os.environ["R_USER"] = "C:/Users/YourUsername/Documents/R/win-library/4.4"
```

Modify these paths according to the local R installation.

### Install `lmom`

Open R and run:

```r
install.packages("lmom", dependencies = TRUE)
```

Verify installation:

```r
installed.packages()["lmom", ]
```

---

# 🚀 How to Run the Workflow

## Step 1 — Clone the Repository

```bash
git clone https://github.com/heshamgeo/CMIP6-BiasCorrection-SWAT.git
```

Then enter the repository:

```bash
cd CMIP6-BiasCorrection-SWAT
```

> Update the repository URL and directory name if this project is hosted under a different GitHub account or repository name.

---

## Step 2 — Install Dependencies

```bash
pip install -r requirements.txt
```

Alternatively:

```bash
pip install xarray numpy pandas matplotlib scipy rioxarray geopandas netCDF4
```

---

## Step 3 — Run the Notebooks Sequentially

Run the notebooks in the following order:

### 01. Download and Clip

```text
Download_clip_CMIP6.ipynb
```

↓

### 02. Preprocessing

```text
Preprocessing.ipynb
```

↓

### 03. Quantile Mapping

```text
Quantile_mapping.ipynb
```

↓

### 04. SWAT+ Input Formatting

```text
SWAT_inputs_Format.ipynb
```

↓

### 05. Extreme Event Analysis

```text
Extreme_event_analysis.ipynb
```

---

# 📁 Example Directory Structure

```text
CMIP6-BiasCorrection-SWAT/
│
├── Download_clip_CMIP6.ipynb
├── Preprocessing.ipynb
├── Quantile_mapping.ipynb
├── SWAT_inputs_Format.ipynb
├── Extreme_event_analysis.ipynb
│
├── workingfolder/
│   │
│   ├── clipped_data/
│   │
│   ├── standardized_data/
│   │
│   ├── bias_corrected/
│   │
│   ├── SWAT_INPUT/
│   │
│   └── Results_Plots/
│
├── requirements.txt
│
└── README.md
```

Large climate datasets should generally **not be committed directly to GitHub**. Store data externally and provide instructions for obtaining the datasets.

---

# 🌎 CMIP6 Data

The workflow is designed around CMIP6 climate projections.

Depending on the implementation, climate data can be obtained from publicly available CMIP6 repositories such as:

* NASA NEX-GDDP-CMIP6
* ESGF
* Other authorized CMIP6 data providers

Researchers should record:

* GCM name
* Ensemble member
* SSP scenario
* Variable
* Temporal resolution
* Spatial resolution
* Version
* Download source

This information is essential for reproducibility.

---

# 🧪 Reproducibility Checklist

For every research application, document:

* [ ] CMIP6 model
* [ ] Ensemble member
* [ ] SSP scenario
* [ ] Climate variables
* [ ] Historical period
* [ ] Future period
* [ ] Observation/reference dataset
* [ ] Bias-correction method
* [ ] Calibration period
* [ ] Validation period
* [ ] Spatial resolution
* [ ] Temporal resolution
* [ ] SWAT+ version
* [ ] SWAT+ calibration procedure
* [ ] Extreme-value distribution
* [ ] Return-period calculation
* [ ] Bootstrap configuration
* [ ] Python version
* [ ] R version
* [ ] Package versions

---

# 📊 Expected Research Outputs

The complete workflow can produce:

### Climate Outputs

* Bias-corrected precipitation
* Bias-corrected temperature
* Other SWAT-required climate variables
* Historical climate statistics
* Future climate projections

### Hydrological Outputs

* Simulated streamflow
* Annual maximum streamflow
* Seasonal discharge
* Extreme-flow statistics
* Future hydrological projections

### Statistical Outputs

* Return-period curves
* Extreme-value distributions
* Confidence intervals
* Bootstrap uncertainty estimates
* Historical vs future comparisons

### Visualization Outputs

* Climate time series
* Probability distributions
* CDF comparisons
* Spatial maps
* Return-period plots
* Historical/future comparison plots

---

# 🔭 Future Development

Potential extensions of this project include:

* Multiple bias-correction methods
* Quantile Delta Mapping
* Empirical Quantile Mapping
* Distribution Mapping
* Trend-preserving bias correction
* Bias correction of additional climate variables
* Multi-model ensemble processing
* Climate-extreme indices
* SPEI/SPI drought analysis
* Flood-frequency analysis
* Low-flow analysis
* Spatial uncertainty analysis
* Scenario comparison
* SWAT+ calibration and validation automation
* Climate-impact attribution
* Automated research-quality reporting

---

# 📖 Citation and Attribution

If you use this workflow in research, please cite the original datasets, models, methods, and software used in your analysis.

At minimum, document the specific:

* CMIP6 dataset
* GCM(s)
* SSP scenario(s)
* CHIRPS or other observational dataset
* SWAT+ model
* Bias-correction methodology
* Statistical distribution methodology

Please consult the original data providers for the appropriate citation requirements.

---

# 👨‍💻 Author

**Imran Ul Haq**

Climate Analyst
**Weather and Climate Services (WCS)**
Islamabad, Pakistan

Research interests include:

* Climate Data Analysis
* Climate Change Impact Assessment
* Climate Modeling
* Bias Correction
* CMIP6 Climate Projections
* Climate Extremes
* Hydrological Modeling
* Climate Risk Assessment
* Statistical Analysis
* Multi-Model Ensemble Analysis
* Climate Adaptation

---

# 🤝 Contributions

Contributions and suggestions are welcome.

If you identify an issue, methodological improvement, or useful extension:

1. Open an issue.
2. Describe the problem or proposed improvement.
3. Include relevant scientific references where appropriate.
4. Submit a pull request for code or documentation changes.

---

# 📜 License

This repository should be used in accordance with the license specified by the repository owner.

Datasets and third-party software retain their original licenses and attribution requirements.

Users are responsible for checking the licensing conditions of:

* CMIP6 datasets
* CHIRPS
* SWAT+
* R packages
* Python packages
* Other third-party resources

---

# 🙏 Acknowledgements

This work builds upon the scientific contributions of the climate-modeling, hydrological-modeling, and open-source scientific-computing communities.

Particular acknowledgement is given to the developers and data providers of:

* CMIP6
* NASA NEX-GDDP-CMIP6
* CHIRPS
* SWAT+
* Python scientific ecosystem
* R statistical ecosystem

---

## ⭐ Project Summary

This repository provides an end-to-end framework for transforming **CMIP6 climate projections into bias-corrected climate inputs for SWAT+**, followed by **hydrological extreme-event and return-period analysis**.

```text
CMIP6
  ↓
Download
  ↓
Spatial Clipping
  ↓
Preprocessing
  ↓
Quantile Mapping
  ↓
Bias-Corrected Climate Data
  ↓
SWAT+ Climate Inputs
  ↓
Hydrological Simulation
  ↓
Annual Maximum Series
  ↓
Extreme Value Analysis
  ↓
Return Periods
  ↓
Uncertainty Assessment
  ↓
Climate & Hydrological Risk Research
```

**Author:** Imran Ul Haq
**Position:** Climate Analyst, Weather and Climate Services, Islamabad
**Repository:** CMIP6 Bias Correction and Climate Impact Analysis
