# Text Classification API (ONNX + FastAPI + Docker)

A production-ready NLP text classification API built with:

* **FastAPI** for high-performance REST API
* **ONNX Runtime** for optimized inference
* **Docker** for containerization
* **Model versioning** support
* **Health check monitoring**
* Designed for cloud deployment (Cloud Run, Render, etc.)

---

## 🚀 Overview
This is a repository created for the development and deployment of Besafe1.0.00
It contains a scalable text classification model service that:

* Accepts raw **text input**
* Converts text → tokenized sequence
* Runs inference using an **ONNX model**
* Returns prediction + confidence score
* Supports **model versioning**
* Includes a **health check endpoint** for production monitoring

The model was originally trained using TensorFlow/Keras and converted to ONNX for efficient deployment.

---

# 📦 Project Structure

```
text-ml-api/
│
├── app/
│   ├── main.py              # FastAPI entrypoint
│   ├── model_loader.py      # Model + tokenizer loading
│   ├── schema.py            # Request/response schemas
│
├── models/
│   └── v1/
│       ├── model.onnx       # ONNX model file
│       └── tokenizer.pkl    # Saved tokenizer
│
├── requirements.txt
├── Dockerfile
├── .dockerignore
└── README.md
```

---

# 🧠 How It Works

```
User Text
   ↓
Tokenizer
   ↓
Integer Sequence
   ↓
Padding
   ↓
ONNX Runtime Inference
   ↓
Prediction + Confidence
```

---

#  Features

✅ ONNX optimized inference
✅ No TensorFlow in production
✅ Model versioning via environment variables
✅ Health check endpoint
✅ Input validation with Pydantic
✅ Docker-ready
✅ Cloud-ready

---

# 🛠 Installation (Local Development)

## 1️⃣ Clone the Repository

```bash
git clone https://github.com/Tomix-pyt/Project-Besafe.git
cd Project-Besafe
```

---

## 2️⃣ Create Virtual Environment

```bash
python -m venv venv
source venv/bin/activate  # Mac/Linux
venv\Scripts\activate     # Windows
```

---

## 3️⃣ Install Dependencies

```bash
pip install -r requirements.txt
```

---

## 4️⃣ Run the API

```bash
uvicorn app.main:app --reload
```

Open:

```
http://127.0.0.1:8000/docs
```

You’ll see interactive Swagger documentation.

---

# 🧪 API Endpoints

---

## 🏠 GET /

Basic API check.

### Response

```json
{
  "message": "Text Classification API running"
}
```

---

## 🔍 POST /predict

Classifies input text.

### Request Body

```json
{
  "text": "This is a sample sentence"
}
```

### Response

```json
{
  "prediction": 1,
  "confidence": 0.8732,
  "model_version": "v1"
}
```

* `prediction`: Binary label (0 or 1)
* `confidence`: Model probability score
* `model_version`: Active model version

---

## 🩺 GET /health

Used by cloud providers to verify the container is alive.

### Response

```json
{
  "status": "ok",
  "model_version": "v1"
}
```

If model loading fails:

```json
{
  "status": "error",
  "detail": "error message"
}
```

---

# 🔁 Model Versioning

The system supports multiple model versions.

```
models/
├── v1/
├── v2/
├── v3/
```

To switch versions:

```bash
export MODEL_VERSION=v2
```

Or in Docker:

```bash
docker run -e MODEL_VERSION=v2 -p 8000:8000 text-ml-api
```

No code changes required.

---

# 🐳 Docker Deployment

## Build Image

```bash
docker build -t text-ml-api .
```

## Run Container

```bash
docker run -p 8000:8000 text-ml-api
```

Access:

```
http://localhost:8000/docs
```

---

# ☁️ Cloud Deployment

This project is compatible with:

* Google Cloud Run
* Render
* Railway
* Heroku (Container registry)
* Any Docker-supported platform

Typical deployment steps:

1. Build Docker image
2. Push to registry
3. Deploy container
4. Set environment variable `MODEL_VERSION`

---

# 📊 Converting Model to ONNX

During training (not production):

```bash
pip install tf2onnx onnx
```

Example conversion:

```python
import tensorflow as tf
import tf2onnx
import numpy as np

model = tf.keras.models.load_model("model.h5")

spec = (tf.TensorSpec((1, 100), tf.int32, name="input"),)

tf2onnx.convert.from_keras(
    model,
    input_signature=spec,
    output_path="model.onnx"
)
```

---

# 🧱 Production Architecture

```
Client → Load Balancer → Container → FastAPI → ONNX Runtime
```

Why ONNX?

* Faster inference
* Lower memory usage
* No heavy TensorFlow runtime
* Cross-platform portability

---

# ⚙️ Environment Variables

| Variable      | Description          | Default |
| ------------- | -------------------- | ------- |
| MODEL_VERSION | Model folder to load | v1      |

---

# 🔒 Production Considerations

Recommended next steps:

* Add structured logging
* Add request rate limiting
* Add authentication
* Add CI/CD pipeline
* Add monitoring (Prometheus/Grafana)
* Store models in cloud storage instead of local disk

---

# 🧪 Testing the API

Using curl:

```bash
curl -X POST "http://localhost:8000/predict" \
     -H "Content-Type: application/json" \
     -d '{"text":"example text"}'
```

---

# 📈 Future Improvements

* Multi-class classification support
* HuggingFace transformer ONNX pipeline
* Batch inference endpoint
* Async inference
* Model metadata endpoint (`/info`)
* A/B testing between model versions

---

# 🧑‍💻 Author

Built as part of an AI Engineering production deployment pipeline.

---

# 📜 License

MIT License

---


This repository demonstrates:

* Practical ML system design
* Production deployment mindset
* Scalable model serving
* Clean API architecture

---

# 🧠 Final Notes

This project represents a real-world machine learning deployment pipeline.
It is structured to reflect how ML services are built in industry environments.

If you found this useful, consider starring the repository 

---
