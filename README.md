# Doctor Recommendation System
The below markdown contains the detailed dataset descriptions along with explanation of each notebooks and how to run them.
## Table of Contents
- [Introduction](#introduction)
- [Dataset Explanation](#dataset-explanation)
- [Notebook Details](#notebook-details)
- [How to Run the Notebooks](#how-to-run-the-notebooks)

## Introduction
Effective communication between patients and healthcare
providers is fundamental to ensuring accurate diagnosis
and appropriate treatment. However, articulating symptoms
accurately can be challenging for patients, potentially leading
to miscommunication and suboptimal healthcare outcomes.
In this context, applying the advances in artificial intelligence
(AI) and natural language processing (NLP) presents a
promising opportunity to streamline the process of symptom
description and doctor recommendation, ultimately enhancing
the quality of healthcare delivery.
In this paper, we propose a novel approach to automate
the task of doctor recommendation based on patient-provided
symptom descriptions. Traditional doctor recommendation
systems often rely on structured data such as medical records
or user ratings, which may not capture the nuances of
patient symptoms expressed in natural language. But our
methodology overcomes these limitations because it adopts
sophisticated NLP techniques and machine learning.
Central to our approach is the fine-tuning of the DistilGPT-2
language model on a comprehensive dataset of patient-doctor
conversations. This fine-tuning process enables the language
model to understand and generate responses tailored to
healthcare-related queries. By training on a large corpus
of real-world interactions, the model learns to recognize
patterns and understand the context in which symptoms are
described, thus facilitating more accurate and meaningful
communication between patients and healthcare providers.
Then, we employ the SpaCy library for keyword extraction
from the output generated by the language model. These
extracted keywords serve as the foundation for identifying
the most relevant doctor specialties corresponding to the
patient's symptoms. Leveraging a combination of Rouge-1
and Rouge-L scores, we map these keywords to specialist
descriptions, allowing us to recommend the top three specialist
fields that are most closely aligned with the patient's condition.
Furthermore, our approach extends beyond traditional
recommendation systems by incorporating explore and
exploit-based algorithms such as Multi-Armed Bandit
(MAB), Upper Confidence Bound (UCB), and Thompson
Sampling. These algorithms are renowned for their ability
to balance exploration of new options with exploitation of
known information, enabling more efficient and effective
doctor recommendations. By dynamically adapting to patient
preferences and feedback, these algorithms ensure that
patients are matched with the most suitable healthcare
providers based on their unique needs and preferences.

## Dataset Explanation

### 1) disease_description.csv
- **idx**: Unique identifier for each entry.
- **disease**: Name of the disease.
- **Symptom**: Symptoms associated with the disease.
- **reason**: Possible causes or reasons for the disease.
- **TestsAndProcedures**: Diagnostic tests and medical procedures related to the disease.
- **commonMedications**: Common medications prescribed for the disease.
  
Note: Prognosis to disease mapping is not performed due to inefficiency and potential for incorrect results, which could cause patient panic.

### 2) doctors_data_multireward.csv
- **Doctor Speciality**: Specialization of the doctor.
- **Doctor Name**: Name of the doctor.
- **Ratings**: Ratings or feedback score for the doctor.
- **Experience**: Years of experience of the doctor.
- **Distance from Patient**: Distance between the doctor's location and the patient.
- **Availability**: Availability of the doctor for appointments.
- **Cost of Services**: Cost associated with the services provided by the doctor.

This dataset is utilized by explore and exploit algorithms such as Multi-Arm Bandits, Upper Confidence Bound, and Thompson Sampling.

### 3) specialist_description.csv
- **id**: Unique identifier for each entry.
- **Speciality**: Medical specialization or area of expertise.
- **Description**: Description of the specialty, including subspecialties.
- **Subspeciality**: Sub-specialization within the specialty.

This dataset is used for mapping LLM prognosis to the most relevant specialties based on weighted ROUGE scores on keywords of both LLM prognosis and specialty description (using SpaCy).

### 4) updated_multireward.csv
- **Doctor Speciality**: Specialization of the doctor.
- **Doctor Name**: Name of the doctor.
- **Ratings**: Ratings or feedback score for the doctor.
- **Experience**: Years of experience of the doctor.
- **Distance from Patient**: Distance between the doctor's location and the patient.
- **Availability**: Availability of the doctor for appointments.
- **Cost of Services**: Cost associated with the services provided by the doctor.
- **Address**: Address of the doctor's location.
- **Longitude**: Longitude coordinate of the doctor's location.
- **Latitude**: Latitude coordinate of the doctor's location.

This dataset is an extension of doctors_data_multireward.csv and includes additional information such as addresses, longitude, and latitude. The latitude and longitude are used for computing haversine distance between patient and doctors, which is utilized as an arm in Thompson Sampling for explore-exploit algorithms.


## Notebook Details

### 1) Contextual_Bandits.ipynb
- Contains contextual bandits code for the doctor recommendation system.
- Due to computational constraints, the notebook took a considerable amount of time to run.
- Implementation may involve strategies for incorporating contextual information into bandit algorithms for better recommendation performance.

### 2) CSV_Updater_For_Longitude_Latitude.ipynb
- Used to update the doctor dataset with address, longitude, and latitude.
- Coordinates are generated from the geocoder API, as implemented in the Latitude_Longitude_Extractor.ipynb notebook.

### 3) Latitude_Longitude_Extractor.ipynb
- Contains geocoder API implementation to extract longitude and latitude for all entries in the doctors_data_multireward.csv dataset.
- Essential for computing distances between patients and doctors in the recommendation system.

### 4) Exp_and_Expl_Algo_Eval_Metrics.ipynb
- Includes code for regret analysis, hit rate, coverage, and other evaluation metrics for the Multi-Arm Bandit, Upper Confidence Bound, and Thompson Sampling algorithms.
- Helps assess the performance of explore-exploit algorithms in the doctor recommendation system.

### 5) FineTune_LLM.ipynb
- Contains code for fine-tuning a Language Model (LLM) from DistilGPT2 using healthcare data.
- The data, sourced from Hugging Face, comprises 112k patient-doctor conversations covering various medical queries.

### 6) Specialist_Recommender.ipynb
- Contains code for the Upper Confidence Bound (UCB) and Multi-Arm Bandit algorithms.
- Designed for recommending specialists based on patient queries or symptoms.

### 7) Thompson_Sampling.ipynb
- Includes code for the Thompson Sampling algorithm.
- Utilized for making recommendations in the doctor recommendation system based on explore-exploit principles.


## How to Run the Notebooks

1. **Install Conda and Set Up Conda Environment:**
   - Download and install Anaconda or Miniconda from [here](https://www.anaconda.com/products/distribution).
   - Follow the installation instructions for your operating system.

2. **Create and Activate Conda Environment:**
   - Open your terminal or command prompt.
   - Create a new Conda environment:
     ```
     conda create --name doctor_recommendation_env python=3.8
     ```
   - Activate the Conda environment:
     - **Windows**: `conda activate doctor_recommendation_env`
     - **macOS and Linux**: `source activate doctor_recommendation_env`

3. **Install Dependencies:**
   - Navigate to the directory containing the notebook files.
   - Activate the Conda environment.
   - Install dependencies:
     ```
     pip install -r requirements.txt
     ```

4. **Check Data Paths:**
   - Before running any notebook, ensure that the data paths in the code match the actual locations of your datasets.
   - Update file paths as necessary.

5. **Run the Notebook:**
   - Open the notebook file using Jupyter Notebook or JupyterLab.
   - Activate the Conda environment containing dependencies.
   - Run cells one by one or use "Run All" option.
   - Monitor execution for errors or warnings.

6. **Saving Changes:**
   - Save your changes before closing the notebook.

7. **Deactivate the Conda Environment:**
   - Once done, deactivate the Conda environment:
     ```
     conda deactivate
     ```

Follow these steps to set up your environment, install dependencies, and run the notebooks effectively for the doctor recommendation system.


