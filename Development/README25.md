To bring the visual element of your Shop Oahu and Phoenix Valley projects to life, we’ll implement an image pipeline. This allows you to display storefront photos, menus, or regional "Pokemon-style" environmental captures directly in the Android app.
Since you are used to the flexibility of the digital medium, we will use Coil for the Android side (which stands for Coroutine Image Loader) and FastAPI’s StaticFiles for the backend.
1. The Backend: Serving Storefront Images
FastAPI can serve a folder of images as "Static Files." This is efficient and allows your Android app to access any image via a simple URL.

```
from fastapi import FastAPI
from fastapi.staticfiles import StaticFiles
import os

app = FastAPI()

# Create the directory if it doesn't exist
if not os.path.exists("static/images"):
    os.makedirs("static/images")

# 'Mount' the folder so it's accessible at /static/images
app.mount("/static", StaticFiles(directory="static"), name="static")

# Your Business model now includes an image_url
# Example: http://127.0.0.1:8000/static/images/waikiki_surf.jpg
```

2. The Android Client: Displaying with Coil
Coil is built specifically for Kotlin and Jetpack Compose. It handles memory caching and image fetching without blocking the UI thread—keeping that "Quantum Paper" feel smooth.
Add the dependency to app/build.gradle.kts:
implementation("io.coil-kt:coil-compose:2.6.0")

The Composable Image (AsyncImage):
We'll use AsyncImage inside your Bottom Sheet to show the business photo when a marker is tapped.

```
@Composable
fun BusinessDetailContent(business: BusinessItem) {
    Column(modifier = Modifier.padding(16.dp)) {
        // Coil's AsyncImage handles the URL loading automatically
        AsyncImage(
            model = "http://your-api-ip:8000/static/images/${business.id}.jpg",
            contentDescription = "Photo of ${business.name}",
            modifier = Modifier
                .fillMaxWidth()
                .height(200.dp)
                .clip(RoundedCornerShape(12.dp)),
            contentScale = ContentScale.Crop,
            placeholder = painterResource(R.drawable.placeholder_loading),
            error = painterResource(R.drawable.placeholder_error)
        )
        
        Text(text = business.name, style = MaterialTheme.typography.headlineSmall)
        // ... rest of your details
    }
}
```

3. The "Quantum" Visual Polish
Drawing from your SCAD design background, you can add a "Crossfade" transition. This makes the image appear to materialize on the "paper" rather than just popping in, which feels much more like the premium Material Design aesthetic.

```
AsyncImage(
    model = ImageRequest.Builder(LocalContext.current)
        .data(imageUrl)
        .crossfade(true) // Smooth 200ms transition
        .build(),
    contentDescription = null
)
```

Why this setup is robust:
 * Automatic Caching: Coil automatically caches images in the phone's storage. If a user in Sun Valley loses internet after viewing a shop once, the image will still be there in the "Offline Mode" we built.
 * Low Memory Footprint: It only loads the resolution needed for the screen, which is a lesson learned from the resource-constrained days of the Apple Newton.
 * Dynamic Content: If you update a photo in your static/images folder via your Python bulk-import script, the Android app will show the new photo the next time the user clicks that marker.
Would you like to see how to add an "Upload" feature? This would let you take a photo with your Android camera in the field and save it directly to your Python backend, effectively "documenting" your projects in real-time.
Displaying Images from the Internet
This video provides a practical guide on using the Coil library within Jetpack Compose to load and display images from a URL, which is exactly how your Android client will fetch business photos from your FastAPI backend.

YouTube video views will be stored in your YouTube History, and your data will be stored and used by YouTube according to its Terms of Service
