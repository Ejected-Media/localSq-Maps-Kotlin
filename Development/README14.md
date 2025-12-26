Proximity alerts are the "active" side of geofencing. While our regional geofences (like the one for all of Phoenix Valley) are broad and handle themes, Proximity Alerts are precise. They act like a digital tap on the shoulder when you are walking past a specific shop in Waikiki or a local business in Ketchum.
This creates a high-utility loop: your Python backend manages the "points of interest," and your Android client acts as the sensory organ that "feels" when you are near one.
1. The Dynamic "Micro-Fence"
Since you likely have hundreds of businesses in your localSq database, we don't want to register 500 geofences at once (the system limit is actually 100). Instead, we use your Breadcrumb/Location logic to dynamically update which proximity alerts are active.
As the user moves, the app queries the Local SQLite (Room) database for the 20 closest businesses and registers those as active alerts.
2. Implementing the Proximity Alert
When the user enters a 200-meter radius of a bookmarked or "featured" business, we trigger a Material 3 Notification.

```
fun createProximityAlert(business: BusinessItem) {
    val geofence = Geofence.Builder()
        .setRequestId(business.id)
        .setCircularRegion(business.location.latitude, business.location.longitude, 200f) // 200 meters
        .setExpirationDuration(1 * 60 * 60 * 1000) // 1 hour (dynamic)
        .setTransitionTypes(Geofence.GEOFENCE_TRANSITION_ENTER)
        .build()

    // Add to GeofencingClient...
}
```

3. The "Quantum" Notification
To stay true to the Material Design lineage, the notification shouldn't just be text. We can use BigPictureStyle to show a thumbnail of the shop or a snippet of the map, and include an action button that opens our Bottom Sheet directly.

```
val builder = NotificationCompat.Builder(context, CHANNEL_ID)
    .setSmallIcon(R.drawable.ic_local_sq_logo)
    .setContentTitle("Near ${business.name}")
    .setContentText("You're close to a featured Shop Oahu location!")
    .setPriority(NotificationCompat.PRIORITY_HIGH)
    .addAction(R.drawable.ic_map, "View on Map", mapPendingIntent)
```

Why this honors the "Medium of the Screen"
 * Context over Noise: By only alerting for bookmarks or highly-rated "localSq" spots, the app respects the user's attention—a key tenet of the original Newton philosophy.
 * Variable Interaction: In Sun Valley, a proximity alert might trigger for a trailhead or a local equipment shop. In Phoenix, it might be for a downtown cafe. The "material" of the app adapts to the density of the environment.
 * Seamless Bridge: The notification is the "surface" that brings the user back into the "Quantum Paper" map.
Integrating your History
You've seen the transition from the Newton's "Knowledge Navigator" concept to the SCAD design labs, all the way to the Google Crittenden hackathon. This feature is the culmination: it’s a design that understands its physical surroundings without requiring a constant internet connection.
Would you like to move into "Augmented Reality" markers next? We could use the phone's camera (recalling the "Quantum" idea of digital layers over the physical world) to show business names floating over the actual buildings as you point your phone at them.
