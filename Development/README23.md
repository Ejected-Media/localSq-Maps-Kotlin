To get your localSq backend up and running, we’ll build a "Spatial CMS" using FastAPI. This Python layer will be the brain that categorizes your businesses by region (Phoenix, Oahu, or Ketchum) and serves them to your Kotlin client.
Here is your "Getting Started" guide to setting up the data source for your "Quantum Paper" project.
1. Project Structure & Environment
Create a dedicated directory for your backend. We'll use Pydantic to define our business models, ensuring the data matches the "Medium" (your Kotlin app) perfectly.

```
mkdir localSq-backend
cd localSq-backend
python -m venv venv
source venv/bin/activate  # Or venv\Scripts\activate on Windows
pip install fastapi uvicorn pydantic
```

2. The Spatial Data Model
This model includes the "Biome" logic you presented at the Google SF GameJam. It allows the backend to tell the Android app which "Environmental Theme" to apply.

```
from fastapi import FastAPI
from pydantic import BaseModel
from typing import List, Optional

app = FastAPI(title="localSq Spatial API")

class Business(BaseModel):
    id: str
    name: str
    lat: float
    lng: float
    category: str
    region: str # "PHX", "OAHU", "SUN_VALLEY"
    biome: str   # "Desert", "Oceanic", "Alpine"
    tags: List[str]

# Mock Database (In production, you'd connect this to PostgreSQL/PostGIS)
db = [
    Business(
        id="1", name="Desert Coffee Lab", 
        lat=33.4484, lng=-112.0740, 
        category="Cafe", region="PHX", biome="Desert",
        tags=["localSq-featured", "Waymo-accessible"]
    ),
    Business(
        id="2", name="Waikiki Surf Gear", 
        lat=21.2765, lng=-157.8270, 
        category="Retail", region="OAHU", biome="Oceanic",
        tags=["Shop-Oahu"]
    )
]
```

3. The API Endpoints
These endpoints will be called by the Ktor client we built earlier.

```
@app.get("/businesses/{region}")
async def get_regional_data(region: str):
    """Returns businesses filtered by region for localized caching."""
    return [b for b in db if b.region.upper() == region.upper()]

@app.get("/environment")
async def get_environment_context(lat: float, lng: float):
    """
    The 'Pokemon' Logic: 
    Returns the biome type based on coordinates to drive AR behavior.
    """
    # Simplified logic: In reality, you'd use a GeoJSON boundary check
    if lat > 40: return {"biome": "Alpine", "theme": "Ketchum-Cool"}
    if lat < 25: return {"biome": "Oceanic", "theme": "Oahu-Lush"}
    return {"biome": "Desert", "theme": "Phoenix-Warm"}
```

4. Running the Server
Launch your backend locally to test the "Bridge":
uvicorn main:app --reload

You can now visit http://127.0.0.1:8000/docs to see the Swagger UI—a clean interface to test your "Quantum" data before the Android app even touches it.
5. Why this fits your "Newton" Heritage
 * Extensibility: Just as the Newton was designed to be a "Knowledge Navigator," this API can eventually integrate LiDAR mesh data or NASA satellite imagery without changing the Android client's core code.
 * Materiality: By serving the biome and theme from the backend, you ensure the "Material" of your app (the colors, the pulse of the LiDAR) is always driven by the actual physical location of the user.
Next Step
Now that the backend is ready to serve data, would you like to see how to write a Python script to bulk-import your existing "Shop Oahu" or "Phoenix Valley" spreadsheets directly into this API?
