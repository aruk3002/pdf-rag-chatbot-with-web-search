# 🚀 Deployment Guide

### Secure Deployment for the AI RAG Chatbot

This document outlines **recommended deployment strategies** for the AI RAG Chatbot, with a strong focus on **security**, **secret management**, and **cloud best practices**.
At no point are API keys or sensitive credentials committed to the repository.

---

## 🔐 Security Principles

Before deploying, ensure the following:

* API keys are **never committed** to GitHub
* Secrets are injected via **environment variables** or **secret managers**
* `.env` and `.streamlit/secrets.toml` are listed in `.gitignore`

---

## ✅ Recommended Deployment: Streamlit Cloud

Streamlit Cloud is the **simplest and safest** way to deploy this application.

---

### Step 1: Repository Preparation

Confirm sensitive files are ignored:

```bash
# Ensure secrets are not tracked
.env
.streamlit/secrets.toml
.venv/
```

Commit and push the latest code:

```bash
git add .
git commit -m "Prepare project for secure deployment"
git push origin main
```

---

### Step 2: Deploy the Application

1. Visit **[https://streamlit.io](https://streamlit.io)**
2. Click **Deploy an app**
3. Authenticate using GitHub
4. Select your repository
5. Choose:

   * **Branch:** `main`
   * **Main file:** `app.py`
6. Click **Deploy**

---

### Step 3: Configure Secrets (Required)

After deployment:

1. Open your deployed app dashboard
2. Navigate to **Settings → Secrets**
3. Add secrets in **TOML format**:

```toml
GROQ_API_KEY = "your_groq_api_key"
TAVILY_API_KEY = "your_tavily_api_key"
```

4. Click **Save**
   The app will automatically restart with secure access to the keys.

---

## 🐳 Alternative Deployment: Docker

For containerized or enterprise environments, Docker can be used.

---

### Dockerfile

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8501

CMD ["streamlit", "run", "app.py", "--server.port=8501", "--server.address=0.0.0.0"]
```

---

### Build & Run Locally

```bash
docker build -t ai-rag-chatbot .
docker run \
  -e GROQ_API_KEY="your_key" \
  -e TAVILY_API_KEY="your_key" \
  -p 8501:8501 ai-rag-chatbot
```

---

## ☁️ Deployment via Environment Variables (Any Cloud Provider)

The application supports standard environment variables:

| Variable         | Description               |
| ---------------- | ------------------------- |
| `GROQ_API_KEY`   | Groq LLM API key          |
| `TAVILY_API_KEY` | Tavily web search API key |

---

### Example: Heroku

```bash
heroku config:set GROQ_API_KEY="your_key"
heroku config:set TAVILY_API_KEY="your_key"
```

---

### Example: AWS / GCP

* Use **Secrets Manager**
* Inject values as environment variables at runtime
* Do not hardcode credentials in Docker images or code

---

## 🧪 Local Development Setup

For local testing without exposing credentials:

### 1. Create Secrets File

```bash
.streamlit/secrets.toml
```

```toml
GROQ_API_KEY = "sk-..."
TAVILY_API_KEY = "tvly-..."
```

### 2. Run the Application

```bash
streamlit run app.py
```

---

## 🔐 Security Best Practices (Mandatory)

* ✅ Never commit secrets or tokens
* ✅ Use environment-based configuration
* ✅ Rotate API keys periodically
* ✅ Use separate keys for development and production
* ✅ Avoid cloud-synced directories (e.g., OneDrive) for deployment

---

## 🧯 Troubleshooting

### Issue: **API Key Not Found**

* Verify secrets are added in Streamlit dashboard
* Ensure variable names match exactly
* Restart the application after saving secrets

---

### Issue: **ModuleNotFoundError: streamlit**

* Confirm `streamlit` is listed in `requirements.txt`
* Reinstall dependencies:

  ```bash
  pip install -r requirements.txt
  ```

---

### Issue: **Secrets Not Loading Locally**

* Confirm `.streamlit/secrets.toml` exists
* Validate TOML formatting
* Restart Streamlit:

  ```bash
  streamlit run app.py
  ```

---

## ✅ Final Notes

This deployment setup follows **industry-standard security practices** and is suitable for:

* Academic submissions
* Portfolio projects
* Production-style demos
* Cloud-hosted AI applications
