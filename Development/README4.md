Building a native Android client for your projects like localSq, Shop Oahu, and Phoenix Valley is a great way to improve performance and user experience compared to a web-based Folium map.
Since we are in late 2025, we'll use the most modern approach: Kotlin, Jetpack Compose (the modern toolkit for Android UI), and the Maps Compose library.
1. Prerequisites & API Key
Before coding, you need a Google Cloud Project with the Maps SDK for Android enabled.
 * Go to the Google Cloud Console.
 * Enable the Maps SDK for Android.
 * Create an API Key and restrict it to your Android app's package name and SHA-1 fingerprint for security.
 * In your local.properties file, add:
   MAPS_API_KEY=YOUR_API_KEY_HERE
2. Project Setup
Add the dependencies to your app/build.gradle.kts file. I've included the Maps Compose library, which makes map integration much cleaner than the old Fragment-based way.

```
dependencies {
    // Google Maps for Compose
    implementation("com.google.maps.android:maps-compose:6.1.1")
    
    // Optional: Google Maps Utils (for clustering if your business lists grow)
    implementation("com.google.maps.android:maps-compose-utils:6.1.1")

    // Standard Google Play Services Maps
    implementation("com.google.android.gms:play-services-maps:19.0.0")
}
```

3. The Business Map Activity
Here is a complete, concise example of a Composable function that displays your business markers. This replaces the traditional Activity XML layout with a declarative Kotlin UI.

```
import androidx.compose.runtime.Composable
import com.google.android.gms.maps.model.CameraPosition
import com.google.android.gms.maps.model.LatLng
import com.google.maps.android.compose.*

data class Business(val name: String, val location: LatLng)

@Composable
fun BusinessMapScreen() {
    // Example data for Phoenix Valley or Shop Oahu
    val businesses = listOf(
        Business("Local Shop A", LatLng(33.4484, -112.0740)), // Phoenix example
        Business("Shop Oahu HQ", LatLng(21.3069, -157.8583))  // Honolulu example
    )

    // Initial camera position (Phoenix center)
    val cameraPositionState = rememberCameraPositionState {
        position = CameraPosition.fromLatLngZoom(businesses[0].location, 10f)
    }

    GoogleMap(
        modifier = androidx.compose.ui.Modifier.fillMaxSize(),
        cameraPositionState = cameraPositionState,
        properties = MapProperties(isMyLocationEnabled = true),
        uiSettings = MapUiSettings(zoomControlsEnabled = true)
    ) {
        // Render markers for your businesses
        businesses.forEach { business ->
            Marker(
                state = MarkerState(position = business.location),
                title = business.name,
                snippet = "Click to view details",
                onClick = {
                    // Logic for selecting a business, similar to your localSq projects
                    false
                }
            )
        }
    }
}
```

Key Improvements over Folium:
 * Performance: Native rendering is significantly smoother, especially when zooming or panning with many markers.
 * Offline Support: Native SDKs handle caching much better than web views.
 * Lifecycle Awareness: Using rememberCameraPositionState ensures the map state persists through screen rotations (e.g., switching from portrait to landscape while looking at a shop).
Would you like me to show you how to implement custom marker icons (like using a store logo) or how to add a "Search this area" button similar to Google Maps?
