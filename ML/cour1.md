

# 🧠 Data, Information, Knowledge, and Data Mining Overview

## **1. Data vs Information vs Knowledge**
| Concept         | Definition                                           | Example in ML                                          |
| --------------- | ---------------------------------------------------- | ------------------------------------------------------ |
| **Data**        | Raw, unprocessed facts or measurements               | A CSV file of student grades and attendance            |
| **Information** | Processed or organized data that has meaning         | The average grade per student                          |
| **Knowledge**   | Insights or understanding extracted from information | Discovering that consistent attendance improves grades |

**In ML:**

* Data → what the model learns from.
* Information → what we extract during preprocessing or analysis.
* Knowledge → what the model helps us understand (patterns, trends, decisions).

---

## **2. Data Mining: Definition and Application Domains**

* **Definition:**
  Data mining is the process of discovering patterns, correlations, or useful information from large datasets using statistical and machine learning techniques.

---

## **3. The CRISP-DM Process**

CRISP-DM stands for **Cross-Industry Standard Process for Data Mining** — a structured approach with six stages:

1. **Business Understanding**
   Define the goal and success criteria of the project.
   *Example: Predict which students are at risk of failing.*

2. **Data Understanding**
   Collect and explore data to understand its content, structure, and quality.
   *Example: Identify missing or inconsistent student records.*

3. **Data Preparation**
   Clean, transform, and format data for modeling.
   *Example: Handle missing grades, normalize attendance values.*

4. **Modeling**
   Choose and train algorithms (e.g., decision trees, neural networks).
   *Example: Train a classifier to predict pass/fail.*

5. **Evaluation**
   Assess model performance and whether it meets business goals.
   *Example: Accuracy, recall, precision of predictions.*

6. **Deployment**
   Implement the model in real-world use (dashboard, app, etc.).
   *Example: Automated alerts for struggling students.*



✅ **Summary Table**

| Step                   | Main Goal                | Example                            |
| ---------------------- | ------------------------ | ---------------------------------- |
| Business Understanding | Define the problem       | Predict failing students           |
| Data Understanding     | Explore and check data   | Find missing/invalid grades        |
| Data Preparation       | Clean and format data    | Normalize and fill gaps            |
| Modeling               | Train ML model           | Decision Tree, Logistic Regression |
| Evaluation             | Measure model success    | Accuracy, recall, precision        |
| Deployment             | Apply model in real life | Teacher alert dashboard            |



---

## **4. Dataset Structure and Characteristics**

### **Key Characteristics:**

| Characteristic                           | Description                                       | Why It Matters                                |
| ---------------------------------------- | ------------------------------------------------- | --------------------------------------------- |
| **Relevance**                            | Data should reflect the problem to be solved      | Irrelevant data → misleading patterns         |
| **Diversity / Variability / Complexity** | Data should include different scenarios and cases | Prevents overfitting, improves generalization |
| **Quality**                              | Data must be clean, accurate, and complete        | Errors or outliers reduce model accuracy      |
| **Size**                                 | Dataset must be large enough for learning         | Small datasets → weak models                  |
| **Consistency**                          | Uniform scales, formats, and meanings             | Inconsistency introduces bias/errors          |
| **Balance**                              | Equal distribution across classes                 | Prevents bias toward majority class           |
| **Labeling**                             | Correct and precise labels                        | Wrong labels confuse the model                |
| **Timeliness**                           | Data should be up-to-date                         | Outdated data → poor predictions              |

---

# 🧠 Machine Learning — Study Notes

## **1. Definition**

> Machine Learning (ML) is a branch of Artificial Intelligence (AI) that enables computers to **learn from data** and make **predictions or decisions** without being explicitly programmed.

🧩 **Idea:**
Traditional programming = rules → results
Machine Learning = data + results → rules

---

## **2. Main Types of Machine Learning**

| Type                         | Data Type           | Has Labels? | Goal                  |
| ---------------------------- | ------------------- | ----------- | --------------------- |
| **Supervised Learning**      | Labeled data        | ✅ Yes       | Predict or classify   |
| **Unsupervised Learning**    | Unlabeled data      | ❌ No        | Find hidden patterns  |
| **Semi-Supervised Learning** | Partly labeled data | ⚙️ Some     | Learn from few labels |

---

## **3. Supervised Learning**

### 🔸 **Classification**

* Predict **categories** or **labels**
* 📘 *Examples:* Spam detection, disease type, student pass/fail
* 🔢 **Output:** Discrete (finite classes)

### 🔹 **Regression**

* Predict **continuous values**
* 📘 *Examples:* House price, temperature, exam score
* 🔢 **Output:** Continuous (real numbers)

---

## **4. Unsupervised Learning**

### 🔸 **Clustering**

* Group similar data points
* 📘 *Examples:* Grouping customers by purchase behavior
* 🔍 **Goal:** Discover natural groupings

### 🔹 **Association**

* Find **relationships or rules** between variables
* 📘 *Examples:* “If customer buys bread → likely buys butter”
* 🧠 **Goal:** Generate *if–then* rules

---

## **5. Semi-Supervised Learning**

* Uses **a small labeled** dataset + **a large unlabeled** dataset
* 📘 *Example:* Medical image classification (few labeled scans)
* ⚙️ **Goal:** Improve accuracy without labeling all data manually

---

## **6. Lazy Learners vs. Eager Learners (Classification)**

| Type               | Description                 | Example Algorithms         | Pros             | Cons            |
| ------------------ | --------------------------- | -------------------------- | ---------------- | --------------- |
| **Eager Learners** | Build model during training | Decision Tree, Naïve Bayes | Fast prediction  | Slow training   |
| **Lazy Learners**  | Wait until prediction time  | KNN                        | Simple, flexible | Slow prediction |

### 💡 Quick idea:

* **Eager** = learn early, predict fast
* **Lazy** = learn late, predict slow

---

## **7. Choosing the Right ML Algorithm**

| Situation / Data Characteristic | Algorithm             | Notes                       |
| ------------------------------- | --------------------- | --------------------------- |
| One dominant feature            | **OneRule**           | Simple and interpretable    |
| Independent features            | **Naïve Bayes**       | Probabilistic model         |
| Few representative features     | **Decision Tree**     | Splits data hierarchically  |
| Few decision rules              | **Prism**             | Generates if–then rules     |
| Attributes depend on each other | **Association Rules** | Finds hidden relations      |
| Linear relation (numeric)       | **Linear Regression** | Predicts continuous output  |
| Class based on similarity       | **KNN**               | Lazy learner, uses distance |
| No labels, natural groups       | **Clustering**        | Unsupervised, e.g. K-Means  |



---

## ✅ **Mini Summary**

| Concept              | Meaning                                      |
| -------------------- | -------------------------------------------- |
| **Classification**   | Predicts a category                          |
| **Regression**       | Predicts a continuous number                 |
| **Clustering**       | Finds natural groups                         |
| **Association**      | Finds “if–then” relationships                |
| **Lazy Learner**     | Learns at prediction time (e.g., KNN)        |
| **Eager Learner**    | Learns during training (e.g., Decision Tree) |
| **Algorithm Choice** | Depends on data nature and relationships     |

---
