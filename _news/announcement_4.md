---
layout: post
title: The Network Link-Oriented Streamflow Temperature model (NLOST)
date: 2024-03-25 15:59:00-0400
inline: false
related_posts: false
---

**The Network Link-Oriented Streamflow Temperature model (NLOST)**

Water temperature is a crucial water quality variable controlling the concentration of dissolved oxygen and nutrients. Temperature also influences the productivity and distribution of aquatic ecosystems. Due to climate change, we have seen a rise in the temperature of different environments, which we expect to continue. The described crisis, combined with increased nutrients and nitrate loads, will affect our water quality and ecosystems in ways we still don't understand. Therefore, we require physically based models representing channel network interactions to analyze our streams' current and future environmental quality. In the present work, we present the development and validation of the Network Link-Oriented Streamflow Temperature model (NLOST). We developed NLOST to solve the energy balance equations at each hillslope and channel of the watershed at the air-water interface. NLOST considers solar radiation, net longwave radiation, evaporative heat flux, and convective heat transfer. We formulated NLOST for a general river network using the Ordinary differential equations (ODE) solver previously developed for the Hillslope Link Model (HLM). To validate NLOST, we ran it using the High-Resolution Rapid Refresh (HRRR) meteorological data and simulated discharge records taken from HLM. We tested and validated NLOST using records from the USGS gauges in Cedar River at Waverly between 2015 and 2022. Our preliminary model results show potential for further evaluation, development, and water quality-related applications. With NLOST, we expect to contribute to our understanding of the current and future environmental challenges, helping us to make decisions that will take us to a not-so-lost future.  

{% include figure.html path="assets/img/publication_preview/temperature_model.gif" title="Example result of NLOST for Waverly, Iowa" class="img-fluid rounded z-depth-1" %}

This work is the results of the Master thesis developed by **Valeria Garcia** and directed by myself along with the Professor **Witold Krajewski**.

