Clustering is essential for projects like Shop Oahu or Phoenix Valley, where you might have dozens of businesses in a single shopping center or downtown block. Without it, markers overlap, creating a "stack" that's impossible for users to tap individually.
In the Maps Compose library, we use the MarkerCluster composable. It automatically handles the math of grouping markers as you zoom out and expanding them as you zoom in.
1. Define Your Cluster Item
To use clustering, your business data model must implement the ClusterItem interface. This tells the library exactly where the point is and what text to show.

```
import com.google.android.gms.maps.model.LatLng
import com.google.maps.android.clustering.ClusterItem

data class BusinessItem(
    val id: String,
    val name: String,
    val location: LatLng,
    val category: String
) : ClusterItem {
    override fun getPosition(): LatLng = location
    override fun getTitle(): String = name
    override fun getSnippet(): String = category
    override fun getZIndex(): Float? = null
}
```

2. Implement the MarkerCluster
Instead of a forEach loop to manually place markers, you pass your list of businesses to the MarkerCluster component.

```
@Composable
fun ClusteredBusinessMap(businesses: List<BusinessItem>) {
    val cameraPositionState = rememberCameraPositionState()

    GoogleMap(
        modifier = Modifier.fillMaxSize(),
        cameraPositionState = cameraPositionState
    ) {
        // This single component replaces the individual Marker calls
        MarkerCluster(
            items = businesses,
            onClusterClick = { cluster ->
                // Zoom in when a cluster is clicked
                false // Return false to allow default behavior (like zooming in)
            },
            onClusterItemClick = { business ->
                // Handle individual shop selection (e.g., open localSq details)
                false
            }
        )
    }
}
```

3. Customizing Cluster Appearance
By default, the clusters are simple circles with numbers. For your branding, you might want them to match your app's color palette (e.g., a specific "localSq" blue).
You can use a clusterContent block to design the cluster look using standard Compose UI:

```
MarkerCluster(
    items = businesses,
    clusterContent = { cluster ->
        // Custom Compose UI for the cluster "bubble"
        Surface(
            modifier = Modifier.size(40.dp),
            shape = CircleShape,
            color = Color(0xFF1A73E8), // Your brand color
            contentColor = Color.White,
            border = BorderStroke(2.dp, Color.White)
        ) {
            Box(contentAlignment = Alignment.Center) {
                Text(
                    text = "${cluster.size}",
                    style = MaterialTheme.typography.labelLarge
                )
            }
        }
    }
)
```

Why this is better than Folium's MarkerCluster:
 * State Management: In Folium, updating markers usually requires a full map refresh. Here, if you filter your business list (e.g., "only show coffee shops"), the clusters animate and update instantly because of Compose's state-driven nature.
 * Touch Targets: Android's native clustering is optimized for fingers, ensuring that "cluster clicks" feel responsive and accurate.
Would you like to explore adding a "Bottom Sheet" that slides up with business details when a marker is clicked?
