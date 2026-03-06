# Remote Monitoring of Pregnant Women in Ghana Using a Wearable Device and Mobile App

![University of Ghana](https://img.shields.io/badge/University%20of%20Ghana-Legon-blue)
![Department](https://img.shields.io/badge/Dept-Computer%20Engineering-green)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)
![License](https://img.shields.io/badge/License-Academic-orange)

> **Final Year Project** — Department of Computer Engineering, School of Engineering Sciences, University of Ghana, Legon.

---

## Table of Contents

- [Overview](#overview)
- [The Problem](#the-problem)
- [Our Solution](#our-solution)
- [System Architecture](#system-architecture)
- [Hardware Components](#hardware-components)
- [Software & Technologies](#software--technologies)
- [AI/ML Models](#aiml-models)
- [Mobile App (Awopa)](#mobile-app-awopa)
- [Results & Accuracy](#results--accuracy)
- [Project Structure](#project-structure)
- [Team](#team)
- [Supervisors](#supervisors)
- [Acknowledgements](#acknowledgements)

---

## Overview

Pregnancy-related complications remain a **major cause of maternal and infant mortality** in developing countries. In Ghana, about **12% of female deaths** (ages 15-49) over the past five years are attributed to pregnancy complications, with **62% of these deaths** resulting from delayed or inadequate medical intervention.

This project delivers an **IoT-based wearable device** paired with a **mobile application** and **AI predictive models** to remotely monitor pregnant women's vital signs and predict complications like **preeclampsia** and **anemia** before they become life-threatening.

```
Wearable Device ──> Fog Computing (AI) ──> Cloud Server ──> Mobile App
   (Sensors)         (Predictions)         (Storage)       (Alerts & Dashboard)
```

---

## The Problem

```
                        WHY THIS MATTERS
  ┌─────────────────────────────────────────────────────────┐
  │  40% of pregnancies worldwide face health risks (WHO)   │
  │  62% of maternal deaths in Ghana = delayed intervention │
  │  50%+ fatalities could be avoided with early detection  │
  └─────────────────────────────────────────────────────────┘
```

Pregnant women in Ghana face barriers to regular antenatal care including:

- **Long travel distances** to healthcare facilities
- **High transportation costs** and consultation fees
- **Limited availability** of diagnostic tools in rural hospitals
- **Overburdened healthcare providers** with high patient-to-doctor ratios
- **No digital alert systems** to warn of impending health risks

---

## Our Solution

An integrated system with **three core components** working together:

### 1. Wearable Device (Sensor Sub-division)
A wrist-worn IoT device that continuously captures vital signs:

| Vital Sign | Sensor Used | Protocol |
|---|---|---|
| Heart Rate & SpO2 | MAX30102 | I2C |
| Blood Glucose (non-invasive) | AS7263 (NIR Spectroscopy) | I2C |
| Body Temperature | TMP117 | I2C |
| Motion / Activity | BMI270 (Accelerometer + Gyroscope) | I2C |

- **Microcontroller:** Heltec ESP32-S3
- **Power:** 3.7V Lithium Polymer (LiPo) battery
- **Connectivity:** Wi-Fi + Bluetooth Low Energy (BLE)
- **Data Interval:** Every 60 minutes, stored in EEPROM
- **Casing:** 3D-printed enclosure (designed in SolidWorks)

### 2. AI Predictive Models (Fog Computing Sub-division)
Machine learning models analyze vitals in real-time to predict:
- **Preeclampsia** — using blood pressure (systolic, diastolic, MAP) and protein in urine
- **Anemia** — using BMI, age, and risk factor indicators

### 3. Mobile App & Cloud (Cloud Sub-division)
- **Flutter-based** cross-platform mobile app called **"Awopa"**
- Role-based dashboards for **Pregnant Women**, **Medical Officers**, and **Admins**
- Cloud storage on **AWS** with **MongoDB** database
- **AES-256 encryption** for all health data
- **Facial recognition** authentication (Face-api.js)

---

## System Architecture

```
┌──────────────┐     ┌──────────────────┐     ┌──────────────┐
│   PREGNANT   │     │    WEARABLE      │     │    CLOUD     │
│    WOMAN     │────>│    DEVICE        │────>│    SERVER    │
│              │     │  (ESP32-S3 +     │     │  (AWS + MongoDB)
│  Registration│     │   4 Sensors)     │     │              │
└──────┬───────┘     └──────────────────┘     └──────┬───────┘
       │                                             │
       │         ┌──────────────────┐                │
       │         │   AI PREDICTIVE  │                │
       └────────>│     MODELS       │<───────────────┘
                 │  (Preeclampsia   │
                 │   & Anemia)      │
                 └────────┬─────────┘
                          │
                 ┌────────v─────────┐
                 │   MOBILE APP     │
                 │   (Awopa)        │
                 │                  │
                 │ - Vitals Display │
                 │ - Risk Alerts    │
                 │ - Doctor Chat    │
                 │ - Appointments   │
                 │ - Chatbot        │
                 └──────────────────┘
```

---

## Hardware Components

| Component | Purpose |
|---|---|
| **Heltec ESP32-S3** | Main microcontroller with Wi-Fi & BLE |
| **MAX30102** | Pulse oximetry — heart rate & blood oxygen (SpO2) |
| **AS7263** | Near-infrared spectroscopy — non-invasive blood glucose |
| **TMP117** | High-accuracy digital temperature sensor |
| **BMI270** | 6-axis IMU — accelerometer + gyroscope for motion tracking |
| **3.7V LiPo Battery** | Portable power supply |
| **3D-Printed Casing** | Protective wrist-worn enclosure |

All sensors communicate via the **I2C bus** (SDA: GPIO 41, SCL: GPIO 42).

---

## Software & Technologies

### Wearable Firmware
| Tool | Purpose |
|---|---|
| **PlatformIO (VSCode)** | Development environment |
| **C++** | Firmware programming language |
| **ESP-IDF / FreeRTOS** | Real-time task scheduling |

### AI / Machine Learning
| Tool | Purpose |
|---|---|
| **Python 3.x** | Primary language |
| **Jupyter Notebook** | Development environment |
| **Scikit-learn** | Model training & evaluation |
| **Pandas / NumPy** | Data processing |
| **StandardScaler** | Feature normalization |

### Mobile Application
| Tool | Purpose |
|---|---|
| **Flutter** | Cross-platform mobile framework |
| **NestJS** | Backend API framework |
| **MongoDB** | NoSQL database |
| **Docker** | Containerization |
| **Redis** | Caching |
| **Apache Kafka** | Event streaming |
| **ElasticSearch** | Search functionality |
| **WebSockets** | Real-time communication |
| **Firebase Cloud Messaging** | Push notifications |
| **LangChain** | Pregnancy chatbot AI |
| **Face-api.js** | Facial recognition auth |
| **AWS S3** | Cloud storage |

---

## AI/ML Models

The system uses **Multiclass Logistic Regression** classifiers to predict pregnancy complications with severity levels: **No Risk**, **Mild**, **Moderate**, and **Severe**.

### Preeclampsia Prediction Model

| Metric | Score |
|---|---|
| Training Accuracy | **97%** |
| Test Accuracy | **97%** |
| Validation Accuracy | **97%** |

**Input Features:** Systolic BP, Diastolic BP, Mean Arterial Pressure (MAP), Protein in Urine

### Anemia Prediction Model

| Metric | Score |
|---|---|
| Training Accuracy | **98%** |
| Test Accuracy | **94%** |
| Validation Accuracy | **93%** |

**Input Features:** BMI Value, Age, Risk Factors (e.g., Excessive Vomiting)

Both models were validated against **real clinical data** from 30 pregnant women at **Britannia Medical Centre (BMC)** in Tema, Ghana, and reviewed by a medical consultant using blind test data.

---

## Mobile App (Awopa)

The app provides three role-based dashboards:

### Pregnant Woman Dashboard
- View real-time vitals (Blood Pressure, SpO2, Temperature, Heart Rate, Glucose)
- Create/cancel appointments
- Input protein in urine values
- Access pregnancy tips & info desk
- Interact with AI pregnancy chatbot
- Manage emergency contacts
- Receive risk alerts and notifications

### Medical Officer Dashboard
- Write prescriptions
- Real-time chat with patients
- View live preeclampsia & anemia predictions
- Monitor patient glucose values
- Manage appointments
- Access preeclampsia risk assessments

### System Admin Dashboard
- Manage notifications
- Handle support requests
- View all registered users

---

## Results & Accuracy

The system was tested with anonymized data from **30 pregnant women** at Britannia Medical Centre (BMC), Tema, covering cases of anemia, preeclampsia, and normal conditions.

```
  PREECLAMPSIA MODEL                    ANEMIA MODEL
  ┌─────────────────────┐              ┌─────────────────────┐
  │ Train:  97%         │              │ Train:  98%         │
  │ Test:   97%         │              │ Test:   94%         │
  │ Valid:  97%         │              │ Valid:  93%         │
  └─────────────────────┘              └─────────────────────┘
```

The wearable device measurements were also compared against **standard medical reference devices** (sphygmomanometer, pulse oximeter, thermometer) to verify sensor accuracy.

---

## Project Structure

```
PregnancyMonitorProject/
├── DemonstrationVideo/          # Live testing video demo
│   └── LiveTesting.mp4
├── Materials/                   # Research literature & references
│   ├── Others-Anemia/           # Anemia-related research papers
│   ├── ProblemStatement.pdf
│   └── ValidArticles/           # Thesis & extra research articles
│       ├── ThesisArticles/
│       └── ExtraArticles/
├── Poster/                      # Project poster presentation
│   └── Wearable Poster.pptx
├── Presentations/               # All project presentations
│   ├── Defense/                  # Final defense slides
│   ├── Progress/                # Progress report slides
│   ├── Proposal/                # Initial proposal slides
│   └── WeeklyMeetings/         # Weekly progress updates (WK1-Final)
├── Results/                     # Test measurement data
│   └── WearableMeasurements.xlsx
├── Thesis/                      # Final thesis document
│   ├── PregnancyMonitoringProject -GM.docx
│   └── PregnancyMonitoringProject -GM.pdf
├── PregnancyMonitoringProject_Thesis.pdf   # Submitted thesis PDF
├── PregnancyMonitoringProject-Prof.Mills.docx
└── README.md                    # This file
```

---

## Team

| Name | Student ID |
|---|---|
| **Owoh Einsteina Ofunne** | 10943874 |
| **Anane George Nyarko** | 10947340 |
| **Adika Nathaniel Agbesi Junior** | 10957036 |

---

## Supervisors

- **Prof. Godfrey A. Mills** — Supervisor
- **Prof. Elsie Effah Kaufmann** — Co-Supervisor

---

## Acknowledgements

Special thanks to the collaborators who made this project possible:

- **Dr. Kobby Appiah-Sakyi** — Medical Consultant, Britannia Medical Centre
- **Dr. Frank Amegah** — Senior Resident, Korle-Bu Teaching Hospital
- **Mr. Nathaniel Sakyi Adubier** — Senior Research Engineer, IT Consortium
- **IT Consortium** — Project initiation and hardware funding
- **Mr. Robert Sedohia** — Chief Technician, Dept. of Computer Engineering

---

> *"Coming together is a beginning, staying together is progress, and working together is success."*

**University of Ghana, Legon | Department of Computer Engineering | October 2025**
