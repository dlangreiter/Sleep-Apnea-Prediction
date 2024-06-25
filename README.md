# Sleep-Apnea-Prediction
BMET5934: Biomedical Machine Learning

**Project Description**:
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

**Project Main**

**_Files_**
- _maintest.py_ = File containing functions to find the best hyperparameters
- _tools.py_ = File continaing various functions
- _mainsteps_hyperparamter_tuning.py_ = File containing various functions and workflows to find the best hyperparameters and save the best model.
- _demo.ipynb_ = Demo shown to lab tutours
- _FixPython.m_ = Used to fix mat file created by python

**_Function flow chart:_**

_Record:_
MATlAB's cellular data was transformed into a Python dictionary using the "h5storage" package.

We designed a function to segment records into 60-second pieces, accounting for sample rates (ECG, SP02).

Separated ECG, SP02, and QRS data into a structured dictionary. 

_Noise Cancellation for ECG data_:
IIR notch filter with 50 Hz resonant frequency to eliminate power supply noise. A median filter was also used to address baseline wandering.

A 4th-order bandpass Butterworth filter (0.2,50Hz) was applied to remove noises outside the ECG frequency range. This was then incorporated with the Savitzky-Golay filter (6th order, 30-window width) to enhance signal-to-noise ratio (SNR) while retaining signal shape.

**Network Structure**

**_Feautre extraction block: RCTG (Residual connect + CNN + Attention + GRU)_**
Data Entry:
- Initial data is reshaped into a default of 32 channels upon entry.

Two main Structures:
- GRU (Gated Recurrent Unit):
- An optimised version of LSTM with a reduced number of gates.
- It comprises four layers, each having a bidirectional GRU, a batch normalisation, and a dropout layer with a rate of 0.75.
- Inspiration Erdenebayar Urtnasan's team, which achieved a 0.99 F1 score processing single lead ECG data from 92 SDB patients (Urtnasan et al., 2018)

- **CNN**
Feature extraction with convolution layers (kernel size 11), activation, dropout (rate specifically not mentioned), and max-pooling (stride of 2).

Output then feeds into a multi-head attention layer, enhancing features and weight distribution.

Incorporates residual connections to mitigate information loss due to the network's depth (He et al., 2016)

The output from the attention mechanism is fused with the GRU output and reshaped using convolution to achieve the desired channel configuration.

![image](https://github.com/dlangreiter/Sleep-Apnea-Prediction/assets/106584079/24f2407f-fbff-439f-837d-6a1cfc20cc2b)

For the total network structure, two different inputs are present. ECG signal and SP02 signal. The ECG part has two basic feature extraction modules and is followed by a GRU layer. The SP02 part has one basic feature extraction module and one GRU layer. Then, after those signals are processed in two parts of the network, results will be put together using an average pool layer, total connection layers and SoftMax layer to generate the final output of the classifier.

![image](https://github.com/dlangreiter/Sleep-Apnea-Prediction/assets/106584079/24c775d6-f50a-4137-ab7a-bfba37fe6eed)

**Some reference：**
Almutairi, H., Hassan, G. M., & Datta, A. (2023). Classification of sleep stages from EEG, EOG and EMG signals by SSNet (arXiv:2307.05373). arXiv. https://doi.org/10.48550/arXiv.2307.05373

Álvarez, D., Hornero, R., Abásolo, D., Campo, F. del, & Zamarrón, C. (2006). Nonlinear characteristics of blood oxygen saturation from nocturnal oximetry for obstructive sleep apnoea detection. Physiological Measurement, 27(4), 399. https://doi.org/10.1088/0967-3334/27/4/006

Dai, Y., Gieseke, F., Oehmcke, S., Wu, Y., & Barnard, K. (2021). Attentional Feature Fusion. 3560–3569. https://openaccess.thecvf.com/content/WACV2021/html/Dai_Attentional_Feature_Fusion_WACV_2021_paper.html

Deb, K., & Jain, H. (2014). An Evolutionary Many-Objective Optimization Algorithm Using Reference-Point-Based Nondominated Sorting Approach, Part I: Solving Problems With Box Constraints. IEEE Transactions on Evolutionary Computation, 18(4), 577–601. https://doi.org/10.1109/TEVC.2013.2281535

He, K., Zhang, X., Ren, S., & Sun, J. (2016). Deep Residual Learning for Image Recognition. 770–778. https://openaccess.thecvf.com/content_cvpr_2016/html/He_Deep_Residual_Learning_CVPR_2016_paper.html

Kawala-Sterniuk, A., Podpora, M., Pelc, M., Blaszczyszyn, M., Gorzelanczyk, E. J., Martinek, R., & Ozana, S. (2020). Comparison of Smoothing Filters in Analysis of EEG Data for the Medical Diagnostics Purposes. Sensors, 20(3), Article 3. https://doi.org/10.3390/s20030807

Lempel, A., & Ziv, J. (1976). On the Complexity of Finite Sequences. IEEE Transactions on Information Theory, 22(1), 75–81. https://doi.org/10.1109/TIT.1976.1055501

Lin, T.-Y., Goyal, P., Girshick, R., He, K., & Dollar, P. (2017). Focal Loss for Dense Object Detection. 2980–2988. https://openaccess.thecvf.com/content_iccv_2017/html/Lin_Focal_Loss_for_ICCV_2017_paper.html

Loshchilov, I., & Hutter, F. (2019). Decoupled Weight Decay Regularization (arXiv:1711.05101). arXiv. https://doi.org/10.48550/arXiv.1711.05101

Misra, D. (2020). Mish: A Self Regularized Non-Monotonic Activation Function (arXiv:1908.08681). arXiv. https://doi.org/10.48550/arXiv.1908.08681

Mostafa, S. S., Mendonça, F., G. Ravelo-García, A., & Morgado-Dias, F. (2019). A Systematic Review of Detecting Sleep Apnea Using Deep Learning. Sensors, 19(22), Article 22. https://doi.org/10.3390/s19224934

Urtnasan, E., Park, J.-U., Joo, E.-Y., & Lee, K.-J. (2018). Automated Detection of Obstructive Sleep Apnea Events from a Single-Lead Electrocardiogram Using a Convolutional Neural Network. Journal of Medical Systems, 42(6), 104. https://doi.org/10.1007/s10916-018-0963-0

Van Steenkiste, T., Groenendaal, W., Ruyssinck, J., Dreesen, P., Klerkx, S., Smeets, C., de Francisco, R., Deschrijver, D., & Dhaene, T. (2018). Systematic Comparison of Respiratory Signals for the Automated Detection of Sleep Apnea. 2018 40th Annual International Conference of the IEEE Engineering in Medicine and Biology Society (EMBC), 449–452. https://doi.org/10.1109/EMBC.2018.8512307

Zhu, J., Zhou, A., Gong, Q., Zhou, Y., Huang, J., & Chen, Z. (2022). Detection of Sleep Apnea from Electrocardiogram and Pulse Oximetry Signals Using Random Forest. Applied Sciences, 12(9), Article 9. https://doi.org/10.3390/app12094218



