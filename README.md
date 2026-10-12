# TheBugBusters (Backend)

*This repository contains the **Backend** source code and logic for TheBugBusters project, developed for the Dedalus Datathon 2025. The user interface, including the patient list and the chat view, can be found in the [Frontend Repository](https://github.com/Lustom/TheBugBustersFrontend).*

## Overview
This backend service was designed to power the clinical assistant application developed during the **Dedalus Datathon 2025**. It acts as the core engine, securely handling patient data retrieval, processing natural language queries, and dynamically generating both contextual responses and structured data representations (tables and charts).

A robust REST API was built to bridge the gap between the frontend interface and the underlying database, utilizing advanced AI models to translate medical staff inquiries into accurate SQL queries while maintaining strict domain boundaries.

## Core Architecture & Challenges Addressed

### 1. Data Engineering & Synthetic Augmentation
The original database provided for the challenge was significantly incomplete and contained noisy, unstructured data. To ensure a robust testing and production environment, a comprehensive data preprocessing pipeline was executed. The database was rigorously cleaned, and missing information was algorithmically augmented with realistic synthetic data. This resulted in a reliable and comprehensive SQLite database (`database.db`) containing patient records, lab results, medications, and clinical notes.

### 2. Prompt Engineering & Domain Restriction (Guardrails)
A specialized medical chatbot was developed using a provided LLM API key (via LiteLLM / Claude 3.5 Sonnet). A major challenge involved constraining the model's behavior to prevent hallucinations and out-of-domain responses. Strict prompt engineering techniques and guardrails were applied to ensure the model strictly answered medical-related queries. The system architecture dynamically injects context based on the specific patient selected, forcing the model to base its answers exclusively on the retrieved clinical data rather than external general knowledge.

### 3. Dynamic Text-to-SQL Pipeline
To allow doctors to query the database using natural language (e.g., *"Show me a graph of the patient's glucose levels over the last week"*), a Text-to-SQL engine was implemented. The system dynamically reads the database schema and generates syntactically valid SQLite queries. It includes automated error handling and fallback mechanisms to automatically adjust queries if incompatible functions (like string aggregation issues in SQLite) or missing columns are detected during execution.

### 4. Automated Clinical Reporting
An integrated reporting module (`FPDF`) was developed to automatically parse the ongoing conversation and patient data, generating downloadable PDF files. This allows medical staff to instantly export updated clinical summaries and conversation histories for physical records.

## Technologies Used
* **Python** (Core Logic)
* **Flask & Flask-CORS** (REST API Framework)
* **SQLite3** (Relational Database)
* **LiteLLM / OpenAI API** (Natural Language Processing & Text-to-SQL)
* **Prompt Engineering** (Context injection & model guardrails)
* **FPDF** (PDF Document Generation)

## Local Setup & Installation

To run this backend server locally:

1. **Clone the repository** and navigate to the backend directory.
2. **Install the required dependencies:**
   ```bash
   pip install flask flask-cors openai python-dotenv fpdf
   ```
3. **Environment Configuration:**
   Create a `.env` file in the root directory and add the API key:
   ```env
   API_KEY="your_api_key_here"
   ```
   *(Note: The `BASE_URL` is configured to use the Dedalus proxy environment).*
4. **Run the Server:**
   Execute the main script to start the Flask server on port 5000:
   ```bash
   python backend.py
   ```

<img width="1391" height="777" alt="THE BUG BUSTERS" src="https://github.com/user-attachments/assets/d355973b-4dfb-465f-a174-fadaa95923ec" />
