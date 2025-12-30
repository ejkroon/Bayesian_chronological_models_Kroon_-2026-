# Bayesian_chronological_models_Kroon_-2026-
Bayesian chronological models for Kroon, E.J., (in prep). New Approach to Prehistoric Migrations: Bayesian Chronological Modelling and Ceramic Technology Shed New Light on the Emergence of Corded Ware. Proceedings of the Prehistoric Society

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