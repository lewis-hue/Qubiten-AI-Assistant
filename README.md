# Production-Grade Serverless RAG Application: A Full-Stack LLM System for Secure Document Analysis and Constitutional Compliance

**Qubiten AI** is a full-stack, production-grade chatbot application designed for intelligent document analysis and constitutional compliance. It leverages a Retrieval-Augmented Generation (RAG) system powered by the Groq LLaMA 3.1 LLM to provide context-aware, accurate, and streaming answers based on the Constitution of Kenya, 2010.

The application features a modern web interface with client-side document processing, voice-to-text transcription via Google Cloud Speech-to-Text, and is architected for stateless, scalable deployment on Google Cloud Run using Docker.


---

## 📋 Project Overview

The Qubiten AI Assistant is an expert AI consultant specializing in legal and compliance document analysis. **The system's core function is to empower users by not only providing answers from the constitution but by actively analyzing their queries to deliver clear, actionable guidance on the specific compliance obligations they need to meet.** Users can ask questions directly, upload documents with their queries for detailed analysis, or use their voice for transcription. The system is designed with a **stateless architecture** and a smart **client-side data processing** model, optimizing for security, performance, and cost-efficiency in a production environment.

## Chatbot Live Demo

<video src="https://github.com/lewis-hue/Qubiten-AI-Assistant/blob/main/chatbot%20Live%20Demo.mp4?raw=true" width="100%" controls="controls">
  Your browser does not support the video tag. You can watch the demo directly on <a href="https://github.com/lewis-hue/Qubiten-AI-Assistant/blob/main/chatbot%20Live%20Demo.mp4">GitHub</a>.
</video>


## ✨ Core Features

*   **Proactive Compliance Advisory:** Translates complex constitutional law into actionable compliance requirements tailored to a user's specific sector.
*   **Intelligent Document Analysis:** Supports multiple document formats (`PDF`, `DOCX`, `TXT`, `CSV`, `JSON`, `MD`) and extracts explicit and implicit questions.
*   **Retrieval-Augmented Generation (RAG):** Utilizes a pre-processed vector store of the **Kenyan Constitution, 2010** and dynamically incorporates user documents for citation-backed answers.
*   **Real-time Streaming Responses:** Integrates with the **Groq API** to stream responses token-by-token for an immediate and interactive experience.
*   **Voice-to-Text Transcription:** Features a voice recording module that uses **Google Cloud Speech-to-Text** for accurate and fast transcription.
*   **Secure Client-Side Data Processing:** Documents are processed and stored in the user's browser (IndexedDB/localStorage), ensuring data privacy and a stateless backend.
*   **Production-Ready Architecture:** Fully containerized with **Docker** and designed for serverless deployment on **Google Cloud Run** for automatic scaling.

## 🏗️ Technical Architecture

The application is built on a modern, decoupled architecture designed for scalability and maintainability.

1.  **Frontend (Client-Side):** A responsive single-page application built with **vanilla JavaScript, HTML5, and CSS3** that manages all user interactions, client-side storage, and text extraction from documents.
2.  **Backend (Flask Application):** A **Python Flask** server that provides a RESTful API, manages session-based rate limiting, and orchestrates the RAG workflow.
3.  **AI Core (RAG Pipeline):** The logic that retrieves relevant constitutional articles from a **FAISS vector store**, combines them with user-provided context, constructs a detailed prompt, and communicates with the **Groq API**.
4.  **Cloud Infrastructure (Google Cloud Platform):** The complete cloud-native stack, including **Container Registry, Cloud Build (CI/CD), Cloud Run (Serverless Hosting), Secret Manager,** and the **Speech-to-Text API**.

## 🛠️ Core Competencies: Skills, Tools, and Technologies

| **Category**                  | **Technologies & Tools**                                                                         | **Key Skills & Concepts Demonstrated**                                                                                                                                                                                          |
| ----------------------------- | ------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Backend Development**       | Python 3.11, Flask, Gunicorn, `python-dotenv`, `re`                                              | RESTful API Design, Serverless Architecture, Stateless Application Logic, Environment Configuration, Session-Based Rate Limiting, Asynchronous Programming                                                         |
| **Frontend Development**      | Vanilla JavaScript (ES6+), HTML5, CSS3, `FileReader` API, `MediaRecorder` API                    | DOM Manipulation, Asynchronous API Calls (Fetch), Client-Side Data Processing & Storage (IndexedDB, localStorage), Real-time UI Updates, Responsive Design, State Management                               |
| **AI & Machine Learning**     | Groq API (LLaMA 3.1), LangChain, FAISS (Vector Store), Google Cloud Speech-to-Text API            | **Retrieval-Augmented Generation (RAG)**, **Prompt Engineering**, Large Language Model (LLM) Integration, NLP for Question Extraction, Semantic Search, Vector Embeddings, Voice-to-Text Transcription |
| **DevOps & Cloud Deployment** | **Docker**, **Google Cloud Run**, Google Cloud Build, Google Container Registry, Secret Manager    | **Containerization** (Multi-stage builds), **CI/CD Automation** (`cloudbuild.yaml`), **Serverless Deployment**, Infrastructure as Code (IaC) principles, Secure Credential Management, Production Monitoring & Logging   |
| **Databases & Storage**       | **FAISS** (Vector Database), **IndexedDB** / **localStorage** (Client-Side)                           | In-memory Vector Search, Structured Client-Side Storage, Data Persistence Logic, NoSQL principles                                                                                                                 |
| **Software Engineering**      | Git, GitHub, REST APIs, JSON                                                                     | Version Control, Full-Stack System Design, Decoupled Architecture, Code Modularity, Comprehensive Error Handling, Technical Documentation (Markdown/.Rmd)                                                     |


## 🧠 System Design & Logic

### Retrieval-Augmented Generation (RAG) Flow
The chatbot's accuracy is driven by a sophisticated RAG pipeline:
1.  **Keyword Retrieval:** A user's query is first processed to identify key terms.
2.  **Vector Search:** These keywords are used to perform a semantic search against the pre-indexed FAISS vector store of the Kenyan Constitution.
3.  **Context Assembly:** The top matching document chunks (constitutional articles) are retrieved.
4.  **Prompt Engineering:** A highly detailed system prompt, the retrieved legal context, and the user's question are compiled into a final prompt that grounds the LLM, forcing it to base its answer on the provided legal text.
5.  **LLM Call & Streaming:** The prompt is sent to the Groq API. The response is streamed back through the Flask backend to the user's browser.

### Client-Side Data Handling
To ensure scalability and security, the application shifts data storage and processing to the client:
1.  **No Server Uploads:** In production, documents are not stored on the server.
2.  **Browser Processing:** The frontend JavaScript uses the `FileReader` API to extract text from user-uploaded documents directly in the browser.
3.  **Browser Storage:** The extracted text is stored in **IndexedDB**.
4.  **Data in-Request:** The relevant text is pulled from IndexedDB and sent as part of the JSON payload to the `/chat` endpoint, keeping the backend stateless and secure.

## 🚀 Production Deployment Guide

This application is designed for a seamless, secure deployment on Google Cloud Run. The deployment process involves standard DevOps practices, including API enablement, secure secret management, and automated builds.

### 1. Prerequisites & Setup
Before deployment, it is necessary to set up a Google Cloud Project with a billing account. Key APIs must be enabled, including **Cloud Run, Container Registry, Cloud Build, Secret Manager,** and the **Speech-to-Text API.**

### 2. Environment & Secret Management
For production, all sensitive credentials, particularly the `GROQ_API_KEY`, are managed securely using **Google Cloud Secret Manager**. The Cloud Run service is configured to mount these secrets as environment variables at runtime.

### 3. Containerization & Build
The application is containerized using a multi-stage `Dockerfile` that produces a minimal, secure production image. A `cloudbuild.yaml` file automates the entire process of building the Docker image, pushing it to Google Container Registry, and deploying it to Cloud Run.

### 4. Deployment to Cloud Run
The deployment is managed as a serverless service on Google Cloud Run. Key configurations include setting the instance's memory and CPU, a request timeout appropriate for streaming LLM responses (300 seconds), and defining concurrency limits to manage costs and performance.

### 5. Verification
Post-deployment, the service is verified by accessing its public URL and checking a dedicated `/health` endpoint, which confirms that the application is running and has successfully loaded its core knowledge base.

## 🔮 Future Enhancements

*   **Advanced Analytics:** Implement a dashboard to track query types, user engagement, and AI performance metrics.
*   **Batch Processing:** Allow users to upload multiple documents for a comprehensive, cross-referenced compliance audit.
*   **Custom Knowledge Bases:** Enable users to upload their own internal policy documents to create temporary, session-based knowledge bases.
*   **Multi-language Support:** Extend the UI and AI prompts to support both English and Kiswahili.
*   **Microservices Architecture:** Decouple the document processing, AI orchestration, and frontend services for enhanced scalability and maintenance.
