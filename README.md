# 🚗 Car Price Prediction App

A machine learning web application that predicts the resale price of used cars based on key vehicle attributes. Built with **Python**, trained using a **Random Forest** model, and deployed via **Streamlit** and **Render**.

🔗 **Live Demo**: [car-price-prediction-7jfg7ev2srti242vtpicx5.streamlit.app](https://car-price-prediction-7jfg7ev2srti242vtpicx5.streamlit.app/)

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Dataset](#dataset)
- [Model](#model)
- [Installation & Setup](#installation--setup)
- [Usage](#usage)
- [Deployment](#deployment)
- [Screenshots](#screenshots)
- [Contributing](#contributing)
- [License](#license)

---

## 🧠 Overview

This app allows users to input details about a used car — such as brand, year, fuel type, transmission, and kilometers driven — and instantly receive a predicted resale price powered by a trained Random Forest regression model.

The model was trained on the **CarDekho dataset**, which contains thousands of real used-car listings from across India.

---

## ✨ Features

- 🔍 Predict used car prices in real time
- 🎛️ Interactive UI built with Streamlit
- 🌲 Random Forest model with high accuracy
- 📊 Data preprocessing and feature encoding pipeline
- ☁️ Fully deployed and accessible via browser
- 🐳 Dev container support for consistent development environments

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Language | Python 3.x |
| ML Model | Scikit-learn (Random Forest Regressor) |
| Backend API | FastAPI |
| Frontend | Streamlit |
| Deployment | Streamlit Cloud / Render |
| Data | CarDekho Dataset (CSV) |
| Serialization | Pickle (`.pkl`) |

---

## 📁 Project Structure

```
car-price-prediction/
│
├── .devcontainer/              # Dev container configuration
├── __pycache__/                # Python cache files
│
├── cardekho_data (1).csv       # Raw dataset
├── feature_columns.pkl         # Saved feature column names
├── random_forest_model.pkl     # Trained Random Forest model
│
├── train.py                    # Model training script
├── model.py                    # Model loading and prediction logic
├── schema.py                   # Pydantic schema for FastAPI request/response
├── main.py                     # FastAPI backend — exposes /predict endpoint
├── streamlit_app.py            # Streamlit frontend UI (calls FastAPI)
│
├── requirements.txt            # Python dependencies
├── runtime.txt                 # Python version specification
├── .python-version             # Python version (for pyenv)
│
└── README.md                   # Project documentation
```

---

## 📦 Dataset

The app uses the **CarDekho dataset** (`cardekho_data (1).csv`), which includes the following features:

| Feature | Description |
|---|---|
| `Car_Name` | Name/brand of the car |
| `Year` | Year of manufacture |
| `Selling_Price` | Price the owner wants to sell at (target variable) |
| `Present_Price` | Current ex-showroom price |
| `Kms_Driven` | Total kilometers driven |
| `Fuel_Type` | Petrol / Diesel / CNG |
| `Seller_Type` | Dealer or Individual |
| `Transmission` | Manual or Automatic |
| `Owner` | Number of previous owners |

---

## 🌲 Model

The prediction model is a **Random Forest Regressor** trained using Scikit-learn.

**Training pipeline:**
1. Load and clean the CarDekho dataset
2. Encode categorical variables
3. Split data into train/test sets
4. Train a `RandomForestRegressor`
5. Evaluate using RMSE and R² score
6. Serialize the model using `pickle` → `random_forest_model.pkl`
7. Save feature column names → `feature_columns.pkl`

To retrain the model:
```bash
python train.py
```

---

## ⚡ FastAPI Backend

The app uses **FastAPI** as the backend API layer. The Streamlit frontend sends user inputs to the FastAPI `/predict` endpoint, which runs the model and returns the predicted price.

### Architecture Flow

```
User (Browser)
     │
     ▼
Streamlit Frontend (streamlit_app.py)
     │  HTTP POST /predict
     ▼
FastAPI Backend (main.py)
     │  loads model & features
     ▼
Random Forest Model (random_forest_model.pkl)
     │  returns prediction
     ▼
Streamlit displays the result
```

### API Endpoint

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/` | Health check — confirms API is running |
| `POST` | `/predict` | Accepts car details, returns predicted price |

### Request Schema (`schema.py`)

The request body is validated using **Pydantic**:

```python
{
  "Car_Name": "honda city",
  "Year": 2017,
  "Present_Price": 8.5,
  "Kms_Driven": 35000,
  "Fuel_Type": "Petrol",
  "Seller_Type": "Individual",
  "Transmission": "Manual",
  "Owner": 0
}
```

### Sample Response

```json
{
  "predicted_price": 5.43
}
```

### Run the FastAPI Server Locally

```bash
uvicorn main:app --reload
```

Visit the interactive API docs at: `http://127.0.0.1:8000/docs`

---

## ⚙️ Installation & Setup

### Prerequisites

- Python 3.9+
- pip

### 1. Clone the Repository

```bash
git clone https://github.com/Prince-05/car-price-prediction.git
cd car-price-prediction
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. (Optional) Use Dev Container

If you're using VS Code with the Dev Containers extension:

1. Open the project in VS Code
2. Click **"Reopen in Container"** when prompted
3. The environment will be automatically configured

### 4. Run the FastAPI Backend

```bash
uvicorn main:app --reload
```

API will be live at `http://127.0.0.1:8000`
Interactive docs at `http://127.0.0.1:8000/docs`

### 5. Run the Streamlit Frontend

```bash
streamlit run streamlit_app.py
```

Open your browser and visit: `http://localhost:8501`

---

## 🖥️ Usage

1. Open the app in your browser
2. Enter the car details:
   - Car name / brand
   - Year of manufacture
   - Fuel type (Petrol / Diesel / CNG)
   - Transmission type (Manual / Automatic)
   - Kilometers driven
   - Number of previous owners
   - Present market price
3. Click **"Predict Price"**
4. View the estimated resale value instantly

---

## 🚀 Deployment

This app is deployed on **Streamlit Community Cloud**.

### Deploy to Streamlit Cloud

1. Push your code to GitHub
2. Go to [streamlit.io/cloud](https://streamlit.io/cloud) and sign in
3. Click **"New App"** → select your repository
4. Set the main file to `streamlit_app.py`
5. Click **Deploy**

### Deploy to Render (FastAPI Backend)

1. Create a new **Web Service** on [render.com](https://render.com)
2. Connect your GitHub repository
3. Set the **Start Command**:
   ```bash
   uvicorn main:app --host 0.0.0.0 --port $PORT
   ```
4. Set environment to **Python** and deploy
5. Copy the Render service URL and update `streamlit_app.py` to point to it for API calls

---

## 🤝 Contributing

Contributions are welcome! To contribute:

1. Fork this repository
2. Create a new branch: `git checkout -b feature/your-feature-name`
3. Make your changes and commit: `git commit -m "Add your feature"`
4. Push to your branch: `git push origin feature/your-feature-name`
5. Open a Pull Request

---

## 👤 Author

**Prince-05**
- GitHub: [@Prince-05](https://github.com/Prince-05)

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

> ⭐ If you found this project helpful, consider giving it a star on GitHub!
