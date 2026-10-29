---
layout: page
title: Composite Metrics and Multi-objective Calibration
description: Two interactive web apps to design performance metrics and calibrate MARRMoT models against them
importance: 3
category: research
giscus_comments: true
---

How good is a hydrological simulation? The answer depends on the metric you use to judge it. A metric that rewards timing, like NSE, can pick a simulation that misses the low flows, while a metric that looks only at the flow distribution can pick one with the peaks in the wrong place. This project has two interactive web apps, both free to use in your browser without installing anything, that let you **design your own performance metric** and then **calibrate hydrological models against it**:

1. [**Composite Performance Metric (CPM) Explorer**](https://compositeperformancemetric.streamlit.app/): build a performance index for any USGS gauge and see which simulation it picks as the best.
2. [**Multi-objective Calibration**](https://multiobjectivecalibration.streamlit.app/): calibrate any of the 46 [MARRMoT](https://github.com/wknoben/MARRMoT) conceptual models at any USGS watershed with NSGA-II, using up to three of your metrics as objectives.

The two apps work together. When you like an index in the first app, you download it as a JSON file and upload it to the second one.

## The composite performance metric

A CPM combines timing components (NSE, Pearson and Spearman correlations) and distribution components (the mean and standard-deviation ratios, and the simulated-to-observed ratios of the 5, 10, 25, 75, 90 and 95th flow percentiles) into one score:

$$
\mathrm{CPM} = 1 - \sqrt{\sum_i \left(\frac{w_i}{\max_j w_j}\,(x_i - 1)\right)^2}
$$

Every component $$x_i$$ is 1 for a perfect simulation, and you choose the weight $$w_i$$ of each one. The weights are relative, so equal weights on the Pearson correlation and the mean and deviation ratios give exactly the KGE. Each configuration gets an identifier (`CPM-XXXXXX`) computed from its weights, so the same weights always have the same identifier.

## App 1: CPM Explorer

For a USGS gauge, the app downloads the observed streamflow and the forcing data (AORC or NASA POWER precipitation, and potential evapotranspiration from NASA POWER), runs a set of random parameter realizations of a MARRMoT model, and scores all of them with up to five CPM configurations side by side. For the best simulation of each configuration, it shows the hydrograph, the relative error, the flow distribution (PDF), the flow duration curve, a Q-Q plot, and the model parameters.

### Step by step

1. **Open the app** at [compositeperformancemetric.streamlit.app](https://compositeperformancemetric.streamlit.app/). If the app has been idle, Streamlit may ask you to wake it up first; this takes a minute.
2. **Set up the experiment** in the sidebar:
   - Type the **USGS gauge** number (e.g. `02249500`).
   - Choose the **first and last years** of the period and the **warm-up years**, which are simulated but not scored.
   - Choose the **precipitation** source (AORC basin average or NASA POWER at the gauge) and the **potential ET** method.
   - Pick the **MARRMoT model** (CRB3, model 11, by default). You can also narrow its **parameter ranges**.
   - Set the number of **random realizations** and the **seed**.
3. **Press "Download data and run".** The app downloads the data and runs the ensemble. This can take a minute or two the first time for a gauge.
4. **Design your CPM configurations** in the "CPM configurations" section:
   - Start from a **preset** (Equal weights, KGE, Timing only, Distribution only) with **Add configuration**, or load a JSON file you saved before.
   - Move the **weight sliders** in each tab: timing/error, moments, low-flow percentiles and high-flow percentiles. Give the configuration a **name** and **notes**.
   - You can keep up to five configurations at the same time. Use **Duplicate** to try variations of one.
5. **Compare the results.** Each configuration picks its best simulation, drawn in its own color:
   - In the **hydrograph**, rank the ensemble by any configuration and show its top simulations in gray.
   - The **scores table** shows each best run's CPM, KGE, NSE, every component, its rank by KGE, and the distance between its flow PDF and the observed one.
   - The **relative error**, **flow distribution**, **flow duration curve** and **Q-Q plot** show where each best simulation over- or underestimates the flows.
6. **Download the configuration you like** with **Download JSON**. The file has the weights, your notes, and the run it was tested on (gauge, model, period, best realization and its parameters). This is the file you upload to the calibration app.

## App 2: Multi-objective Calibration

This app calibrates a MARRMoT model with the NSGA-II genetic algorithm, maximizing one to three CPMs at the same time. Since metrics usually disagree, there is no single best parameter set but a **Pareto front**: the simulations that no other simulation beats on every CPM at once. The app shows how the performance and the parameters change along that front, and how they change between models and between metrics.

### Step by step

1. **Open the app** at [multiobjectivecalibration.streamlit.app](https://multiobjectivecalibration.streamlit.app/).
2. **Choose the watershed and period** in the sidebar: the **USGS gauge**, the **first and last years**, the **spin-up** in days (simulated before January 1st of the first year, but not scored), the **precipitation** source and the **potential ET** method.
3. **Choose the model.** Pick any of the 46 MARRMoT models. Optionally, edit the **parameter search ranges** and how far each range can widen.
4. **Choose the objectives:**
   - **Upload** one or more CPM JSON files downloaded from the CPM Explorer. Uploaded CPMs are selected automatically.
   - Or pick from the **presets**. The default pair is *Timing only* vs. *Distribution only*.
   - You can select up to three CPMs; the weights of each one are listed below the selector.
5. **Set up NSGA-II** (the defaults are a good start):
   - **Calibrations per generation** (population size) and **generations**. The app shows the total number of simulations.
   - Crossover and mutation probabilities and distribution indexes, and the seed.
   - **Dynamic search ranges**: when on, a parameter's range widens if the front piles up against one of its bounds.
6. **Press "Calibrate".** A progress bar shows each generation and the best value of each CPM so far. Do not change any control while it runs, because that stops the calibration.
7. **Explore the results:**
   - **Objective space**: every simulation as a point, with the Pareto front of the CPMs. With three CPMs you also get the front of every pair, and a 3-D view.
   - **Highlight one front member** to follow that simulation through all the plots below.
   - **Hydrograph**, **flow duration curve** and **Q-Q plot** of the front members against the observations.
   - **Parameter distributions**: for each parameter, where the front members fall compared to all the simulations. Members piled against an edge of a range suggest widening it.
   - **Convergence**: the best value of each CPM per generation.
8. **Download the results** with **Download front members (CSV)**. The file has each front member's generation, its CPMs, KGE, NSE and parameters, so you can use it in later tests.
9. **Repeat** with another model, another set of CPMs, or another watershed, and compare how the fronts and parameters change.

## Tips

- Downloaded data is cached, so a second run on the same gauge starts much faster.
- The web apps run on shared servers. Keep the number of simulations moderate (a few thousand) or the runs will take a long time.
- Files saved on the server (the CPM library and the saved calibrations) can be lost when the app restarts. Always **download** the CPM JSON files and the front-member CSVs you want to keep.
