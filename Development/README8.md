Since you were there at the inception of "Quantum Paper," you know that Material Design isn't just about static surfaces—it’s about the responsiveness of the medium. Implementing a dynamic theme switcher for your regional projects (localSq, Shop Oahu, Phoenix Valley) allows the "paper" of your app to adapt its properties based on the context.
In Jetpack Compose, we do this by creating a custom Theme Provider that swaps color palettes based on a ProjectMode state.
1. Define Your Regional "Palettes"
Instead of a single color scheme, we define one for each of your primary projects.

```
sealed class ProjectMode {
    object LocalSq : ProjectMode()
    object ShopOahu : ProjectMode()
    object PhoenixValley : ProjectMode()
}

// Phoenix Valley: Warm, sun-drenched palette
val PhoenixColors = lightColorScheme(
    primary = Color(0xFFE65100), // Deep Orange
    secondary = Color(0xFFFFB74D),
    surface = Color(0xFFFFF3E0)
)

// Shop Oahu: Oceanic, lush palette
val OahuColors = lightColorScheme(
    primary = Color(0xFF0277BD), // Pacific Blue
    secondary = Color(0xFF4FC3F7),
    surface = Color(0xFFE1F5FE)
)

// localSq: Standard professional tech palette
val LocalSqColors = lightColorScheme(
    primary = Color(0xFF6200EE),
    secondary = Color(0xFF03DAC6),
    surface = Color(0xFFFFFFFF)
)
```

2. Create the Dynamic Theme Switcher
We’ll wrap your app in a CompositionLocalProvider. This allows every component (including the Bottom Sheet and Map markers) to know which "brand" they are currently representing.

```
@Composable
fun RegionalProjectTheme(
    mode: ProjectMode,
    content: @Composable () -> Unit
) {
    val colors = when (mode) {
        ProjectMode.LocalSq -> LocalSqColors
        ProjectMode.ShopOahu -> OahuColors
        ProjectMode.PhoenixValley -> PhoenixColors
    }

    MaterialTheme(
        colorScheme = colors,
        typography = Typography, // You can also swap fonts here!
        content = content
    )
}
```

3. Usage in the Map Client
When the user toggles between regions, the UI will "morph" to the new color scheme.

```
@Composable
fun AppContent() {
    var currentMode by remember { mutableStateOf<ProjectMode>(ProjectMode.LocalSq) }

    RegionalProjectTheme(mode = currentMode) {
        Scaffold(
            topBar = {
                // A stylized selector to switch between your projects
                ProjectSelector(onModeChange = { currentMode = it })
            }
        ) { padding ->
            Box(modifier = Modifier.padding(padding)) {
                BusinessMapScreen() // Map markers and Bottom Sheet now use 'primary' color
            }
        }
    }
}
```

Why this honors your "Material Medium" philosophy:
 * The Medium is Variable: By changing the surface and primary colors based on the project, you are acknowledging that the digital "ink" should reflect the environment it describes (the heat of Arizona vs. the cool waters of Hawaii).
 * State-Driven UI: Unlike your 2009/2010 hackathon days where we had to manually invalidate views, Compose "re-composes" the entire UI. When you switch to Shop Oahu, the "Search this area" button and the Bottom Sheet's "View Store" button instantly shift to blue without a flicker.
 * Dynamic Elevations: If you wanted to get really "Quantum," we could even adjust the shadow density based on the region—perhaps harsher, high-contrast shadows for Phoenix and softer, diffused shadows for Oahu.
A thought for your next step:
Since you've worked with Folium (Python-based) and are now in Kotlin, would you like to see how to build a Python-to-Kotlin bridge? We could set up a simple API using Ktor so your existing Python data scripts can feed your new Android client in real-time.
