# LLM Asymmetric Auditing Framework

## Overview
The LLM Asymmetric Auditing Framework is a DevSecOps tool designed to evaluate the resilience of edge-deployed Large Language Models (LLMs) against translation and obfuscation-based prompt injections. By testing payloads using techniques like Base32, Base64, ROT13, and syntactic scrambling, this framework identifies critical vulnerabilities such as "Refusal Leakage" and "Stealth Leaks." 

It utilizes an asymmetric architecture, combining a deterministic Regex Data Loss Prevention (DLP) layer with a frontier-class semantic judge to establish objective ground truth and eliminate the evaluator hallucinations common in symmetric edge auditing.

## Supported LLMs & Architecture
This framework is configured to perform an A/B test comparing a fully localized edge pipeline against a cloud-based frontier pipeline. It evaluates the models across two distinct roles: **Targets** (the models defending the secrets) and **Judges** (the automated evaluators grading the outputs).

### 1. Edge Pipeline (Local Zero-Trust)
Powered by the localized Ollama REST API.
* **Target Model:** `mistral` (Mistral 7B) — Evaluated for its vulnerability to mismatched generalization and refusal leakage.
* **Judge Model:** `llama3` (Llama 3 8B) — Used as the baseline symmetric evaluator to demonstrate the limits of edge-based AI Red Teaming (e.g., evaluator hallucinations and metric inversion).

### 2. Cloud Pipeline (Frontier Baseline)
Powered by the Google GenAI SDK.
* **Target Model:** `gemini-2.5-flash` — Evaluated as the secure, high-parameter baseline defending against obfuscation.
* **Judge Model:** `gemini-2.5-flash-lite` — Utilized as the asymmetric semantic evaluator to establish objective ground truth, successfully catching encoded "Stealth Leaks" that bypass standard regex.

## Execution Guide (Google Colab)

Follow these steps to execute the auditing pipeline using Google Colab:

### 1. Upload the Notebook
* Navigate to [colab.research.google.com](https://colab.research.google.com/). 
* Click on **File > Upload notebook** in the top menu. 
* Drag and drop your `LLM_Audit_Engine.ipynb` file or browse to select it.

### 2. Configure the Hardware Accelerator (GPU)
* Go to **Runtime > Change runtime type** in the top menu. 
* Under the "Hardware accelerator" dropdown, select **T4 GPU** (or whichever GPU tier you prefer). 
* Click **Save**.

### 3. Set Up API Keys (Colab Secrets)
* Click the **Key icon** (Secrets) on the left-hand sidebar. 
* Click **Add new secret**. 
* Enter `GOOGLE_API_KEY` key name and paste your actual API key into the value field.
* **Important:** Toggle the switch next to the key to grant the notebook access to it.

### 4. Upload External Payloads
* Colab runs on temporary virtual machines. The notebook needs to read a local `payloads.json` file, so you must upload it into the environment.
* *Note: Any files uploaded here will be deleted when the Colab session disconnects or times out.*

### 5. Execute the Pipeline
* Click the **Play button** next to a cell or press `Shift + Enter` to run them sequentially and verify outputs.
