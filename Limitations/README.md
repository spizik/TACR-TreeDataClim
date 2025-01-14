# Modified version of the VS-Lite process-based model of wood formation

The approach presented here is largely based on the VS-Lite model originally published by [Dr. Tolwinski-Ward](https://www.suztw.com/). The VS-Lite is a simple process-based model of wood formation assuming that the intra-annual radial growth rate is non-linearly dependent on temperature, soil moisture, and photoperiod. In the framework of the TreeDataClim project, we used the model to calculate the relative growth rate and the intensity of climatic limitation of wood formation at each site on a monthly scale.

### Principles of the VS-Lite model
A detailed description of the VS-Lite concepts and workflow is provided in a publication by Tolwinski-Ward et al. (2011). The The VS-Lite forward model simulates a radial growth of trees by approximating the non-linear response of growth based to monthly temperature (GrT) and moisture availability (GrM). Dimensionless partial growth responses to temperature and moisture are represented by ramp functions. Next, the integral growth rate (GrINT) is calculated for each month, which equals the lower of GrT and GrM, weighted by the ratio of monthly mean day length to day length of the summer solstice (GrE). GrINT represents a proxy for a growth rate during a given month.

### Modifications to the original version of the VS-Lite model
To improve the applicability of the VS-Lite model in cold and wet environments, which was shown insufficient (Breitenmoser et al. 2014), we modified the response functions of the model to better capture the reality in extremely cold and extremely warm environments. Specifically, we incorporated an assumption of the limitation of radial growth by too high soil moisture and too high temperature. The key part of the VS-Lite algorithm is the pair of non-linear equations converting monthly temperatures (T) and soil moisture (M) into partial growth rates to temperature (GrT) and partial growth rates to soil moisture (GrM). In the original version of the model, they have a ramp form. Indeed, partial growth rate to temperature is defined as:

![obrazek](https://user-images.githubusercontent.com/25429975/235633230-8a457e73-59ab-4061-983d-ac4bd05d13b0.png)

where T1 represents a parameter of threshold temperature below which cambial activity cannot be sustained, and T2 represents the lower margin of temperature optimum, above which growth decouples from the temperature. The analogous equation is used to calculate GrM. This simplified approach might not be appropriate at sites and regions where the cambial activity might be limited by very high temperatures or by soil water oversaturation. Indeed, we modified the original equations used for the calculation of partial growth rates by introducing parameters of the upper margin of optimal conditions (M3 and T3) and upper thresholds above which the cambial activity cannot be sustained (M4 and T4). The modified equation for the partial growth rate to temperature has a form:

![obrazek](https://user-images.githubusercontent.com/25429975/235633135-add0da4c-e62b-45d6-a990-f2f7d88dcd0c.png)

Therefore, the response of partial growth rates to climatic variability has a single optimum (T2-T3), two intervals of linear response of growth rate to the climatic variable (T1-T2 & T3-T4), and two intervals of conditions not permitting radial growth (<T1 & >T4):

![obrazek](https://user-images.githubusercontent.com/25429975/235648004-205298de-78ba-4be6-9074-69b1f53a3fbb.png)

The analogous form of the equation is used to calculate partial growth rates to soil moisture. Depending on specific values of GrT and GrM, the dominant climatic limiting effect can be identified for each month of the simulation period. The approach used for an assessment of the intensity of various types of climate-growth limitation for annual wood formation at a given site follows Tumajer et al. (2017). Specifically, we considered all radial growth occuring under conditions of GrT<GrM as limited by (high or low) temperature, while all growth during months with GrT>GrM was considered as limited by (high, low) soil moisture. We distinguished specific form of the limitation based on an instant value of temperature and soil moisture in given month:

![obrazek](https://user-images.githubusercontent.com/25429975/235668311-35a491be-08f8-4ca7-82ef-6cd822200320.png)

### Applicability
The scripts can be run using Matlab or Octave (recommended). As an input, the scripts require site chronologies, climatic data (monthly mean air temperature and precipitation totals), and site coordinates. However, these input files for Octave have to have a different format compared to R scripts. Therefore, the template for input files to be used with the VS-Lite is stored in a specific subfolder of the `Input` folder.

All three scripts should be downloaded and **stored in the same working folder**. To calibrate the VS-Lite model, run the `Skript_RUN.m` function in Octave. The function will automatically load the `estimate_randomization_decline.m` script to generate a large number of random combinations of the model parameters. Next, the script will store the set of parameters resulting in the highest correlation between the simulated and observed chronologies. With this set of parameters, the full model is automatically calibrated and various types of results are stored using `VSLite_decline.m`.

### References
*Breitenmoser, P., Brönnimann, S., Frank, D., 2014. Forward modelling of tree-ring width and comparison with a global network of tree-ring chronologies. Climate of the Past 10, 437–449. https://doi.org/10.5194/cp-10-437-2014*

*Tolwinski-Ward, S.E., Evans, M.N., Hughes, M.K., Anchukaitis, K.J., 2011. An efficient forward model of the climate controls on interannual variation in tree-ring width. Climate Dynamics 36, 2419–2439. https://doi.org/10.1007/s00382-010-0945-5*

*Tumajer, J., Altman, J., Štěpánek, P., Treml, V., Doležal, J., Cienciala, E., 2017. Increasing moisture limitation of Norway spruce in Central Europe revealed by forward modelling of tree growth in tree-ring network. Agricultural and Forest Meteorology 247, 56–64. https://doi.org/10.1016/j.agrformet.2017.07.015*
