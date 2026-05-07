# Review Comments — Submission 8764

**Paper:** A Physics-Guided Gradient Boosting Framework for Open-Source Aviation Fuel Estimation

## Strengths
- The introduction and related work are well structured. Section 2 organises prior work around three macro-themes (physics-based modelling, data-driven prediction from proprietary data, and open-data approaches), and Section 2.5 makes the methodological gap explicit, motivating the contributions clearly.
- The synthetic widebody augmentation strategy (Section 3.3.6) is well motivated and addresses the narrowbody/widebody class imbalance directly, raising the widebody training share from 18.7% to 34.1%. The ablation in Tables 10 and 11 quantifies its effect cleanly, with overall RMSE reduced by 16.1% and the gains concentrated in widebody segments.


## Potential issues
- The source of the METAR data not cited in the methodology. METAR is introduced as a key input but its source (Iowa Environmental Mesonet [42]) is only cited in the Open Data Statement on page 20. We suggest citing it at first use in Section 3.2.1.
- OpenAP FuelFlow is being used both as a feature and as the baseline. The OpenAP fuel estimate is an input to XGBoost (Table 1) and also the physics baseline in Table 6. Beating OpenAP when the model already sees its prediction is expected, so the comparison overstates the gain over the physics baseline. Reporting Table 6 with the OpenAP feature removed, or isolating its contribution in the ablation, would clarify the actual improvement.

## Questions and suggestions for further research
- Quantify the contribution of the OpenAP feature. Adding a Table 11 row with the OpenAP feature group removed would isolate how much of the gain comes from the learned residual correction over the physics estimate.
- Narrowbody/widebody split as an intermediate model. Section 6 already proposes per-type models, but given that fuselage height alone accounts for 52.9% of the XGBoost gain (Section 4), a simpler narrowbody/widebody two-model split may capture much of that benefit at a fraction of the complexity. Reporting this configuration would clarify whether finer per-type stratification is worth the added cost.

## Typographical and editorial remarks

### Inconsistent terminology / capitalization
- "Wing Mac" → "Wing MAC"
- "narrowbody/widebody" vs "narrow-body/wide-body"
- "Modeling" vs "Modelling"
- "high-fidelity" vs "high fidelity"
- "hyper-parameter(s)" vs "hyperparameter(s)"
- "takeoff" vs "take-off" vs "take off"
