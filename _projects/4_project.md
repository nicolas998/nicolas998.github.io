---
layout: page
title: Stream Quality Toy
description: An interactive web app to explore a physically based stream water temperature model
img: assets/img/temperature_snapshot.png
importance: 1
category: research
giscus_comments: true
---

[**Stream Quality Toy**](https://streamqualitytoy.streamlit.app/) is an interactive web app where you can explore how stream water temperature responds to meteorological forcing and channel conditions, directly from your browser and without installing anything.

The app is part of a research project in which we are developing a physically based water temperature model. The model solves the energy balance at the air-water interface, accounting for solar radiation, net longwave radiation, evaporative heat flux, and convective heat transfer. It is formulated as a system of ordinary differential equations (ODEs), which allows it to be executed with the ODE solver of the Hillslope Link Model (HLM), where it is implemented. This makes it possible to simulate water temperature at every hillslope and channel link of a river network, coupled with the discharge simulated by HLM.

This work builds on the Network Link-Oriented Streamflow Temperature model (NLOST); you can read more about it in the JAWRA [publication](https://onlinelibrary.wiley.com/doi/10.1111/1752-1688.70140).

You can access the app [here](https://streamqualitytoy.streamlit.app/).

{% include figure.html path="assets/img/temperature_snapshot.png" title="Example of simulated stream water temperature in the Cedar River at Waverly, Iowa" class="img-fluid rounded z-depth-1" %}
