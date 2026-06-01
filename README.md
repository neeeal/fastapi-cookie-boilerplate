# Fast Cookie Boilerplate
        FastAPI and Cookiecutter-data-science template

This repository contains a decoupled machine learning architecture. It consists of an isolated data science laboratory for model training and a lightweight FastAPI web server for production inference.

The web service is configured for seamless deployment on Render.

## 🏗️ Repository Architecture

To maintain a clean separation of concerns, this project uses two distinct environments:

* **`/data_science/`**: The experimental lab. Managed by Conda (`environment.yml`). Contains heavy tools such as Jupyter, Pandas, and Matplotlib.
* **`/` (Root)**: The production API. Managed by Pip (`requirements.txt`). Contains only the lightweight dependencies required to serve the model, such as FastAPI, Uvicorn, and Scikit-learn.
* **`/models/`**: The shared handoff folder. Final models trained in the lab are saved here for the API to load.

---

## 🧪 1. The Data Science Lab (Local Development)

All exploratory data analysis and model training happen inside the `data_science/` directory.

### Initializing the Lab Environment

Ensure Conda is installed, then run:

```bash
cd data_science
conda env create -f environment.yml
conda activate my_ml_lab
```

### Updating Lab Dependencies

If you need to install new exploratory tools such as Seaborn or XGBoost, install them while the Conda environment is active, then export the changes to track them in Git:

```bash
conda install <package_name>
conda env export --from-history > environment.yml
```

---

## 🚀 2. The FastAPI Web Server (Local Development)

The root directory handles model inference and API routing. It is deliberately kept lightweight.

### Initializing the API Environment

Open a new terminal at the root of the project. Do not use the Conda ML environment. It is recommended to use a standard Python virtual environment.

```bash
# Create and activate a virtual environment
python -m venv venv

# macOS/Linux
source venv/bin/activate

# Windows
# venv\Scripts\activate

# Install the lightweight API requirements
pip install -r requirements.txt
```

### Starting the API Locally

Once dependencies are installed and a trained model exists in the `models/` directory, start the development server:

```bash
uvicorn main:app --reload
```

The API will be available at:

```text
http://127.0.0.1:8000
```

---

## ☁️ 3. Deploy to Render

When code is pushed to GitHub, Render will automatically ignore the heavy `data_science/` folder and build the API using the root `requirements.txt`.

### Manual Deployment Steps

1. Create a new Web Service on Render.
2. Connect this repository.
3. Render will automatically detect that you are deploying a Python service and use Pip to install the root dependencies.
4. Specify the following Start Command:

```bash
uvicorn main:app --host 0.0.0.0 --port $PORT
```

5. Click **Create Web Service**.

---

## ▶️ Local Testing

To verify that the FastAPI application is working correctly in your local environment, run:

```bash
uvicorn main:app --reload
```

Then open the following URL in your browser:

```text
http://127.0.0.1:8000
```

You can also access the automatically generated API documentation at:

```text
http://127.0.0.1:8000/docs
```

---

## 🙏 Acknowledgements

Thanks to Harish for the inspiration behind a FastAPI quickstart for Render and for providing sample code that helped shape this project.
