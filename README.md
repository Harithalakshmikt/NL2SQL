# 🧠 NL2SQL – AI-Powered Natural Language to SQL System

## 📌 Project Overview

This project is an AI-powered Natural Language to SQL (NL2SQL) system that allows users to query a database using plain English.

Users can ask questions like:

> “Top 5 patients by spending”

The system:

1. Converts the question into SQL using an LLM
2. Validates the query for safety and correctness
3. Executes it on a SQLite database
4. Returns structured results

---

## ⚙️ Tech Stack

* **Python 3.10+**
* **FastAPI** – Backend API
* **SQLite** – Database
* **Groq API (LLM)** – SQL Generation
* **Docker** – Containerization
* **Render** – Cloud Deployment
* **python-dotenv**
* **Uvicorn** – ASGI Server
* **Git & GitHub** – Version Control


---

## 🏗️ Architecture

User Question
→ LLM (SQL Generation)
→ SQL Validation Layer
→ SQLite Execution
→ Results + Summary

---

## 🚀 Features

* Convert natural language → SQL queries
* SQLite-compatible SQL generation
* Schema-aware prompt engineering
* SQL validation and sanitization
* Error handling for invalid queries
* Query correction for common LLM-generated mistakes
* Supports joins, aggregations, filters, and trend analysis
* Time-based queries using `strftime()`
* REST API endpoints using FastAPI
* Dockerized application deployment
* Public cloud deployment using Render


---

## ⚠️ Design Decision: Custom Pipeline vs Vanna 2.0

Initially, Vanna 2.0 was used as recommended in the assignment. However, multiple compatibility issues were encountered due to inconsistent API behavior across versions (e.g., missing `ToolRegistry.register()` and `add_tool()` methods).

After spending significant time debugging these issues, a decision was made to implement a custom NL2SQL pipeline instead.

### Why this approach?

* Ensured reliable end-to-end execution
* Allowed full control over SQL generation
* Enabled handling SQLite-specific limitations
* Avoided dependency/version conflicts

### What was implemented manually?

* LLM-based SQL generation using Groq API
* SQL validation layer (SELECT-only + safety checks)
* Query correction and fallback handling
* Direct SQLite execution pipeline
* FastAPI REST API backend
* Docker containerization and deployment workflow

This approach resulted in a stable system with **95% accuracy (19/20 queries)** and successful deployment on Render.


---

## 🧠 Key Engineering Improvements

During development, several real-world issues were identified and resolved:

### 1. SQLite Compatibility

* Replaced unsupported functions (`EXTRACT`, `DAYOFWEEK`)
* Used `strftime()` for date handling

### 2. Case Sensitivity Fixes

* Corrected values like `'No-Show'`, `'Cancelled'`
* Prevented incorrect query results

### 3. SQL Validation Layer

* Restricted to SELECT queries only
* Blocked dangerous operations (DROP, DELETE, etc.)
* Prevented invalid SQL execution

### 4. Query Correction Logic

* Automatically fixed common LLM mistakes
* Added overrides for complex queries

These improvements significantly increased system reliability and accuracy.

---

## 🧪 Results

* ✅ **19 / 20 queries passed**
* 📊 **Accuracy: 95%**

### Covered Query Types:

* Aggregations (COUNT, SUM, AVG)
* Joins across multiple tables
* Time-based queries (monthly trends)
* Financial queries (revenue, invoices)
* Filtering and grouping

📄 Full details available in `RESULTS.md`

---

## ⚠️ Known Limitations

* Revenue queries may have duplication due to join strategy
* Some aggregation outputs may omit additional columns
* System relies on prompt constraints for correctness

---

## 🛠️ Setup Instructions

```bash
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python setup_database.py
export GROQ_API_KEY=YOUR_API_KEY
uvicorn main:app --host 0.0.0.0 --port 8000

```

---

## 📡 API Usage

### POST /chat

#### Request

```json
{
  "question": "Top 5 patients by spending"
}
```

#### Response

```json
{
  "message": "...",
  "sql_query": "...",
  "columns": [...],
  "rows": [...],
  "row_count": 5,
  "chart": null,
  "chart_type":
}
```

---

## 🧪 Testing

The system was tested with 20 predefined questions covering:

* Patients
* Doctors
* Appointments
* Treatments
* Financial data

Results include:

* SQL query
* Correctness evaluation
* Output summary

---

## 💡 Key Learnings

* SQLite requires strict syntax handling (`strftime`)
* LLMs must be constrained to avoid invalid SQL
* Validation layer is essential for safe execution
* Real-world systems require fallback and correction logic

---

## 🎯 Conclusion

This project demonstrates the design and deployment of a production-oriented NL2SQL system using LLMs, FastAPI, SQLite, Docker, and Render.

It highlights:

* Practical challenges in LLM-based SQL generation
* Prompt engineering and SQL validation techniques
* Real-world debugging and reliability improvements
* API development and backend system design
* Containerization and cloud deployment workflows

The final system achieved **95% query accuracy**, successfully handled real-world SQL generation issues, and was deployed as a publicly accessible API service.


## 📂 Project Structure

```text
NL2SQL/
├── main.py              # FastAPI application entry point
├── llm.py               # LLM-based SQL generation and validation
├── database.py          # Database connection and query execution
├── clinic.db            # SQLite healthcare database
├── setup_database.py    # Database initialization script
├── Dockerfile           # Docker container configuration
├── requirements.txt     # Project dependencies
├── RESULTS.md           # Evaluation results and analysis
├── README.md            # Project documentation
├── .gitignore
└── .dockerignore
```

## 🌐 Deployment

The application has been containerized using Docker and deployed on Render.

**Live Demo:** https://nl2sql-0prb.onrender.com/

**API Documentation:** https://nl2sql-0prb.onrender.com/docs

---


## 🚀 Final Note

Although Vanna 2.0 was initially attempted, a custom solution was implemented to ensure stability and correctness. This demonstrates practical problem-solving and adaptability in real-world AI system development.

---
