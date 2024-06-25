# Sleep-Apnea-Prediction
BMET5934: Biomedical Machine Learning

Sleep apnea is a disorder characterised by interruptions of breathing during sleep. Sufferers often wake up gasping for air or unrested and foggy in the morning. Long-term consequences, including high blood pressure, usually lead to poor outcomes.
With an estimated 1 billion people having sleep apnea and more than 90% being undiagnosed, the financial burden of moderate-sever OSA and its associated disease impacts cost Australians $21.2 billion per year.

Current diagnostic methods are complex, cumbersome, and expensive, often involving sleep studies. The ECG offers a relatively inexpensive and accessible way to take health-related measurements, which could be extrapolated using machine learning techniques to diagnose a patient with CPAP therapy successfully. The ECG signal is influenced by two separate physiological processes.

- The instantaneous heart rate
  - Slow during apnoea, fast after apnoea
 
- ECG derived respiration
  - Body surface ECG modulated by breathing
 
- Pulse oximetry (SpO2)
  - During apnoea and hypopnoea, a decrease in SpO2 signal is often seen
 
In this project, which I performed with a group of peers at the University of Sydney in my fourth year of Biomedical Engineering, majoring in Biocomputation, we were given a Train and Test dataset and Test annotations that must be filled with either a positive or negative apnea sign.

The given data parameters we have are
- N = No apnea, A = Apnea
- ECG data
- QRS points
- Sp02 data
- Signal rate (ECG, SP02)

The data must be segmented into 60-second clips, and using all parameters, an estimation of A or N must be filled into the projecttestannotations.mat file.


