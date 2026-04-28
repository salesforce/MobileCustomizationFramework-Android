# Mobile Customization Framework for Android

## 📝 Overview

The Mobile Customization Framework (MCF) for Android is a comprehensive Jetpack Compose-based framework that enables dynamic, server-driven UI rendering for Salesforce mobile applications. Built entirely with Jetpack Compose, it provides a flexible architecture for mapping JSON component definitions to native Android components while maintaining consistency with the Salesforce Lightning Design System.

### Key Features

- **🚀 Server-Driven UI**: Render dynamic interfaces from JSON configurations without app updates
- **🎨 Salesforce Design System**: All components follow Lightning Design System guidelines
- **📱 Jetpack Compose Native**: Modern Android development with declarative UI patterns
- **🔧 Extensible Architecture**: Easy to add custom components and view providers
- **🤖 AI Components**: Built-in support for AI-powered features and interactions
- **📊 Data Integration**: Seamless data binding and state management
- **🎯 Type-Safe**: Kotlin-based with strong typing and coroutines support
- **♿️ Accessibility First**: Full TalkBack support and accessibility compliance
- **🎨 Styling Hooks**: Dynamic theming with server-driven styling tokens

## 🚀 Prerequisites

- Android Studio Meerkat 2024.3.1 or newer
- Kotlin 2.1.0+
- Minimum SDK: API level 28 (Android 9)
- Jetpack Compose BOM configured in your project

## 📦 Installation

### Step 1: Add the Maven Repository

Add the MCF Maven repository to your project's `settings.gradle.kts`:

```kotlin
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()
        maven {
            url = uri("https://opensource.salesforce.com/MobileCustomizationFramework-Android/mcf-repository")
        }
    }
}
```

Or if using `settings.gradle` (Groovy):

```groovy
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()
        maven { url 'https://opensource.salesforce.com/MobileCustomizationFramework-Android/mcf-repository' }
    }
}
```

### Step 2: Add the Dependencies

Add MCF to your module's `build.gradle.kts`:

```kotlin
dependencies {
    implementation("com.salesforce.android.mobilecustomizationframework:mobile-customization-framework:<version>")
    implementation("com.salesforce.android.mobilecustomizationframework:mobile-customization-components:<version>")
}
```

Or in `build.gradle` (Groovy):

```groovy
dependencies {
    implementation 'com.salesforce.android.mobilecustomizationframework:mobile-customization-framework:<version>'
    implementation 'com.salesforce.android.mobilecustomizationframework:mobile-customization-components:<version>'
}
```

> Check the [releases](https://github.com/salesforce/MobileCustomizationFramework-Android/releases) for the latest available version.

## 🏗️ Architecture

The framework consists of three main modules:

### 📦 Mobile Customization Components
Basic Salesforce components that can be bound to data using DataProvider implementations.

### 🔧 Mobile Customization Framework
Core framework for mapping layout metadata definitions to MCF components. Includes ViewProvider interface for component mapping and base implementations for core MCF components.

## 💡 Quick Start Example

```kotlin
import androidx.compose.runtime.*
import com.salesforce.mobilecustomization.framework.*
import com.salesforce.mobilecustomization.components.*

@Composable
fun MyDynamicScreen() {
    val componentDefinition = remember {
        ComponentDefinition.fromJson(uemJsonString)
    }

    MCFView(definition = componentDefinition)
}
```

### Example Layout Metadata JSON Definition

MCF renders UI from layout metadata JSON definitions. Here is an example:

```json
{
  "view": {
    "definition": "generated/uem_ui_example",
    "properties": {},
    "regions": {
      "components": {
        "components": [
          {
            "definition": "ui/container",
            "properties": {
              "backgroundColor": "#f3f2f2",
              "padding": "16"
            },
            "regions": {
              "components": {
                "components": [
                  {
                    "definition": "ui/card",
                    "properties": {},
                    "regions": {
                      "components": {
                        "components": [
                          {
                            "definition": "ui/text",
                            "properties": {
                              "text": "Welcome to MCF",
                              "style": "titlesFontScale3Semibold",
                              "color": "$colors.onSurface1"
                            },
                            "regions": {}
                          },
                          {
                            "definition": "ui/text",
                            "properties": {
                              "text": "Build dynamic, server-driven mobile experiences.",
                              "style": "bodyFontScale2Regular",
                              "color": "$colors.onSurface2"
                            },
                            "regions": {}
                          }
                        ]
                      }
                    }
                  },
                  {
                    "definition": "ui/card",
                    "properties": {},
                    "regions": {
                      "components": {
                        "components": [
                          {
                            "definition": "ui/column",
                            "properties": {
                              "gap": "4",
                              "width": "fill"
                            },
                            "regions": {
                              "components": {
                                "components": [
                                  {
                                    "definition": "ui/button",
                                    "properties": {
                                      "label": "Edit Record",
                                      "variant": "brand",
                                      "iconName": "utility:edit",
                                      "iconPosition": "left"
                                    },
                                    "regions": {}
                                  },
                                  {
                                    "definition": "ui/button",
                                    "properties": {
                                      "label": "View Details",
                                      "variant": "neutral"
                                    },
                                    "regions": {}
                                  }
                                ]
                              }
                            }
                          }
                        ]
                      }
                    }
                  }
                ]
              }
            }
          }
        ]
      }
    }
  },
  "target": "mcf__native",
  "apiName": "uem_ui_example",
  "id": "uem_ui_example"
}
```

## 🎨 Styling Hooks Support

MCF supports comprehensive styling hooks for dynamic theming:

```kotlin
@Composable
fun CustomComponent(view: ComponentDefinition) {
    // Clean property access with automatic parsing
    val backgroundColor = view.getColor("backgroundColor")
    val gap = view.getDimension("gap")
    val padding = view.getPaddingValues("padding")

    // Use in your component
    Box(
        modifier = Modifier
            .background(backgroundColor)
            .padding(padding)
    ) {
        Column {
            Text("First item")
            Spacer(modifier = Modifier.height(gap))
            Text("Second item")
        }
    }
}
```

### Supported Styling Types

- **Colors**: `$colors.surfaceContainer1`, `#FF0000`
- **Dimensions**: `$spacing.spacing4`, `$sizing.sizing8`, `16.dp`
- **Padding**: `$spacing.spacing4`, `16.dp`, complex padding objects

### Direct StylingHooks Usage

```kotlin
// Using styling hooks
val backgroundColor = StylingHooks.parseColor(
    "$colors.surfaceContainer1",
    Color.White
)
val gap = StylingHooks.parseDpFromStylingHook(
    "$spacing.spacing4",
    8.dp
)
val padding = StylingHooks.parsePadding(
    "$spacing.spacing4",
    PaddingValues(0.dp)
)

// Using raw values
val customColor = StylingHooks.parseColor("#FF0000", Color.Black)
val customGap = StylingHooks.parseDpFromStylingHook("16.dp", 8.dp)
```

## 🔌 Extensibility

Add custom components to the framework:

```kotlin
// 1. Define your custom view provider
class MyCustomViewProvider : ViewProvider {
    override fun canHandle(definition: String): Boolean {
        return definition == "custom/myComponent"
    }

    @Composable
    override fun GetView(modifier: Modifier, view: ComponentDefinition) {
        Text(
            text = view.getString(Properties.TITLE, ""),
            modifier = modifier
        )
    }
}

// 2. Register it with ViewProviderService
val viewProviderService: ViewProviderService = ... // obtain from your service provider
viewProviderService.register(MyCustomViewProvider())

// 3. Provide it via CompositionLocal and render metadata
@Composable
fun MyMCFScreen(uemRoot: ComponentDefinition) {
    CompositionLocalProvider(
        LocalViewProvider provides viewProviderService
    ) {
        ComponentMapper(view = uemRoot)
    }
}
```

## ♿️ Accessibility

All MCF components are built with accessibility as a priority:

- **TalkBack Support**: Comprehensive screen reader support with meaningful content descriptions
- **Touch Targets**: Minimum 48dp touch targets on all interactive elements
- **Color Contrast**: WCAG AA compliant color combinations
- **State Descriptions**: Clear indication of component states
- **Keyboard Navigation**: Full keyboard navigation support
- **Dynamic Font Scaling**: Respect user's system font size preferences

## 🧪 Testing

MCF components are designed to be testable:

```kotlin
@Test
fun testMCFComponent() {
    composeTestRule.setContent {
        val definition = ComponentDefinition(
            type = "button",
            properties = mapOf("label" to "Test Button")
        )
        MCFView(definition = definition)
    }

    composeTestRule
        .onNodeWithText("Test Button")
        .assertExists()
        .performClick()
}
```

## 📄 License

MobileCustomizationFramework is available under the terms specified in the [TERMS_OF_USE.txt](TERMS_OF_USE.txt) file.

Copyright 2026 Salesforce, Inc. All rights reserved.

## 🤝 Support

For bug reports and feature requests, please use the GitHub Issues section of this repository.

For Salesforce employees: Please refer to internal documentation for contribution guidelines and development setup.

## 🔗 Related Projects

- [SharedUI-Android](https://github.com/salesforce/SharedUI-Android) - Salesforce Lightning Design System component library for Android
- [SLDSIcons-Android](https://github.com/salesforce/SLDSIcons-Android) - Salesforce Lightning Design System icon library for Android

## 📚 Additional Resources

- [Salesforce Lightning Design System](https://www.lightningdesignsystem.com/)

---

**Note**: This library is provided as binary packages for use in mobile applications. See the TERMS_OF_USE.txt for complete details on usage, warranty, and liability.
