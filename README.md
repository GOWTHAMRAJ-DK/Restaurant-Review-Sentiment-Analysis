# 🍽️ Restaurant Review Sentiment Analysis using LLM Prompt Engineering

An LLM-powered pipeline that analyzes restaurant customer reviews and converts unstructured text into **structured sentiment insights, aspect-level feedback, liked/disliked features, and customer responses**.

This project uses **Mistral-7B-Instruct-v0.1** with **prompt engineering only** — no fine-tuning and no RAG.

---

## 📌 Project Overview

Customer reviews contain valuable information about food quality, service, ambience, and overall customer satisfaction.

However, manually analyzing a large number of reviews is time-consuming and difficult to scale.

This project automates the review analysis process using an LLM. For every review, the system:

- Identifies the overall sentiment
- Performs aspect-based sentiment analysis
- Extracts liked and disliked features
- Generates a polite, sentiment-aware customer response

The goal is to transform raw customer reviews into **actionable restaurant insights**.

---

## 🎯 Objectives

### 1. Overall Sentiment Analysis
Classify each review as:

- 🟢 Positive
- 🟡 Neutral
- 🔴 Negative

### 2. Aspect-Based Sentiment Analysis

Analyze sentiment for:

- 🍴 Food Quality
- 👨‍🍳 Service
- 🏠 Ambience

If an aspect is not mentioned in the review, it is classified as **Not Applicable (N/A)**.

### 3. Feature Extraction

Identify specific positive and negative points mentioned by customers.

**Examples:**

- Food → Taste, freshness, portion size
- Service → Waiting time, staff behaviour
- Ambience → Lighting, cleanliness, atmosphere

### 4. Customer Response Generation

Generate a polite and context-aware response based on the customer's sentiment and feedback.

---

## 📂 Dataset

The project uses a CSV file named:

```text
restaurant_reviews.csv
```

### Dataset Structure

| Column | Description |
|---|---|
| `restaurant_ID` | Unique restaurant identifier |
| `rating_review` | Customer star rating |
| `review_full` | Complete customer review |

### Dataset Details

- Total reviews: **20**
- Ratings: **1, 3, and 5 stars**
- 5 stars → Positive
- 3 stars → Neutral
- 1 star → Negative

> ⭐ Star ratings are used as proxy labels for evaluating overall sentiment.

The dataset is not included in this repository.

---

## 🛠️ Tech Stack

### Programming Language

- Python

### LLM

- `mistralai/Mistral-7B-Instruct-v0.1`

### Libraries

- Transformers
- PyTorch
- Accelerate
- BitsAndBytes
- Pandas
- Scikit-learn

### Environment

- Google Colab
- NVIDIA T4 GPU or better

---

## ⚙️ System Workflow

```text
Customer Reviews CSV
        ↓
Load & Preprocess Reviews
        ↓
Load Mistral-7B-Instruct
        ↓
Prompt 1
Overall Sentiment
        ↓
Prompt 2
Aspect-Level Sentiment
        ↓
Prompt 3
Liked / Disliked Features
        ↓
Evaluate Predictions
        ↓
Zero-shot vs Few-shot Comparison
        ↓
Prompt 4
Customer Response Generation
        ↓
Structured JSON Output
```

---

## 🔑 Key Techniques

### 🧠 Prompt Engineering

The project relies on carefully designed prompts to guide the LLM to produce consistent sentiment classifications and structured outputs.

No model fine-tuning was performed.

### 📦 Structured JSON Output

The prompts instruct the model to return information in a predefined JSON structure.

Example:

```json
{
  "overall_sentiment": "Positive",
  "food_quality": "Positive",
  "service": "Neutral",
  "ambience": "Positive"
}
```

This makes the model output easier to process programmatically.

### 🔄 JSON Extraction & Retry

LLM responses may occasionally contain malformed or additional text.

To handle this, the pipeline uses:

- Regex-based JSON extraction
- `json.loads()`
- Retry handling for invalid responses

### 🎯 Deterministic Generation

The model uses:

```python
do_sample=False
```

This helps produce reproducible outputs during evaluation.

### ⚡ Quantized Model Loading

The notebook attempts different model-loading configurations depending on the available GPU memory:

```text
8-bit quantization
      ↓
4-bit quantization
      ↓
FP16 fallback
```

This makes the notebook more flexible across different Colab GPU environments.

---

## 🔬 Prompting Approach

Two prompting strategies were compared.

### Baseline — Zero-Shot

The model receives the task instructions without example outputs.

### Improved — Few-Shot + Definitions

The prompt was enhanced with:

- Clear sentiment definitions
- Aspect definitions
- Example inputs and outputs
- More explicit classification instructions

This was tested to determine whether additional prompt guidance improved performance.

---

## 📊 Evaluation Results

Star ratings were used as proxy ground truth:

| Rating | Proxy Sentiment |
|---:|---|
| ⭐⭐⭐⭐⭐ 5 | Positive |
| ⭐⭐⭐ 3 | Neutral |
| ⭐ 1 | Negative |

### Overall Sentiment Performance

| Prompt | Accuracy | Macro-F1 |
|---|---:|---:|
| Baseline — Zero-Shot | **0.95** | **0.95** |
| Few-Shot + Definitions | **0.95** | **0.95** |

### Predicted Sentiment Distribution

| Sentiment | Reviews |
|---|---:|
| Positive | 6 |
| Neutral | 7 |
| Negative | 7 |

---

## 📌 Aspect-Level Results

### 👨‍🍳 Service

- Negative: **10**
- Positive: **9**
- Neutral: **1**

### 🍴 Food Quality

- Positive: **7**
- Negative: **6**
- Neutral: **5**
- N/A: **2**

### 🏠 Ambience

- Positive: **8**
- Neutral: **7**
- N/A: **4**
- Negative: **1**

These aspect-level results help identify which parts of the restaurant experience customers are satisfied or dissatisfied with.

---

## 🔍 Error Analysis

The evaluation identified **1 misclassified review**.

The review had a **5-star rating**, but the model classified it as **Neutral**.

This highlights an important limitation of using star ratings as proxy sentiment labels: a high rating does not always perfectly correspond to the sentiment expressed in the review text.

---

## 🚀 How to Run

### 1. Open Google Colab

Open the project notebook in Google Colab.

### 2. Enable GPU

Go to:

```text
Runtime → Change runtime type → T4 GPU
```

### 3. Install Dependencies

Run the installation cell provided in the notebook.

After installation:

```text
Runtime → Restart session
```

> ⚠️ Do not run the installation cell again after restarting the session.

### 4. Upload Dataset

Upload:

```text
restaurant_reviews.csv
```

to Google Drive.

Update the dataset path in the notebook accordingly.

### 5. Hugging Face Token — Optional

A Hugging Face token can optionally be added as a Colab secret:

```text
HF_TOKEN
```

The model is publicly available, so a token is not required for normal access.

### 6. Run the Notebook

Run the remaining cells in order.

---

## 📁 Project Structure

```text
restaurant-review-sentiment/
│
├── restaurant_review_sentiment.ipynb
├── README.md
└── restaurant_reviews.csv   # Not included in repository
```

---

## 💡 Example Use Case

A restaurant receives hundreds of customer reviews every day.

Instead of manually reading every review, this system can automatically identify:

```text
Overall Sentiment
        ↓
Positive / Neutral / Negative

        +

Food Quality
        ↓
Positive / Neutral / Negative / N/A

        +

Service
        ↓
Positive / Neutral / Negative / N/A

        +

Ambience
        ↓
Positive / Neutral / Negative / N/A

        +

Customer Feedback
        ↓
Liked / Disliked Features

        +

LLM Generated Response
```

This can help restaurants quickly understand customer feedback and prioritize areas for improvement.

---

## ⚠️ Limitations

- Dataset contains only **20 reviews**
- Evaluation results are therefore indicative rather than statistically strong
- Star ratings are only proxy labels for sentiment
- Mistral-7B-Instruct-v0.1 is an older 7B model compared with newer LLMs
- Prompt-based classification can still produce occasional incorrect outputs
- Aspect-level results are not evaluated against manually annotated ground truth

---

## 🔮 Future Improvements

### 📈 Larger Dataset

Use a larger dataset with manually verified sentiment and aspect labels.

### 🏷️ Human-Annotated Ground Truth

Create reliable labels for:

- Overall sentiment
- Food sentiment
- Service sentiment
- Ambience sentiment

### 🤖 Newer LLMs

Experiment with newer and stronger open-source or commercial LLMs.

### 🖥️ Gradio Interface

Build an interactive interface where users can enter a restaurant review and instantly receive:

- Overall sentiment
- Aspect sentiment
- Key feedback
- Generated response

### 📊 Restaurant Insight Dashboard

Create a dashboard that aggregates reviews by restaurant and displays:

- Sentiment distribution
- Food satisfaction
- Service satisfaction
- Ambience satisfaction
- Common complaints
- Common positive features

---

## 🎓 Key Learning Outcomes

Through this project, I gained practical experience in:

- Large Language Models (LLMs)
- Prompt Engineering
- Zero-shot and Few-shot Prompting
- Sentiment Analysis
- Aspect-Based Sentiment Analysis
- Structured JSON generation
- LLM output parsing and validation
- Quantized model loading
- Hugging Face Transformers
- GPU-based inference
- Model evaluation using Accuracy and Macro-F1
- Error analysis

---

## 📜 License

This project is created for **educational purposes**.

The Mistral-7B-Instruct-v0.1 model is released under the **Apache 2.0 License**.

---

## 👨‍💻 Project Summary

**Restaurant Review Sentiment Analysis using LLM Prompt Engineering**

> An end-to-end LLM-based system that transforms raw restaurant reviews into structured sentiment insights, aspect-level feedback, key customer preferences, and automated responses using **Mistral-7B-Instruct-v0.1 and prompt engineering**.
