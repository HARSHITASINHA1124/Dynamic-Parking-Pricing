# Dynamic-Parking-Pricing

A real-time dynamic pricing engine that adjusts parking prices based on traffic demand, congestion, and special day factors. Includes rerouting suggestions for optimal user experience.

📌 Overview
This project simulates a smart urban parking system where prices are updated dynamically based on:

Occupancy & queue levels

Traffic congestion

Day-specific behavior (weekend/holiday)

Competitive pricing with rerouting logic

It uses Pathway for real-time data stream processing and Bokeh + Panel for dynamic visualizations.

🛠️ Tech Stack
Category	Tools / Libraries Used
Language	Python 3
Real-Time Engine	Pathway
Data Handling	Pandas, NumPy
Visualization	Bokeh, Panel
Geo-logic	Haversine Formula (Python)
Data Replay	Pathway's replay_csv for simulating streams

🧠 Architecture Diagram (Mermaid)

flowchart TD
    A[Raw CSV Dataset] --> B[Data Preprocessing (Pandas)]
    B --> C[Stream Ingestion (Pathway)]
    C --> D[Model 1: Linear Pricing]
    C --> E[Model 2: Demand-Based Pricing]
    C --> F[Model 3: Competitive Pricing + Rerouting]
    F --> G[Filtered Join with Nearby Lots]
    G --> H[Suggestion Engine via UDF]
    D --> I[Pricing Visualization (Bokeh)]
    E --> I
    H --> J[User Rerouting Decisions]
    
🔍 Project Workflow
Step 1: 📥 Dataset Preprocessing
Combine LastUpdatedDate and LastUpdatedTime into a single timestamp

Generate VehicleTypeWeight for different types of vehicles

Save a filtered dataset with only necessary features

Step 2: 🚀 Streaming Simulation
Load dataset using Pathway’s replay_csv method at 1000 rows/sec

Parse timestamps and add extra date fields

Step 3: 💰 Price Models
Model 1: Linear Pricing

price_linear = 10 + (Occupancy / Capacity) * 5
Model 2: Demand-Based Pricing
Factors in:

Occupancy

Queue length

Traffic congestion

Special day

Clamped to [5, 20] to avoid extreme pricing.

Model 3: Competitive Pricing
Joins each location with nearby others

Suggests rerouting if:

Occupancy > 95%

Nearby price < Current price

Step 4: 📍 Filtering with Haversine Logic
Although not implemented inside Pathway (as it's a pure Python function), the architecture supports distance-based rerouting in future extensions.

Step 5: 📊 Real-Time Visualization
Bokeh used for plotting dynamic price trends

Panel serves it as an interactive dashboard

📁 Folder Structure

.
├── dataset.csv
├── parking_stream.csv
├── dynamic_parking_pricing.ipynb   # Main source code
├── README.md                       # You're here!


🚀 Future Improvements
Integrate real GPS data for live Haversine-based rerouting

Add historical logging and alerts when prices exceed thresholds

Connect to external booking/payment APIs

Extend with machine learning prediction for future prices

🧾 Requirements
Install via:

pip install pathway bokeh panel
Run the script:


python dynamic_parking_pricing.py
Launch dashboard:

panel serve dynamic_parking_pricing.py
