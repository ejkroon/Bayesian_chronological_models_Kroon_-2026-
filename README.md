# Bayesian chronological models in Kroon (2026)
This repository contains the raw data, OxCal models, and outputs for 
the Bayesian chronological models in Kroon (in prep.). New Approach 
to Prehistoric Migrations: Bayesian Chronological Modelling and Ceramic 
Technology Shed New Light on the Emergence of Corded Ware. Proceedings 
of the Prehistoric Society.

## Structure of the dataset
The folder structure for the dataset is shown below. The radiocarbon 
dates and sources for all models can be found under `raw_data`. 

```
__OxCal_models
 |___Dutch chronology -> (Kroon in prep., Fig. 2)
 |  |_ bayesian_chronological_model.txt
 |  |_ order.csv
 |  |_ outliers.csv
 |  |_ output.csv
 |  |_ visual_output.pdf
 |___European chronology -> (Kroon in prep., Fig. 8)
 |   |_GAC_CW
 |   |_GAE_CW
 |   |_GAW_CW
 |   |_PW_CW
 |___raw_data
    |_radiocarbon_dates.csv
    |_bibliography_C14s
```

The Bayesian chronological models and their output are ordered by the 
figure in the main text in which they appear. The ordering of the 
files under `Dutch_chronology` (see above) is identical to that of the 
files in the subfolders of `European_chronology`.

These files contain the following information:
- [name]_bayesian_chronological_models.txt: the model in OxCal CQL as 
  run in OxCal v.4.4.4 (Bronk Ramsey 2021).
- [name]_output.csv: the output of the Bayesian chronological model.
- [name]_order.csv: the raw output of the Order() queries.
- [name]_outliers.csv: the Outlier view of the model output.
- [name]_visual_output.pdf: a visual rendering of the model output. 
  These visualisations show the agreement (Amodel) and comvergence (C) 
  of all models. The brackets and OxCal verbs show the model definition.
  The average 2σ and 3σ confidence intervals are shown for all
  distributions. Solid circles indicate the means of posterior 
  distributions, transparent circles the means of the radiocarbon dates 
  prior to modelling. The scale at the bottom of the model is in 
  modelled years BC. 

## General model
```C++
 Plot()
 {
  // Outlier models
  Outlier_Model("General", T(5), U(0,4), "t");
  Outlier_Model("Charcoal", Exp(1, -10,0), U(0,3), "t");
  Outlier_Model("Cremation", Exp(0.9,-10,-0.1), U(1,3),"t");
  // Model with independent sequences for CW and PW
  Sequence("CW")
  {
   Boundary("Start_CW")
   {
    Start("Start_CW_S");
    Transition("Start_CW_T");
    End("Start_CW_E");
   };
   Phase("CW radiocarbon dates")
   {
    KDE_Plot("CW")
    {
    };
    Sum()
    {
    };
    //R-dates and appropriate outlier models
    };
   Boundary("End_CW")
   {
    Start("End_CW_S");
    Transition("End_CW_T");
    End("End_CW_E");
   };
  };
  Sequence("PW")
  {
   Boundary("Start_PW")
   {
    Start("Start_PW_S");
    Transition("Start_PW_T");
    End("Start_PW_E");
   };
   Phase("PW radiocarbon dates")
   {
    KDE_Plot("PW")
    {
    };
    Sum()
    {
    };
    //R-dates and appropriate outlier models
    };
   Boundary("End_PW")
   {
    Start("End_PW_S");
    Transition("End_PW_T");
    End("End_PW_E");
   };
  };
  Order()
  {
  };
  Difference("Overlap PW_CW", "Start_CW", "End_PW");
  Difference("Max overlap PW_CW", "Start_CW_E", "End_PW_S");
  Difference("Min overlap PW_CW", "Start_CW_S", "End_PW_E");
 };
```

## How to cite
Please cite the main publication: Kroon, E.J., (in prep.). New Approach 
to Prehistoric Migrations: Bayesian Chronological Modelling and Ceramic 
Technology Shed New Light on the Emergence of Corded Ware. Proceedings 
of the Prehistoric Society.

For the radiocarbon dates, please refer to the sources mentioned in 
`raw_data/radiocarbon_dates.csv`.

Bibliography
Bronk Ramsey 2021

