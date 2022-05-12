# g.tec & BR41N.IO Hackathon on Unresponsive Wakefulness Syndrome (UWS) analysis

## Analyze a vibro-tactile P300 BCI data-set from a patient with disorders of consciousness in order to optimize pre-processing, feature extraction and classification algorithms.

## 1. Introduction

Predicting the outcome in terms of recovery from disorders of consciousness (DOC) in patients who survive coma after acute brain injury is challenging. Predicting patient outcome in unresponsive wakefulness syndrome (UWS) and minimally conscious state (MCS) is of great importance because it directly affects the follow-up treatment and financial burden on the patient's family and society. Widely used behavioral assessment of awareness has objective limitations, and it has been estimated that up to 30–40% of MCS
patients are erroneously classified as UWS. For the current task, Brain-computer interfaces (BCIs) - systems that provide a direct connection between brain activity and computer systems were used. BCIs for DOC patients are based on the principle that performing a mental task (such as discriminating among target and non-target stimuli) can elicit distinct EEG signals that can be used to infer conscious awareness. BCIs for DOC patients often (but not always) rely on auditory or vibrotactile stimuli, since
DOC patients typically cannot use visual-based interfaces.

## 2. Methods and materials

The experiments were performed using EEG-based Brain-computer interface. It includes a laptop with installed software, three vibrotactile stimulators installed on both wrists and back of subject, two in-ear headphones, one g.USBamp biosignal amplifier with 16 channels and one EEG cap with 16 active electrodes. EEG data were recorded from an electrode cap with eight channels (Fz, C3, Cz, C4, CP1, CPz, CP2 and Pz) based on the International 10–20 electrode system, then amplified and sampled at 256 Hz. The left and right vibrotactile stimulators were placed on each wrist, and a third stimulator placed on the back receives 75% of the stimuli, acting as a distractor. Thus, each
sequence of eight stimuli was presented at a ratio of 1:1:6. Each run lasted 3 min and contained four trial blocks. At the beginning of each trial block, the subject was verbally instructed by headphones to mentally count stimuli on the right or the left wrist.

## 3. Dataset

Dataset contains electroencephalograms (EEG) of two patients and is divided into 4 MATLAB files for each patient (Fig. 1). The dataset was only available to hackathon participants.

![image](https://user-images.githubusercontent.com/69838126/168012932-c531881c-1b7a-4e7d-a451-83ce07a90b07.png)
Fig. 1. Dataset.

## 4. Signal processing and classification

MATLAB files were converted to Python array format and the raw EEG data was extracted. Data epochs from -100 ms to 600 ms from each stimulation point were created and baseline corrected. The data were notch-filtered at 50 Hz and bandpass-filtered within 0.1–30 Hz. Trials with an amplitude above 100 mV were automatically rejected.

## 5. Signal processing and classification

I use 6 different machine learning pipelines to classify the P300 event-related components based on the each patient EEG data.

* Vect + LR : Vectorization of the trial + Logistic Regression. This can be considered the standard decoding pipeline for MEG / EEG.
* Vect + RegLDA : Vectorization of the trial + Regularized LDA. This one is very commonly used in P300 BCIs. It can outperform the previous one but become unusable if the number of dimension is too high.
* ERPCov + TS: ErpCovariance + Tangent space mapping. Riemannian geometry-based pipeline.
* ERPCov + MDM: ErpCovariance + MDM. Riemannian geometry classifier with classification by Minimum Distance to Mean.
* XdawnCov + TS: XdawnCov + Tangent space mapping. PyRiemann's ERP classifier, which is decoding in the tangent space with a logistic regression.
* XdawnCov + MDM: XdawnCovariance + MDM. Similar to the ErpCovariance classifier with classification by Minimum Distance to Mean.

Evaluation is done through cross-validation, with accuracy as metric. The classifier score was calculated for each trial group (7 non-target trials, 1 target trial), and the trial with the highest score was identified as the target. If the classified trial was the real target, the classification was considered correct.

## 6. Results

The best accuracy of ~87% was obtained from the ErpCovariance + Tangent space bundle (Fig. 2).

![image](https://user-images.githubusercontent.com/69838126/168031177-93260bad-1526-4b53-a69f-2c975ca0edb6.png)
Fig. 2. Classifier performance.

However, during the analysis of the EEG data, I combined and averaged all epochs for each patient. Visualizing this, everyone can see in the resulting plot that the first patient shows a distinct N170 ERP response (negative peak at around 170 ms after stimulation) to an auditory stimulus along with a distinct P300 ERP response (positive peak at around 300 ms after stimulation) to the vibrotactile stimulus (Fig. 3). The opposite picture is observed with the second patient, which visualization looks like noise. With this information, we can say with 100% accuracy that the first patient was conscious during the session and the second patient was unconscious during the session. My next goal was to find a classifier that can predict the mentioned visual observations with 100% accuracy.
For this problem, I combined all the available target epochs of each patient and used the same 6 ML pipelines for classification. All four pipelines based on Riemannian geometry were able to distinguish between the presence or absence of ERP P300 response to stimuli (Fig. 4).

## 7. Conclusion

By combining the vibrotactile P300-based Brain-Computer Interface (BCI), basic signal processing, and Riemann's ERP classifier, we can assess with 100% accuracy whether a patient is in the Unresponsive Wakefulness State or Conscious State.
