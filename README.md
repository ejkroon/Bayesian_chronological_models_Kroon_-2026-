# Bayesian chronological models in Kroon (2026)
This repository contains the raw data, OxCal models, and outputs for 
the Bayesian chronological models in Kroon (in prep.). New Approach 
to Prehistoric Migrations: Bayesian Chronological Modelling and Ceramic 
Technology Shed New Light on the Emergence of Corded Ware. Proceedings 
of the Prehistoric Society.

## Structure of the dataset
The folder structure for the dataset is shown in Suppl. Fig. 1. The 
radiocarbon dates and their sources publications can be found under 
`raw_data`. 

```
// Supplementary Figure 1: Folder structure of the Supplementary data
__OxCal_models
 |___Dutch chronology -> (Kroon in prep., Fig. 2)
 |  |_ bayesian_chronological_model.txt
 |  |_ order.csv
 |  |_ outliers.csv
 |  |_ output.csv
 |  |_ visual_output.pdf
 |___European chronology -> (Kroon in prep., Fig. 10)
 |   |_GAC_CW
 |   |_GAE_CW
 |   |_GAW_CW
 |   |_PW_CW
 |___raw_data
    |_radiocarbon_dates.csv
    |_bibliography_radiocarbon_dates
```

The Bayesian chronological models and their output are ordered by the 
figure in the main text in which they appear. The ordering of the 
files under `Dutch_chronology` (see Suppl. Fig. 1) is identical to that 
of the files in the subfolders of `European_chronology`. For ease of 
computation, the transitions between Pitted Ware (PW), Globular Amphora 
West (GAW), East (GAE), and Central (GAC) on the one hand and local 
Corded Ware (CW) groups have been modelled separately.

The files in the folders contain the following information:
- [name]_bayesian_chronological_models.txt: the model in OxCal CQL as 
  run in OxCal v.4.4.4 (Bronk Ramsey 2021).
- [name]_output.csv: the output of the Bayesian chronological model.
- [name]_order.csv: the raw output of the Order() queries.
- [name]_outliers.csv: the Outlier view of the model output.
- [name]_visual_output.pdf: a visual rendering of the model output. 
  These visualisations show the agreement (Amodel) and comvergence (C) 
  of all models. The brackets and OxCal verbs show the model definition.
  The 2 $\sigma$, and 3 $\sigma$ confidence intervals are shown for 
  all distributions. Solid circles indicate the mean $\mu$ of 
  posterior distributions, transparent circles the means of the 
  radiocarbon dates prior to modelling. All years are modelled years BC.

## Radiocarbon dates
The dataset encompasses 704 radiocarbon dates. These radiocarbon dates 
are reported in `raw_data/radiocarbon_dates.csv` following the 
recommendations by Bayliss (2015). 

The radiocarbon dates used here stem from several overview works. 
Chiefly, Bourgeois _et al._ (2025, S1) for CW and Dutch Bell Beaker 
(BB); Müller (2023, Supplement 9) for GAW, GAC, and GAE; Dreshaj _et al._ 
(2022, Appendix B; 2023a, 2023b) and Menne and Brunner (2021, 
Supplement 1) for Swifterbant (SW); and lastly Vanhanen _et al._ (2019, 
Supplementary Table 1) and Lindström (2024, Appendix 2) for PW. The 
radiocarbon datesets for Funnel Beaker West (FBW) and Dutch CW are the 
same from Kroon (2024, Appendix A) and for Vlaardingen (VL) from Kroon 
(in press.).

For a discussion about sample selection, please refer to the main text.

## Model set-up
The aim of these models is to investigate whether the available absolute 
dates support a rapid transition from indigenous societies to Corded 
Ware (CW) during the 3rd millennium BCE in Europe. To this end, the 
models are structured as follows (see Suppl. Fig. 3).

```C++
// Supplementary Figure 3: Example of model structure
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
  Sequence("FBW")
  {
   Boundary("Start_FBW")
   {
    Start("Start_FBW_S");
    Transition("Start_FBW_T");
    End("Start_FBW_E");
   };
   Phase("FBW radiocarbon dates")
   {
    KDE_Plot("FBW")
    {
    };
    Sum()
    {
    };
    //R-dates and appropriate outlier models
    };
   Boundary("End_FBW")
   {
    Start("End_FBW_S");
    Transition("End_FBW_T");
    End("End_FBW_E");
   };
  };
  Order()
  {
  };
  Difference("Overlap FBW_CW", "Start_CW", "End_FBW");
  Difference("Max overlap FBW_CW", "Start_CW_E", "End_FBW_S");
  Difference("Min overlap FBW_CW", "Start_CW_S", "End_FBW_E");
 };
```
The available radiocarbon dates for CW and the indigenous group in the
respective area (e.g., Funnel Beaker West (FBW) in the Dutch uplands) 
are placed in a phase in independent sequences. This set-up avoids any 
assumption about the chronological relations between groups, as would 
be the case if the radiocarbon dates were treated as phases within a 
single sequence (Bronk Ramsey 2009a). 

The dates are calibrated in OxCal v.4.4.4 (Bronk Ramsey 2021) with the 
R-Date() function and the IntCal20 calibration curve (Reimer et al. 
2021). Separate outlier models are applied for cremations (Rose _et al._
2020), charcoal and other dates (Bronk Ramsey 2009b). The odds of an 
outlier were set to 1 for all radiocarbon dates on charcoal and 
calcined bone. The odds of an outlier for the remainder of the samples 
was set to 0.05 and increased as necessary over the course of multiple 
iterations of the model.

The dataset contains several instances in which multiple radiocarbon 
dates were performed on the same organism (e.g., an animal deposition 
at Malice 1, see Witkowska 2021), as well as instances in which 
multiple radiocarbon dates were performed on different organisms buried 
in a single event (e.g. the mass burial at Koszyce 3; Włodarczak _et al._ 
2021). These dates have been incorporated with the R-combine() and 
Combine() functions, respectively (see Bayliss 2015).

The model summarises the dates in each sequence with the KDE_plot() and 
Sum() functions (Bronk Ramsey 2017). The start and end dates of each 
group are then inferred with the trapezium model, since this model is 
held to be the most appropriate for cultural phenomena (Lee and 
Bronk Ramsey 2011).

Finally, the model determines whether overlap between the indigenous 
and migrant groups is likely through the Order() query. The duration of 
this overlap (or gap) is then calculated with the Difference() query.

## Notes about overlaps in main text
The Bayesian chronological models feature three Difference() queries 
per boundary which estimate the overlap between archaeological phenomena. In the .txt files, these are:
- Overlap: the difference between the midpoints of the 
  trapezoidal boundaries, equivalent to the normal boundaries.
- Min overlap: the difference between the start of the start boundary 
  of the youngest phenomenon and the end of the end boundary for the 
  oldest phenomenon.
- Max overlap: the difference between the end of the start boundary of 
  the youngest phenomenon and the start of the end boundary for the oldest phenomenon.

The naming got a bit confused here as I was going through alternative 
parameterisations and had to clean up for the main text. But the overlaps referred to in the text are the Min overlaps. These best reflect the nature of the trapezoidal boundaries (cf. Lee and Bronk Ramsey 2012). 

## Notes about model outputs
In this section, a number of outliers flagged by OxCal in the Bayesian 
chronological models are discussed.

### Dutch chronology
- GrN-16040 (4550±60BP, A=20.4%) from a cremation burial in Haidberg-
  Schöppingen. Attributed to LFG. The calibrated range of this 
  date is relatively old compared to the modelled age. Possibly an
  early outlier for cremation burials.
- GrN-5028 (3810±60BP; A=59.1%) was performed on charcoal from a VL 
  settlement at Leidschendam. Lanting and Van der Plicht (2000, 71) 
  report possible contamination for this date. The date is relatively 
  young compared to its modelled age.
- GrN-26219 (4880±70BP; A=56%) is also relatively young relative 
  compared to its modelled age. This date was performed on peat remnants 
  deemed to be contemporary with a SW settlement. Natural processes 
  during peat growth may explain the outlier (cf. Quik _et al._ 2022).

### European chronology
#### PW-CW
-  Ua-19468 (3890±55BP; A=50.4%) stands out as young compared to its 
   modelled age. This date was performed on human bone from a CW burial,
   but no information was reported to exclude potential reservoir 
   effects.
-  Ua-37496 (4135±45; A=33%) which is old compared to its modelled age. 
   This date too was performed on human bone from a CW burial, but no 
   contextual information was available to evaluate whether reservoir 
   effects play a role.
-  LuS-8676 (3810±50BP; A=41.7%) is young compared to its modelled age. 
   This date stems from animal bone (Sus sp.) from a PW settlement 
   context. No information was reported which could explain this 
   mismatch.
-  Ua-65842 (3805±31BP; A=46.1%) is too young compared to its modelled 
   age. The date stems from a cereal found in a PW settlement context. 
   The seed may have been a younger intrusion.

#### GAW-CW
This model flags 5 dates performed on human bone from CW burials as 
outliers. These dates are all relatively young compared to their 
modelled age, and 4 appear to form a series from the same laboratory. 
-  MAMS-52833 (3771±23BP; A=56.3%)
-  Kl-4146 (3760±45BP; A=47.6%)
-  Kl-4150 (3770±50BP; A=52.4%)
-  Kl-4149/KlA-2968 (3760±30BP; A=46.8%)
-  Kl-4148 (3740±55BP; A=35%)

#### GAC-CW
-  The X2-test in the R_Combine() for Malice 1, feature 32, Individual 
   A fails and returns df=1, T=4.5. The burial contains multiple 
   individuals which differ considerably (ca. 200y BP in some cases) in 
   terms of radiocarbon age. Perhaps a sample misattribution.
-  Poz-44431 (4440±40BP; A=59.6%). The modelled age is relatively young 
   compared to the unmodelled age, causing the mismatch. This date was 
   performed on a human bone from a GA burial context, but no 
   information was reported which enables evaluation of potential 
   reservoir effects.

#### GAE-CW
-  Poz-9587 (4145±35BP; A=33.4%) has a modelled age which is young 
   compared to its calibrated age. The date was performed on a human 
   bone from a CW burial, but no information was reported which enables 
   evaluation of potential reservoir effects.

## How to cite
Please cite the main publication: Kroon, E.J., (in prep.). New Approach 
to Prehistoric Migrations: Bayesian Chronological Modelling and Ceramic 
Technology Shed New Light on the Emergence of Corded Ware. Proceedings 
of the Prehistoric Society.

For the radiocarbon dates, please refer to the sources mentioned in 
`raw_data/radiocarbon_dates.csv`.

## Bibliography
Bayliss, A., 2015. Quality in Bayesian chronological models in 
archaeology. _World Archaeology_ 47(4), 677-700. DOI: 
[10.1080/00438243.2015.1067640](https://doi.org/10.1080/00438243.2015.1067640).

Bourgeois, Q.P.J., Helmecke, F., Olerud, L., Djakovic, I., Guadalupe 
Castro González, M. & Kroon, E.J. 2025a. Spatiotemporal reconstruction 
of Corded Ware and Bell Beaker burial ritual reveals complex dynamics 
divergent from steppe ancestry. _Science Advances_ 11(20), eadx2262. DOI:
[10.1126/sciadv.adx2262](https://doi.org/10.1126/sciadv.adx2262).

Bronk Ramsey, C., 2009a. Bayesian analysis of radiocarbon dates. 
_Radiocarbon_ 51(1), 337–60. [10.1017/S0033822200033865](https://doi.org/10.1017/S0033822200033865).

Bronk Ramsey, C., 2009b. Dealing with outliers and offsets in 
radiocarbon dating. _Radiocarbon_ 51(3), 1023-45. DOI: 
[10.1017/S0033822200034093](https://doi.org/10.1017/S0033822200034093).

Bronk Ramsey, C., 2017. Methods for summarizing radiocarbon datasets. 
_Radiocarbon_ 59(6), 1809-33. DOI: [10.1017/RDC.2017.108](https://doi.org/10.1017/RDC.2017.108).

Bronk Ramsey, C. 2021. _OxCal v4.4.4_.

Dreshaj, M., Dee, M., Peeters, H., and Raemaekers, D., 2022. Blind 
dates: Exploring uncertainty in the radiocarbon evidence on the 
emergence of animal husbandry in the Dutch wetlands. _Journal of_ 
_Archaeological Science: Reports_ 45, 103589. DOI:
[10.1016/j.jasrep.2022.103589](https://doi.org/10.1016/j.jasrep.2022.103589).

Dreshaj, M., Dee, M., Brusgaard, N., Raemaekers, D., and Peeters, H., 
2023a. High-resolution Bayesian chronology of the earliest evidence of 
domesticated animals in the Dutch wetlands (Hardinxveld-Giessendam 
archaeological sites). _PLOS ONE_ 18(1), e0280619. DOI: 
[10.1371/journal.pone.0280619](https://doi.org/10.1371/journal.pone.0280619).

Dreshaj, M., Raemaekers, D., and Dee, M., 2023b. Chronological modeling 
on a calibration plateau: Implications for the emergence of agriculture 
in the Dutch wetlands. _Radiocarbon_ 65(6), 1280–1298. DOI: 
[10.1017/RDC.2023.126](https://doi.org/10.1017/RDC.2023.126).

Kroon, E.J., 2024. _Serial Learners: Interactions between Funnel Beaker_ 
_West and Corded Ware Communities in the Netherlands during the Third_ 
_Millennium BCE from the Perspective of Ceramic Technology_. Leiden: 
Sidestone Press. DOI: [10.59641/4e367hq](https://doi.org/10.59641/4e367hq).

Kroon, E.J., in press. Corded Ware was never alone: Ceramic technology 
and Bayesian chronological modelling demonstrate interactions between 
migrating and indigenous communities in the Netherlands 5,000 years ago.
DOI: [10.17026/AR/3SKXTB](https://doi.org/10.17026/AR/3SKXTB).

Lanting, J.N., and Van der Plicht, J., 2000. De 14C-chronologie van de 
Nederlandse pre- en protohistorie, III: Neolithicum. _Palaeohistoria_ 
41/42, 1-110.

Lee, S., Bronk Ramsey, C. 2012. Development and application of the 
trapezoidal model for archaeological chronologies. _Radiocarbon_ 54(1): 
107-122. DOI: [10.2458/azu_js_rc.v54i1.12397](https://doi.org/10.2458/azu_js_rc.v54i1.12397).

Lindström, T., (2024). _Människor, djur och varelser i miniatyr:_ 
_Flerartliga förbindelser i den gropkeramiska kulturen_ (PhD 
dissertation). Stockholm University, Stockholm.

Menne, J. & Brunner, M., 2021. Transition from Swifterbant to 
Funnelbeaker: A Bayesian Chronological Model. _Open Archaeology_ 7(1), 
1235–43. DOI: [10.1515/opar-2020-0191](https://doi.org/10.1515/opar-2020-0191).

Müller, J., 2023. _Separation, hybridisation, and networks: Globular_ 
_Amphora sedentary pastoralists ca. 3200-2700 BCE_. Leiden: Sidestone 
Press. DOI: [10.59641/a0e4613c](https://doi.org/10.59641/a0e4613c).

Quik, C., Palstra, S.W., Ban Beek, R., Van der Velde, Y., Candel, J.H.
J., Van der Linden, M., Kubiak-Martens, L., Swindles, G.T., Makasake, 
B., Wallinga, J., 2022. Dating basal peat: The geochronology of peat 
initiation revisited. _Quaternary Geology_ 72, 101278. DOI:
[10.1016/j.quageo.2022.101278](https://doi.org/10.1016/j.quageo.2022.101278).

Reimer, P.J., Austin, W.E.N., Bard, E., Bayliss, A., Blackwell, P.G., 
Bronk Ramsey, C., Butzinm M., Cheng, H., Edwards, R.L., Friedrich, M., 
Grootes, P.M., Guilderson, T.P., Hajdas, I., Heaton, T.J., Hogg, A.G., 
Hughen, K.A., Kromer, B., Manning, S.W., Muscheler, R., Palmer, J.G., 
Pearson, C., Van der Plicht, J., Reimer, R.W., Richards, D.A., Scott, 
E.M., Southon, J.R., Turney, C.S.M., Wacker, L., Adolphi, F., Büntgen, 
U., Capano, M., Fahrni, S.M., Fogtmann-Schulz, A., Friedrich, R., 
Köhler, P., Kudsk, S., Miyake, F., Olsen, J., Reinig, F., Sakamoto, M., 
Sookdeo, A., and Talamo, S., 2020. The IntCal20 northern hemisphere 
radiocarbon age calibration curve (0–55 cal kBP). _Radiocarbon_ 62(4), 
725–757. DOI: [10.1017/RDC.2020.41](https://doi.org/10.1017/RDC.2020.41).

Rose, H.A., Meadows, J., and Henriksen M.B., 2020. Bayesian modeling of 
wood-age offsets in cremated bone. _Radiocarbon_ 62(2), 379-401. DOI: 
[10.1017/RDC.2020.3](https://doi.org/10.1017/RDC.2020.3).

Vanhanen, S., Gustafsson, S., Ranheden, H., Björck, N., Kemell, M., and 
Heyd, V., 2019. Maritime hunter-gatherers adopt cultivation at the 
farming extreme of northern Europe 5000 Years Ago. _Scientific Reports_ 
9(1), 4756. DOI: [10.1038/s41598-019-41293-z](https://doi.org/10.1038/s41598-019-41293-z).

Witkowska, B., 2021. Radiocarbon dating of the archival funeral 
complexes of the Globular Amphora culture on the Sandomierz Upland: 
Gajowizna, Malice, Mierzanowice and Sandomierz sites. _Baltic-Pontic_
_Studies_ 25, 7–47. DOI: [10.14746/bps.2021.25.1](https://doi.org/10.14746/bps.2021.25.1).

Włodarczak, P., Szczepanek, A., and Przybyła, M.M., 2021. Grave of the 
Globular Amphora culture from Koszyce in the chronological perspective. 
_Baltic-Pontic Studies_ 25. DOI: [10.14746/bps.2021.25.6](https://doi.org/10.14746/bps.2021.25.6).