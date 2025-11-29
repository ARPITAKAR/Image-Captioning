📘 Image Captioning Application — DenseNet201 + LSTM + Streamlit + Docker + AWS EC2

An end-to-end Image Captioning System that generates natural-language captions for uploaded images using DenseNet201 as a feature extractor and an LSTM decoder.
The entire application is containerized with Docker and deployed on AWS EC2.

📌 Table of Contents

Overview

Model Architecture

Training Pipeline

Inference Flow

Streamlit Frontend

Project Structure

Setup Instructions

Run Locally

Docker Build & Run

AWS EC2 Deployment

Issues & Solutions

Links

🔍 Overview

This project implements a computer vision + NLP pipeline that can look at an image and generate a meaningful caption. It combines:

DenseNet201 (pretrained on ImageNet) → extracts visual features

LSTM decoder → generates captions token-by-token

Streamlit UI → user uploads an image and sees generated captions

Docker → packaging for reproducibility

AWS EC2 → cloud deployment

🧠 Model Architecture
1️⃣ DenseNet201 Feature Extractor

Loaded DenseNet201 without its classification head

Extracted feature vector from the Global Average Pooling layer

Output embedding size: 1920

2️⃣ Caption Decoder (LSTM)

Tokenized captions using Keras Tokenizer

Generated sequences using teacher forcing

Combined:

Image embedding (Dense → Reshape)

Embedded caption tokens

LSTM(256)

Added skip-connection from image embedding to LSTM output

Final prediction layer: Dense(vocab_size, softmax)

3️⃣ Loss / Training

Categorical cross-entropy

Optimized using Adam

Custom data generator using tf.keras.utils.Sequence

⚙️ Training Pipeline
Custom Data Generator (Sequence)

Handles:

batching

tokenization

padding

generating (input sequence → next word) pairs

combining image features + partial captions

Ensures memory efficiency during training.

🚀 Inference Flow

Load:

model.keras

feature_extractor.keras

tokenizer.pkl

Preprocess uploaded image:

Resize → 224×224

Normalize

Extract 1920-dim feature vector

Start caption with "startseq"

Iterate until:

"endseq" is predicted

or max length reached

Render caption in Streamlit UI.

🎨 Streamlit Frontend

Upload an image (JPG/PNG)

Model generates caption in real time

Display image + caption

Uses Matplotlib for visualization

Clean, minimal UI

Run locally:

streamlit run densenets.py

📁 Project Structure
📦 image-captioning
├── Models/
│   ├── model.keras
│   ├── feature_extractor.keras
│   └── tokenizer.pkl
├── densenets.py        # Streamlit app file
├── requirements.txt
├── Dockerfile
├── README.md
└── utils/              # (optional helpers)

🛠️ Setup Instructions
Clone this repository
git clone <your-repo-url>
cd image-captioning

Install dependencies
pip install -r requirements.txt

Run Streamlit App
streamlit run densenets.py

🐳 Docker Build & Run
Build Image
docker build -t arpit1004/tiger .

Run Container
docker run -p 8000:8000 arpit1004/tiger

Open application:
http://localhost:8000

☁️ AWS EC2 Deployment
1️⃣ Launch EC2 Instance

OS: Ubuntu

Type: t3.small (recommended for TensorFlow)

Allow ports:

22 (SSH)

8000 (app)

2️⃣ Install Docker
sudo apt update
sudo apt install -y docker.io
sudo systemctl start docker
sudo usermod -aG docker $USER
exit


Re-login.

3️⃣ Pull Image
docker pull arpit1004/tiger

4️⃣ Run Container
docker run -p 8000:8000 arpit1004/tiger

5️⃣ Access App
http://<PUBLIC_IP>:8000
