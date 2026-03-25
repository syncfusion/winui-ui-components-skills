# Appearance and Styling

## Table of Contents
- [Overview](#overview)
- [Predefined Palettes](#predefined-palettes)
- [Custom Color Palettes](#custom-color-palettes)
- [Gradient Fills](#gradient-fills)
- [Segment-Specific Colors](#segment-specific-colors)
- [Stroke Customization](#stroke-customization)
- [Best Practices](#best-practices)
- [Common Scenarios](#common-scenarios)
- [Related Resources](#related-resources)

## Overview

The WinUI circular chart provides extensive customization options for colors, gradients, and visual styling. You can use predefined color palettes, define custom colors, or apply gradients to individual segments.

**Key Customization Features:**
- Predefined palette themes (Metro, Vibrant, etc.)
- Custom color palettes via PaletteBrushes
- Linear and radial gradients
- Per-segment color control
- Stroke (border) customization
- Fill and opacity settings

## Predefined Palettes

The chart includes several built-in color palettes:

### Available Palettes

- **Metro** - Flat, modern colors
- **AutumnBrights** - Warm autumn tones
- **FloraHues** - Natural, garden colors
- **Pineapple** - Bright tropical colors
- **TomotoSpectrum** - Warm red spectrum
- **RedChrome** - Bold red theme
- **PurpleChrome** - Rich purple theme
- **BlueChrome** - Cool blue theme
- **GreenChrome** - Fresh green theme
- **Elite** - Professional, muted colors
- **LightCandy** - Soft pastel colors
- **SandyBeach** - Beach-inspired neutrals

### Using a Predefined Palette

**XAML:**
```xml
<chart:SfCircularChart Palette="Metro">
    <chart:SfCircularChart.Series>
        <chart:PieSeries ItemsSource="{Binding Data}"
                       XBindingPath="Category"
                       YBindingPath="Value"/>
    </chart:SfCircularChart.Series>
</chart:SfCircularChart>
```

**C#:**
```csharp
SfCircularChart chart = new SfCircularChart();
chart.Palette = ChartColorPalette.Metro;

PieSeries series = new PieSeries()
{
    ItemsSource = viewModel.Data,
    XBindingPath = "Category",
    YBindingPath = "Value"
};
chart.Series.Add(series);
```

### Comparing Palettes

**XAML:**
```xml
<!-- Vibrant colors for marketing data -->
<chart:SfCircularChart Palette="AutumnBrights">
    <chart:SfCircularChart.Series>
        <chart:PieSeries ItemsSource="{Binding MarketingData}"/>
    </chart:SfCircularChart.Series>
</chart:SfCircularChart>

<!-- Professional colors for financial data -->
<chart:SfCircularChart Palette="Elite">
    <chart:SfCircularChart.Series>
        <chart:PieSeries ItemsSource="{Binding FinancialData}"/>
    </chart:SfCircularChart.Series>
</chart:SfCircularChart>
```

## Custom Color Palettes

Define your own color scheme using **PaletteBrushes**:

### Basic Custom Palette

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.PaletteBrushes>
        <SolidColorBrush Color="#FF6B9BD1"/>
        <SolidColorBrush Color="#FFA4D65E"/>
        <SolidColorBrush Color="#FFFF6D6D"/>
        <SolidColorBrush Color="#FFFFD84C"/>
        <SolidColorBrush Color="#FF9966FF"/>
    </chart:SfCircularChart.PaletteBrushes>

    <chart:SfCircularChart.Series>
        <chart:PieSeries ItemsSource="{Binding Data}"
                       XBindingPath="Product"
                       YBindingPath="Sales"/>
    </chart:SfCircularChart.Series>
</chart:SfCircularChart>
```

**C#:**
```csharp
SfCircularChart chart = new SfCircularChart();

chart.PaletteBrushes = new List<Brush>()
{
    new SolidColorBrush(ColorHelper.FromArgb(255, 107, 155, 209)), // Blue
    new SolidColorBrush(ColorHelper.FromArgb(255, 164, 214, 94)),  // Green
    new SolidColorBrush(ColorHelper.FromArgb(255, 255, 109, 109)), // Red
    new SolidColorBrush(ColorHelper.FromArgb(255, 255, 216, 76)),  // Yellow
    new SolidColorBrush(ColorHelper.FromArgb(255, 153, 102, 255))  // Purple
};

PieSeries series = new PieSeries()
{
    ItemsSource = viewModel.Data,
    XBindingPath = "Product",
    YBindingPath = "Sales"
};

chart.Series.Add(series);
```

**How it works:** The chart cycles through the custom colors for each data point

### Brand Colors

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.PaletteBrushes>
        <!-- Primary brand color -->
        <SolidColorBrush Color="#1976D2"/>
        <!-- Secondary brand color -->
        <SolidColorBrush Color="#FFA726"/>
        <!-- Accent colors -->
        <SolidColorBrush Color="#66BB6A"/>
        <SolidColorBrush Color="#EF5350"/>
        <SolidColorBrush Color="#AB47BC"/>
    </chart:SfCircularChart.PaletteBrushes>

    <chart:SfCircularChart.Series>
        <chart:PieSeries ItemsSource="{Binding CompanyData}"/>
    </chart:SfCircularChart.Series>
</chart:SfCircularChart>
```

### Monochromatic Palette

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.PaletteBrushes>
        <SolidColorBrush Color="#0D47A1"/>
        <SolidColorBrush Color="#1565C0"/>
        <SolidColorBrush Color="#1976D2"/>
        <SolidColorBrush Color="#1E88E5"/>
        <SolidColorBrush Color="#42A5F5"/>
        <SolidColorBrush Color="#64B5F6"/>
    </chart:SfCircularChart.PaletteBrushes>

    <chart:SfCircularChart.Series>
        <chart:DoughnutSeries ItemsSource="{Binding Data}"/>
    </chart:SfCircularChart.Series>
</chart:SfCircularChart>
```

**Use case:** Different shades of blue for cohesive, professional look

## Gradient Fills

Apply gradient brushes for sophisticated visual effects:

### Linear Gradient

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.PaletteBrushes>
        <!-- Blue gradient -->
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="#667eea" Offset="0"/>
            <GradientStop Color="#764ba2" Offset="1"/>
        </LinearGradientBrush>
        
        <!-- Orange gradient -->
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="#f093fb" Offset="0"/>
            <GradientStop Color="#f5576c" Offset="1"/>
        </LinearGradientBrush>
        
        <!-- Green gradient -->
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="#4facfe" Offset="0"/>
            <GradientStop Color="#00f2fe" Offset="1"/>
        </LinearGradientBrush>
    </chart:SfCircularChart.PaletteBrushes>

    <chart:SfCircularChart.Series>
        <chart:PieSeries ItemsSource="{Binding Data}"
                       XBindingPath="Category"
                       YBindingPath="Value"/>
    </chart:SfCircularChart.Series>
</chart:SfCircularChart>
```

**C#:**
```csharp
SfCircularChart chart = new SfCircularChart();

// Create gradient brush
LinearGradientBrush gradient1 = new LinearGradientBrush()
{
    StartPoint = new Point(0, 0),
    EndPoint = new Point(1, 1)
};
gradient1.GradientStops.Add(new GradientStop() { Color = ColorHelper.FromArgb(255, 102, 126, 234), Offset = 0 });
gradient1.GradientStops.Add(new GradientStop() { Color = ColorHelper.FromArgb(255, 118, 75, 162), Offset = 1 });

LinearGradientBrush gradient2 = new LinearGradientBrush()
{
    StartPoint = new Point(0, 0),
    EndPoint = new Point(1, 1)
};
gradient2.GradientStops.Add(new GradientStop() { Color = ColorHelper.FromArgb(255, 240, 147, 251), Offset = 0 });
gradient2.GradientStops.Add(new GradientStop() { Color = ColorHelper.FromArgb(255, 245, 87, 108), Offset = 1 });

chart.PaletteBrushes = new List<Brush>() { gradient1, gradient2 };
```

### Radial Gradient

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.PaletteBrushes>
        <RadialGradientBrush>
            <GradientStop Color="#ffcc00" Offset="0"/>
            <GradientStop Color="#ff6600" Offset="1"/>
        </RadialGradientBrush>
        
        <RadialGradientBrush>
            <GradientStop Color="#00ccff" Offset="0"/>
            <GradientStop Color="#0066cc" Offset="1"/>
        </RadialGradientBrush>
        
        <RadialGradientBrush>
            <GradientStop Color="#66ff66" Offset="0"/>
            <GradientStop Color="#009900" Offset="1"/>
        </RadialGradientBrush>
    </chart:SfCircularChart.PaletteBrushes>

    <chart:SfCircularChart.Series>
        <chart:DoughnutSeries ItemsSource="{Binding Data}"
                            XBindingPath="Label"
                            YBindingPath="Value"/>
    </chart:SfCircularChart.Series>
</chart:SfCircularChart>
```

**Effect:** Creates depth and dimension in segments

### Multi-Stop Gradient

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.PaletteBrushes>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="#ff9a56" Offset="0"/>
            <GradientStop Color="#ff6a88" Offset="0.5"/>
            <GradientStop Color="#ff99ac" Offset="1"/>
        </LinearGradientBrush>
        
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="#a8e063" Offset="0"/>
            <GradientStop Color="#56ab2f" Offset="0.5"/>
            <GradientStop Color="#2d5016" Offset="1"/>
        </LinearGradientBrush>
    </chart:SfCircularChart.PaletteBrushes>

    <chart:SfCircularChart.Series>
        <chart:PieSeries ItemsSource="{Binding Data}"/>
    </chart:SfCircularChart.Series>
</chart:SfCircularChart>
```

## Segment-Specific Colors

Control individual segment colors through the data model:

### Using Fill Property in Data Model

**ViewModel:**
```csharp
public class SalesData
{
    public string Product { get; set; }
    public double Sales { get; set; }
    public Brush Fill { get; set; }
}

public class ChartViewModel
{
    public ObservableCollection<SalesData> Data { get; set; }
    
    public ChartViewModel()
    {
        Data = new ObservableCollection<SalesData>()
        {
            new SalesData() 
            { 
                Product = "Product A", 
                Sales = 30,
                Fill = new SolidColorBrush(Colors.DodgerBlue)
            },
            new SalesData() 
            { 
                Product = "Product B", 
                Sales = 25,
                Fill = new SolidColorBrush(Colors.Orange)
            },
            new SalesData() 
            { 
                Product = "Product C", 
                Sales = 20,
                Fill = new SolidColorBrush(Colors.Green)
            }
        };
    }
}
```

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.Series>
        <chart:PieSeries ItemsSource="{Binding Data}"
                       XBindingPath="Product"
                       YBindingPath="Sales"
                       PointColorMapping="Fill"/>
    </chart:SfCircularChart.Series>
</chart:SfCircularChart>
```

**Key property:** `PointColorMapping="Fill"` maps the Fill property in the data model

### Conditional Coloring

**ViewModel:**
```csharp
public class ChartViewModel
{
    public ObservableCollection<SalesData> Data { get; set; }
    
    public ChartViewModel()
    {
        Data = new ObservableCollection<SalesData>();
        
        foreach (var item in GetSalesData())
        {
            // Green for high sales, red for low sales
            item.Fill = item.Sales > 50 
                ? new SolidColorBrush(Colors.Green)
                : new SolidColorBrush(Colors.Red);
            
            Data.Add(item);
        }
    }
}
```

### Gradient Per Segment

**ViewModel:**
```csharp
public class ChartViewModel
{
    public ObservableCollection<SalesData> Data { get; set; }
    
    public ChartViewModel()
    {
        Data = new ObservableCollection<SalesData>()
        {
            new SalesData() 
            { 
                Product = "Product A", 
                Sales = 40,
                Fill = CreateGradient(Colors.Blue, Colors.LightBlue)
            },
            new SalesData() 
            { 
                Product = "Product B", 
                Sales = 35,
                Fill = CreateGradient(Colors.Orange, Colors.Yellow)
            }
        };
    }
    
    private LinearGradientBrush CreateGradient(Color start, Color end)
    {
        LinearGradientBrush brush = new LinearGradientBrush()
        {
            StartPoint = new Point(0, 0),
            EndPoint = new Point(1, 1)
        };
        brush.GradientStops.Add(new GradientStop() { Color = start, Offset = 0 });
        brush.GradientStops.Add(new GradientStop() { Color = end, Offset = 1 });
        return brush;
    }
}
```

## Stroke Customization

Customize segment borders (strokes):

### Basic Stroke

**XAML:**
```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.Series>
        <chart:PieSeries ItemsSource="{Binding Data}"
                       XBindingPath="Category"
                       YBindingPath="Value"
                       Stroke="White"
                       StrokeThickness="3"/>
    </chart:SfCircularChart.Series>
</chart:SfCircularChart>
```

**C#:**
```csharp
PieSeries series = new PieSeries()
{
    ItemsSource = viewModel.Data,
    XBindingPath = "Category",
    YBindingPath = "Value",
    Stroke = new SolidColorBrush(Colors.White),
    StrokeThickness = 3
};
```

**Effect:** White borders around each segment

### No Stroke (Seamless)

**XAML:**
```xml
<chart:PieSeries ItemsSource="{Binding Data}"
               StrokeThickness="0"/>
```

**Use case:** Modern, flat design with no segment separation

### Highlighted Stroke

**XAML:**
```xml
<chart:PieSeries ItemsSource="{Binding Data}"
               Stroke="#333333"
               StrokeThickness="2"/>
```

**Effect:** Dark borders for strong segment definition

## Best Practices

### Color Selection

1. **Contrast** - Ensure adjacent segments are distinguishable
2. **Accessibility** - Consider color-blind friendly palettes
3. **Context** - Use colors that align with data meaning (red for danger, green for success)
4. **Brand consistency** - Use company colors where appropriate
5. **Limit palette size** - 5-8 colors typically sufficient for readability

### Gradients

1. **Subtlety** - Use gradients sparingly; they can be distracting
2. **Direction** - Keep gradient direction consistent across segments
3. **Contrast preservation** - Ensure gradients don't reduce readability
4. **Performance** - Be aware that gradients may impact performance with many segments

### Stroke

1. **Separation** - Use stroke when segments are similar colors
2. **Thickness** - 1-3 pixels typically works best
3. **Color choice** - White or light gray for most backgrounds
4. **Consistency** - Use same stroke style across all series in multi-series charts

### Palette Choice

1. **Predefined first** - Try built-in palettes before creating custom
2. **Professional contexts** - Use Elite, Metro for business applications
3. **Creative contexts** - Use brighter palettes (FloraHues, AutumnBrights) for marketing
4. **Light backgrounds** - Use darker, saturated colors
5. **Dark backgrounds** - Use brighter colors with higher luminance

## Common Scenarios

### Scenario 1: Corporate Dashboard

```xml
<chart:SfCircularChart Palette="Elite">
    <chart:SfCircularChart.Series>
        <chart:DoughnutSeries ItemsSource="{Binding QuarterlyRevenue}"
                            XBindingPath="Quarter"
                            YBindingPath="Revenue"
                            Stroke="White"
                            StrokeThickness="2"
                            InnerRadius="0.6"/>
    </chart:SfCircularChart.Series>
</chart:SfCircularChart>
```

**Purpose:** Professional, muted colors with clear segment separation

### Scenario 2: Marketing Report

```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.PaletteBrushes>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="#FF6B9BD1" Offset="0"/>
            <GradientStop Color="#FF4A7FB8" Offset="1"/>
        </LinearGradientBrush>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="#FFA4D65E" Offset="0"/>
            <GradientStop Color="#FF7FB544" Offset="1"/>
        </LinearGradientBrush>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="#FFFF6D6D" Offset="0"/>
            <GradientStop Color="#FFFF4545" Offset="1"/>
        </LinearGradientBrush>
    </chart:SfCircularChart.PaletteBrushes>

    <chart:SfCircularChart.Series>
        <chart:PieSeries ItemsSource="{Binding CampaignData}"
                       StrokeThickness="0"/>
    </chart:SfCircularChart.Series>
</chart:SfCircularChart>
```

**Purpose:** Eye-catching gradients for visual impact

### Scenario 3: Status Indicator

```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.Series>
        <chart:PieSeries ItemsSource="{Binding StatusData}"
                       XBindingPath="Status"
                       YBindingPath="Count"
                       PointColorMapping="StatusColor"
                       Stroke="White"
                       StrokeThickness="2"/>
    </chart:SfCircularChart.Series>
</chart:SfCircularChart>
```

**ViewModel:**
```csharp
public class StatusViewModel
{
    public ObservableCollection<StatusData> StatusData { get; set; }
    
    public StatusViewModel()
    {
        StatusData = new ObservableCollection<StatusData>()
        {
            new StatusData() 
            { 
                Status = "Success", 
                Count = 85,
                StatusColor = new SolidColorBrush(Colors.Green)
            },
            new StatusData() 
            { 
                Status = "Warning", 
                Count = 10,
                StatusColor = new SolidColorBrush(Colors.Orange)
            },
            new StatusData() 
            { 
                Status = "Error", 
                Count = 5,
                StatusColor = new SolidColorBrush(Colors.Red)
            }
        };
    }
}
```

**Purpose:** Semantic colors for status visualization

### Scenario 4: Monochrome with Accent

```xml
<chart:SfCircularChart>
    <chart:SfCircularChart.PaletteBrushes>
        <SolidColorBrush Color="#E0E0E0"/>
        <SolidColorBrush Color="#BDBDBD"/>
        <SolidColorBrush Color="#9E9E9E"/>
        <SolidColorBrush Color="#1976D2"/>  <!-- Accent color -->
    </chart:SfCircularChart.PaletteBrushes>

    <chart:SfCircularChart.Series>
        <chart:PieSeries ItemsSource="{Binding Data}"
                       Stroke="White"
                       StrokeThickness="1"/>
    </chart:SfCircularChart.Series>
</chart:SfCircularChart>
```

**Purpose:** Highlight one segment while keeping others neutral

### Scenario 5: Dark Theme

```xml
<chart:SfCircularChart Background="#1E1E1E">
    <chart:SfCircularChart.PaletteBrushes>
        <SolidColorBrush Color="#42A5F5"/>
        <SolidColorBrush Color="#66BB6A"/>
        <SolidColorBrush Color="#FFA726"/>
        <SolidColorBrush Color="#EF5350"/>
        <SolidColorBrush Color="#AB47BC"/>
    </chart:SfCircularChart.PaletteBrushes>

    <chart:SfCircularChart.Series>
        <chart:DoughnutSeries ItemsSource="{Binding Data}"
                            XBindingPath="Category"
                            YBindingPath="Value"
                            Stroke="#1E1E1E"
                            StrokeThickness="2"/>
    </chart:SfCircularChart.Series>
</chart:SfCircularChart>
```

**Purpose:** Bright, vibrant colors on dark background for modern UI

## Related Resources

- **Legend** - See `legend.md` for legend icon colors
- **Data Labels** - See `data-labels.md` for UseSeriesPalette option
- **Selection** - See `selection.md` for SelectionBrush customization
- **Getting Started** - See `getting-started.md` for basic chart setup
