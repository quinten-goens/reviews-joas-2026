# Typos
- Line 39: "performance review committee" -> "Performance Review Commission"
- Line 87: "Section ??" is missing the section number. 
- Lines 109-110: "which is also can be observed" -> "which can also be observed".
- Line 110: "form" -> "from".
- Lines 165-166: "(may change this number and figure)" seems to be a reminder for future updates.
- Line 304: "Automatic Dependent Surveillance-Broadcast" overlaps with the line number. 
- Line 314: "Section ??" is missing the section number. 
- Figure 1 caption: "for randomly sampled 1000 flights" -> "for 1000 randomly sampled flights". 
- Figure 2 caption: "fuel segment." -> "fuel segments.". 

# Problems
- Contrary to the authors' claim in lines 73 to 75, OpenAP's flight phase identification module is in fact aircraft-type agnostic: the underlying fuzzy logic uses universal membership functions for altitude, rate of climb, and speed that are explicitly designed to accommodate inter-type variability without per-type tuning [^sun2016], and the corresponding implementation instantiates `FlightPhase()` with no aircraft-type argument and hard-coded membership parameters [^openap-phase].

[^sun2016]: Sun, J., Ellerbroek, J., & Hoekstra, J. (2016). *Large-Scale Flight Phase Identification from ADS-B Data Using Machine Learning Methods*. 7th International Conference on Research in Air Transportation, Philadelphia, USA.

[^openap-phase]: TUDelft-CNS-ATM/openap, `openap/phase.py`. <https://github.com/TUDelft-CNS-ATM/openap/blob/master/openap/phase.py>

- The authors characterise both fuel flow extremes as physically unrealistic for commercial passenger aircraft in lines 124 to 128. We note that the source dataset is not mentioned to be restricted to commercial passenger operations,^[PRC Data Challenge 2025, Data documentation. <https://ansperformance.eu/study/data-challenge/dc2025/data.html>] which plausibly accounts for the very low fuel flow values observed.

- In chapter 4.2, the authors mention clipping Fuel-Flow above 6.5 kg/s and below 0.05 kg/s. It is explained that 0.05 kg/s corresponds to the minimum descent fuel flow of a small narrow-body aircraft and 6.5 kg/s corresponds to the assumed maximum climb fuel flow of a large wide-body aircraft. Although Section 3.1 cites Baklacioglu (0.04–1.84 kg/s for B738) and the ICAO Aircraft Engine Emissions Databank (0.023–4.69 kg/s per engine) as reference bounds, these references are not explicitly tied to the specific values of 0.05 kg/s and 6.5 kg/s chosen in Section 4.2. In particular, the ICAO bound is reported per engine, so its relation to the aggregate aircraft-level upper bound of 6.5 kg/s is not immediate. We suggest making the link between the cited references and the selected clipping bounds explicit.

- In line 109, it is mentioned that the duration of the fuel reporting windows varies considerably which can be observed from Table 1. When looking at Table 1, it is not clear how this can be observed from the table. It mentions the number of fuel segments, but not the variation in duration. 

- In equation 2: MLW and OEW are mentioned without introduction. Whilst there is an abbreviation table, it would be easier for the reader is the meaning is mentioned.  


# Questions for further research
- In section 4.2, the authors describe clipping implausible fuel flow values to the nearest plausible boundary. The stated causes; data artifacts, timestamp irregularities, and reporting errors; suggest these outliers may be effectively random. If so, clipping them to the boundary value introduces a systematic bias rather than preserving the underlying distribution. Further research could consider alternative imputation strategies, such as linear interpolation from neighbouring timestamps or substitution with a local mean, or, just excluding them as a whole, as potentially more appropriate approaches for improving model accuracy. 

- In section 4.3, the overview of Feature-Set Experiments, the authors mention that the fourth group (lines 321 - 325) - the Vrate Split - provides a higher-resolution representation of vertical dynamics within each segment. They highlight each segment is divided into 10 equal-duration sub intervals and that the mean and standard deviation of the vertical rate are computed for each sub-interval. The choice of dividing each segment by exactly 10 is not substantiated. What would the impact be on the model performance if a different number of sub-intervals were used? The importance of including the Vrate Split is underscored in lines 445 - 448, it would thus be interesting to see the  model performance for variations of this feature. 

# Comments
- The authors provide a good and complete description of the input data.
- The authors do a great job of explaining the feature engineering; the feature selection process is very well detailed and substantiated.
- Section 5.1 on the Fuel-Flow Filter Sensitivity is a good example of the substantiation of decisions made throughout the data cleaning process. Applying the proposed fuel-flow filter is shown to dramatically improve the model RMSE.
- Table 5, together with section 5 on the results and analysis, details to a large extent why certain features should be included and why others (e.g., wind) may not improve the model.
- The findings that the fuel-flow and associated emissions are heavily dependent on mass underscores the importance of the PRC 2024 data challenge and thus I commend the authors for the inclusion of the winning PRC 2024 model to estimate MTOW in their feature engineering pipeline. 
- It's noted to be initially surprising that Block 3 with Heuristic Mass performs better than Block 2 with Recursive Mass. The reason herefore is however well explained in lines 471-473: Errors accumulate in a recursive approach. 