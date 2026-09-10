AI-driven oil spill detection and vessel identification using satellite SAR imagery and AIS tracking data.

SpillTrace detects oil spills from Sentinel-1 SAR satellite imagery and correlates them with historical AIS vessel tracks to identify the vessels most likely responsible — presented through an interactive web-based GIS dashboard.

Overview

Most spill-monitoring systems stop at detection. SpillTrace closes the loop by combining:

Detection — a deep learning segmentation model that identifies oil spills in SAR imagery, distinguishing them from visually similar "look-alike" phenomena (biogenic slicks, wind shadows).
Attribution — an algorithm that correlates a detected spill's location and timing with nearby AIS vessel trajectories, ranking candidate vessels by proximity, track overlap, and suspicious behavior such as AIS signal gaps.
Visualization — an interactive dashboard displaying satellite imagery, spill boundaries, vessel tracks, and ranked attribution results.
Features
Oil spill segmentation from Sentinel-1 SAR imagery (U-Net / DeepLabV3+)
Look-alike discrimination (true spills vs. natural surface phenomena)
AIS-based vessel attribution with explainable ranking (proximity, timing, transponder gaps)
Spatial database with PostGIS for efficient geospatial querying
Interactive web GIS dashboard with live map, spill detail panel, and analytics view
Validated against publicly documented historical spill incidents
Tech Stack
Layer	Technology
Satellite data	Sentinel-1 SAR (Copernicus Data Space Ecosystem / ASF Vertex)
AIS data	AISHub / open historical AIS datasets
ML framework	PyTorch
Geospatial processing	GDAL, Rasterio, GeoPandas, Shapely
Backend	FastAPI, Celery, Redis
Database	PostgreSQL + PostGIS
Frontend	React, Leaflet / Mapbox GL JS, Recharts
Infra	Docker, Docker Compose
Project Structure
spilltrace/
├── data/                   # Raw and processed datasets (gitignored)
│   ├── raw/
│   └── processed/
├── ml/                     # Model training and evaluation
│   ├── models/
│   ├── train.py
│   ├── evaluate.py
│   └── notebooks/
├── attribution/            # Vessel attribution algorithm
│   └── attribution.py
├── backend/                # FastAPI service
│   ├── app/
│   │   ├── main.py
│   │   ├── routers/
│   │   ├── models/
│   │   └── db/
│   ├── requirements.txt
│   └── Dockerfile
├── frontend/                # React dashboard
│   ├── src/
│   ├── public/
│   └── package.json
├── docker-compose.yml
├── .env.example
└── README.md
Getting Started
Prerequisites
Python 3.10+
Node.js 18+
Docker & Docker Compose
A Copernicus Data Space Ecosystem account (dataspace.copernicus.eu)
Access to AIS data (e.g. AISHub)
Setup
Clone the repository
bash
   git clone https://github.com/<your-username>/spilltrace.git
   cd spilltrace
Set up environment variables
bash
   cp .env.example .env
   # Fill in your Copernicus/AIS credentials and database config
Start the database and Redis via Docker
bash
   docker-compose up -d db redis
Backend setup
bash
   cd backend
   python -m venv venv
   source venv/bin/activate   # or venv\Scripts\activate on Windows
   pip install -r requirements.txt
   uvicorn app.main:app --reload
Frontend setup
bash
   cd frontend
   npm install
   npm run dev
ML pipeline (optional — for retraining the detection model)
bash
   cd ml
   pip install -r requirements.txt
   python train.py --config configs/default.yaml

The dashboard will be available at http://localhost:5173 (or your configured frontend port), with the API at http://localhost:8000/docs.

Data Sources
Sentinel-1 SAR imagery: Copernicus Data Space Ecosystem / ASF Vertex
Labeled training data: Deep-SAR oil spill dataset (Kaggle)
AIS vessel data: AISHub, Danish Maritime Authority, Norwegian Coastal Administration open datasets
Model Evaluation
Metric	Score
IoU	TBD
F1-score	TBD
False positive rate (look-alikes)	TBD

(Fill in after training — see ml/evaluate.py)

Roadmap
 Data acquisition and preprocessing pipeline
 Detection model training and evaluation
 Attribution algorithm
 Backend API and database integration
 Frontend dashboard
 Validation against historical spill incidents
 Final report and documentation
License

This project is developed as part of a Final Year Project (FYP). License to be determined — MIT is a reasonable default for academic/open work.

Acknowledgements
Sentinel-1 data provided by the European Space Agency (ESA) / Copernicus Programme
AIS data sourced from open maritime data providers
