# 🍽️ Restaurant Review Analysis Using LLM

## 📌 Project Overview

Customer reviews contain valuable information about food quality, service, and restaurant ambience. However, manually analyzing a large number of reviews can be time-consuming.

This project uses a **Large Language Model (LLM)** to automatically analyze restaurant reviews and extract useful customer insights.

The project uses **Mistral-7B-Instruct** with **Prompt Engineering** to perform sentiment analysis, aspect-level analysis, feature extraction, and customer response generation.

**No Fine-Tuning or RAG is used in this project.**

---

## 🎯 Objective

The main objective of this project is to analyze restaurant reviews using an LLM and generate meaningful insights.

The system performs the following tasks:

* Identify the overall sentiment of each review.
* Analyze sentiment for **Food Quality, Service, and Ambience**.
* Extract liked and disliked features from each aspect.
* Generate a polite and empathetic response for each customer.
* Provide actionable insights from customer feedback.

---

## 🗂️ Dataset

The dataset contains restaurant customer reviews.

| Column          | Description                  |
| --------------- | ---------------------------- |
| `restaurant_id` | Unique ID of the restaurant  |
| `rating_review` | Rating given by the customer |
| `review_full`   | Full customer review         |

**Dataset Size:** 20 rows and 3 columns
**Missing Values:** No missing values

---

## 🧠 Model Used

**Model:** `mistralai/Mistral-7B-Instruct-v0.1`

The model is loaded from **Hugging Face** and used for different restaurant review analysis tasks.

### Model Configuration

* 8-bit quantization using `bitsandbytes`
* `torch.float16` precision
* Automatic device mapping
* Google Colab GPU environment

A reusable `query_mistral()` function is used to send different prompts to the model.

This allows multiple tasks to be performed without retraining the model.

---

## 🔄 Project Workflow

The project follows a **multi-stage prompt engineering pipeline**.

### 1️⃣ Overall Sentiment Analysis

The model analyzes each review and classifies it as:

* Positive
* Negative
* Neutral

The output is generated in a structured JSON format for easier processing.

### 2️⃣ Aspect-Level Sentiment Analysis

Each review is analyzed based on three important restaurant aspects:

* 🍴 Food Quality
* 👨‍🍳 Service
* 🏠 Ambience

This helps identify which part of the restaurant experience affects customer satisfaction.

### 3️⃣ Liked & Disliked Feature Extraction

The system extracts specific features that customers liked or disliked.

Examples:

* **Food:** Taste, freshness, portion size
* **Service:** Staff behaviour, waiting time
* **Ambience:** Seating, cleanliness, atmosphere

This provides more detailed and actionable customer feedback.

### 4️⃣ Customer Response Generation

The LLM generates a polite and empathetic response based on the customer's review.

This can help restaurants respond to customer feedback more efficiently.

---

## 📊 Key Findings

### Overall Sentiment

| Sentiment | Reviews |
| --------- | ------: |
| Negative  |       8 |
| Positive  |       6 |
| Neutral   |       6 |

Negative reviews were slightly higher than positive and neutral reviews.

### Food Quality

| Sentiment      | Reviews |
| -------------- | ------: |
| Negative       |       8 |
| Positive       |       6 |
| Neutral        |       5 |
| Not Applicable |       1 |

Food quality showed a slightly negative trend.

### Service

| Sentiment | Reviews |
| --------- | ------: |
| Negative  |      10 |
| Positive  |       8 |
| Neutral   |       2 |

**Service was the most criticized aspect** in the dataset.

### Ambience

| Sentiment      | Reviews |
| -------------- | ------: |
| Positive       |       8 |
| Neutral        |       6 |
| Not Applicable |       5 |
| Negative       |       1 |

**Ambience was the strongest positive aspect** in the dataset.

---

## 💡 Key Insight

The analysis shows that **Service is the main area that restaurants need to improve, while Ambience is one of their strongest positive factors.**

This type of analysis can help restaurants identify customer pain points and improve their overall customer experience.

---

## ⚙️ Prompt Engineering Approach

Instead of training a new machine learning model, this project uses carefully designed prompts to control the LLM's output.

Different prompts are used for:

* Sentiment classification
* Aspect-level sentiment analysis
* Feature extraction
* Customer response generation

The same LLM can perform multiple tasks by changing the instruction prompt.

---

## 🛠️ Technologies Used

**Programming Language**

* Python

**LLM & NLP**

* Mistral-7B-Instruct
* Hugging Face Transformers
* Prompt Engineering

**Model Inference**

* PyTorch
* Accelerate
* BitsAndBytes

**Data Processing**

* Pandas
* JSON

**Environment**

* Google Colab
* Google Drive
* GPU Runtime

---

## 📁 Project Structure

```text
Restaurant-Review-Analysis/
│
├── Restaurant_Review_Analysis_Using_LLM.ipynb
├── README.md
│
└── data/
    ├── restaurant_reviews.csv
    └── config.json
```

> Dataset and configuration files are not included in the repository.

---

## ▶️ How to Run

### Step 1: Open the Notebook

Open the project notebook in **Google Colab**.

### Step 2: Enable GPU

Go to:

`Runtime → Change runtime type → GPU`

### Step 3: Install Required Libraries

```bash
pip install transformers accelerate bitsandbytes
```

Restart the runtime if required.

### Step 4: Add Required Files

Place the following files in the required Google Drive location:

* `restaurant_reviews.csv`
* `config.json`

### Step 5: Run the Notebook

Run all cells sequentially.

The Mistral-7B model may take some time to load depending on the available GPU.

### Step 6: View the Results

The notebook generates:

* Overall sentiment
* Aspect-level sentiment
* Liked features
* Disliked features
* Customer responses

---

## 📈 Evaluation

The model output can be evaluated using manually labeled reviews.

A subset of reviews can be manually classified and compared with the LLM predictions.

This can be used to calculate:

* Accuracy
* Precision
* Recall
* F1-Score

The performance can also be improved by:

* Improving prompt instructions
* Using better output formatting
* Adjusting `temperature`
* Adjusting `top_p`
* Testing different prompts

---

## ⚠️ Limitations

* The dataset contains only 20 reviews.
* LLM-generated results may vary depending on the prompt and generation settings.
* Some reviews may contain ambiguous opinions.
* Model outputs should be manually checked before making important business decisions.
* The project does not use Fine-Tuning or RAG.

---

## 🚀 Future Improvements

* Use a larger restaurant review dataset.
* Build an interactive **Streamlit dashboard**.
* Add automated evaluation metrics.
* Perform restaurant-wise sentiment comparison.
* Add sentiment visualization.
* Analyze review trends over time.
* Support multiple languages.
* Compare results with other open-source LLMs.

---

## 👨‍💻 Project Highlights

* Built an **LLM-based restaurant review analysis system**.
* Used **Mistral-7B-Instruct** for natural language understanding.
* Applied **Prompt Engineering** without Fine-Tuning or RAG.
* Implemented multi-stage sentiment and aspect analysis.
* Extracted actionable customer feedback.
* Generated automated customer responses.
