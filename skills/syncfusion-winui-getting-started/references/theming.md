# Theming for Syncfusion WinUI

## Available Themes

Syncfusion WinUI provides built-in themes matching Windows design patterns:

1. **FluentLight** - Light theme (default)
2. **FluentDark** - Dark theme
3. **MaterialLight** - Material design light
4. **MaterialDark** - Material design dark
5. **WinUI** - Windows native theme

---

## Setting Theme Globally

### Method 1: In App.xaml.cs (Recommended)

```csharp
using Syncfusion.WinUI.Theme;

public partial class App : Application
{
    public App()
    {
        SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
        
        // Set application-wide theme
        // IMPORTANT: Must be called before InitializeComponent()
        SyncfusionThemesHelper.SetTheme("FluentDark");
        
        this.InitializeComponent();
    }
}
```

### Method 2: Dynamic Theme Switching

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        this.InitializeComponent();
    }
    
    private void ChangeTheme(string themeName)
    {
        // Can be called at any time to switch themes dynamically
        SyncfusionThemesHelper.SetTheme(themeName);
    }
    
    private void DarkModeButton_Click(object sender, RoutedEventArgs e)
    {
        ChangeTheme("FluentDark");
    }
    
    private void LightModeButton_Click(object sender, RoutedEventArgs e)
    {
        ChangeTheme("FluentLight");
    }
}
```

---

## Theme Names Reference

Use these exact strings when calling `SetTheme()`:

```csharp
"FluentLight"      // Default light theme - Windows Fluent Design
"FluentDark"       // Dark theme - Windows Fluent Design
"MaterialLight"    // Light theme - Material Design
"MaterialDark"     // Dark theme - Material Design
"WinUI"            // Windows native theme
```

---

## System Theme Detection

Automatically detect and apply system theme preference:

```csharp
using Windows.UI.ViewManagement;

public partial class App : Application
{
    public App()
    {
        SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
        
        // Detect system theme and apply matching theme
        var uiSettings = new UISettings();
        var color = uiSettings.GetColorValue(UIColorType.Background);
        
        if (color == Windows.UI.Colors.White)
        {
            SyncfusionThemesHelper.SetTheme("FluentLight");
        }
        else
        {
            SyncfusionThemesHelper.SetTheme("FluentDark");
        }
        
        this.InitializeComponent();
    }
}
```

### Listening to System Theme Changes

```csharp
using Windows.UI.ViewManagement;

public partial class App : Application
{
    private UISettings _uiSettings;
    
    public App()
    {
        SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
        
        // Initialize with current theme
        ApplySystemTheme();
        
        // Listen for theme changes
        _uiSettings = new UISettings();
        _uiSettings.ColorValuesChanged += UISettings_ColorValuesChanged;
        
        this.InitializeComponent();
    }
    
    private void UISettings_ColorValuesChanged(UISettings sender, object args)
    {
        // Re-apply theme when system theme changes
        DispatcherQueue.TryEnqueue(() => ApplySystemTheme());
    }
    
    private void ApplySystemTheme()
    {
        var color = _uiSettings.GetColorValue(UIColorType.Background);
        var theme = color == Windows.UI.Colors.White ? "FluentLight" : "FluentDark";
        SyncfusionThemesHelper.SetTheme(theme);
    }
}
```

---

## Theme Customization

### Using Theme Studio (Offline Customization)

Syncfusion provides Theme Studio for creating custom themes:

1. Download Theme Studio from [Syncfusion Downloads](https://www.syncfusion.com/downloads)
2. Open theme file (.xaml) in Theme Studio
3. Customize colors and styles
4. Export custom theme
5. Include in your project resources

### Applying Custom Theme Resources

```xml
<!-- In App.xaml -->
<Application x:Class="YourApp.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    
    <Application.Resources>
        <!-- Include your custom theme resources -->
        <ResourceDictionary Source="ms-appx:///Themes/CustomTheme.xaml" />
    </Application.Resources>
    
</Application>
```

---

## Theme-Aware Component Styling

### Conditional Styling Based on Theme

```csharp
using Syncfusion.WinUI.Theme;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        this.InitializeComponent();
        
        // Get current theme
        string currentTheme = SyncfusionThemesHelper.CurrentTheme;
        
        if (currentTheme == "FluentDark")
        {
            // Apply dark theme specific styling
            ApplyDarkThemeStyles();
        }
        else
        {
            // Apply light theme specific styling
            ApplyLightThemeStyles();
        }
    }
    
    private void ApplyDarkThemeStyles()
    {
        // Configure components for dark theme
        // Example: Adjust accent colors, shadows, etc.
    }
    
    private void ApplyLightThemeStyles()
    {
        // Configure components for light theme
    }
}
```

---

## Saving User Theme Preference

### Store and Restore Theme Selection

```csharp
using Windows.Storage;
using Syncfusion.WinUI.Theme;

public partial class App : Application
{
    private const string THEME_PREFERENCE_KEY = "UserThemePreference";
    
    public App()
    {
        SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
        
        // Load saved theme preference
        string savedTheme = LoadThemePreference();
        if (!string.IsNullOrEmpty(savedTheme))
        {
            SyncfusionThemesHelper.SetTheme(savedTheme);
        }
        else
        {
            // Use system theme as default
            ApplySystemTheme();
        }
        
        this.InitializeComponent();
    }
    
    public void SaveThemePreference(string theme)
    {
        var localSettings = ApplicationData.Current.LocalSettings;
        localSettings.Values[THEME_PREFERENCE_KEY] = theme;
        SyncfusionThemesHelper.SetTheme(theme);
    }
    
    public string LoadThemePreference()
    {
        var localSettings = ApplicationData.Current.LocalSettings;
        return localSettings.Values.ContainsKey(THEME_PREFERENCE_KEY) 
            ? (string)localSettings.Values[THEME_PREFERENCE_KEY] 
            : null;
    }
    
    private void ApplySystemTheme()
    {
        var uiSettings = new Windows.UI.ViewManagement.UISettings();
        var color = uiSettings.GetColorValue(Windows.UI.ViewManagement.UIColorType.Background);
        var theme = color == Windows.UI.Colors.White ? "FluentLight" : "FluentDark";
        SyncfusionThemesHelper.SetTheme(theme);
    }
}
```

---

## Material Design vs. Fluent Design

### When to Use Each

| Aspect | Fluent | Material |
|--------|--------|----------|
| **Best For** | Windows applications | Cross-platform consistency |
| **Feel** | Modern, native Windows | Google Material Design |
| **Animations** | Windows-style transitions | Material motion principles |
| **Color Palette** | Windows accent colors | Material color system |
| **Spacing** | Windows-based | Material guidelines |

### Side-by-Side Comparison

```csharp
// Fluent Light - Default Windows style
SyncfusionThemesHelper.SetTheme("FluentLight");

// vs.

// Material Light - Material Design principles
SyncfusionThemesHelper.SetTheme("MaterialLight");
```

---

## Theme Performance Considerations

### Theme Switch Performance

- Theme switching is fast (milliseconds)
- No need to recreate components
- Safe to call multiple times
- Can be used for real-time theme toggling

### Best Practices

1. **Set theme once at startup** if possible
2. **Cache theme preference** to avoid repeated file I/O
3. **Avoid theme switching in tight loops** or animations
4. **Test theme transitions** on target hardware

---

## Accessibility and Theming

### High Contrast Mode Support

All Syncfusion themes automatically support Windows High Contrast mode:

```csharp
using Windows.UI.ViewManagement;

public partial class App : Application
{
    public App()
    {
        SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
        
        // Check if high contrast is enabled
        var uiSettings = new UISettings();
        if (uiSettings.HighContrast)
        {
            // High contrast mode is active
            // Themes automatically adjust
        }
        
        this.InitializeComponent();
    }
}
```

### WCAG Compliance

All built-in themes meet WCAG AA standards:
- ✓ Text contrast ratio: ≥4.5:1
- ✓ Large text contrast: ≥3:1
- ✓ Focus indicators clearly visible
- ✓ Component interactions keyboard accessible

---

## Theme Documentation Resources

- **Fluent Design System:** [Microsoft Fluent Design](https://www.microsoft.com/design/fluent)
- **Material Design:** [Google Material Design](https://material.io)
- **WCAG Guidelines:** [W3C WCAG 2.1](https://www.w3.org/WAI/WCAG21/quickref/)
- **Syncfusion Themes:** [Theme Studio Documentation](https://www.syncfusion.com/products/wpf/theme-studio)
