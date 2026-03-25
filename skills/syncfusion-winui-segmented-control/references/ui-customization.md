# UI Customization in WinUI Segmented Control

## Table of Contents
- [Overview](#overview)
- [Border Customization](#border-customization)
- [Corner Radius](#corner-radius)
- [Border Colors](#border-colors)
- [Item Template Selector](#item-template-selector)
- [Item Container Style Selector](#item-container-style-selector)
- [Custom Templates](#custom-templates)
- [Best Practices](#best-practices)

## Overview

The Segmented Control offers extensive customization options for borders, corners, colors, templates, and styles. This allows you to create unique segment appearances from modern rounded pills to custom icon layouts.

## Border Customization

### Control Border Thickness

The `BorderThickness` property controls the outer border of the entire segmented control.

```xaml
<syncfusion:SfSegmentedControl 
    BorderThickness="3"
    ItemBorderThickness="0"
    DisplayMemberPath="Name"
    ItemsSource="{Binding Items}"/>
```

**Default:** `1`

### Item Border Thickness

The `ItemBorderThickness` property controls borders between individual segment items.

```xaml
<syncfusion:SfSegmentedControl 
    BorderThickness="0"
    ItemBorderThickness="1"
    DisplayMemberPath="Name"
    ItemsSource="{Binding Items}"/>
```

**Default:** `0,0,1,0` (right border on each item)

### Combined Border Styling

```xaml
<syncfusion:SfSegmentedControl 
    BorderThickness="2"
    ItemBorderThickness="0"
    BorderBrush="#6C58EA"
    SelectionAnimationType="None">
    <x:String>Option 1</x:String>
    <x:String>Option 2</x:String>
    <x:String>Option 3</x:String>
    
    <syncfusion:SfSegmentedControl.ItemContainerStyle>
        <Style TargetType="syncfusion:SfSegmentedItem">
            <Setter Property="Margin" Value="3"/>
        </Style>
    </syncfusion:SfSegmentedControl.ItemContainerStyle>
</syncfusion:SfSegmentedControl>
```

## Corner Radius

### Control Corner Radius

The `CornerRadius` property rounds the corners of the entire control.

```xaml
<syncfusion:SfSegmentedControl 
    CornerRadius="12"
    DisplayMemberPath="Name"
    ItemsSource="{Binding Items}"/>
```

**Default:** `0`

### Item Corner Radius

Use `ItemContainerStyle` to set corner radius for individual items.

```xaml
<syncfusion:SfSegmentedControl CornerRadius="10">
    <syncfusion:SfSegmentedControl.ItemContainerStyle>
        <Style TargetType="syncfusion:SfSegmentedItem">
            <Setter Property="CornerRadius" Value="8"/>
            <Setter Property="Margin" Value="2"/>
        </Style>
    </syncfusion:SfSegmentedControl.ItemContainerStyle>
</syncfusion:SfSegmentedControl>
```

### Selected Segment Corner Radius

```xaml
<Style TargetType="syncfusion:SelectedSegmentBorder" x:Key="roundedSelection">
    <Setter Property="CornerRadius" Value="8"/>
    <Setter Property="Background" Value="#00B7C0"/>
</Style>

<syncfusion:SfSegmentedControl 
    CornerRadius="10"
    SelectedSegmentStyle="{StaticResource roundedSelection}">
    <!-- Items -->
</syncfusion:SfSegmentedControl>
```

## Border Colors

### Control Border Color

```xaml
<syncfusion:SfSegmentedControl 
    BorderBrush="#FF5722"
    BorderThickness="2"
    DisplayMemberPath="Name"
    ItemsSource="{Binding Items}"/>
```

### Item Border Colors

```xaml
<syncfusion:SfSegmentedControl ItemBorderThickness="1">
    <syncfusion:SfSegmentedControl.ItemContainerStyle>
        <Style TargetType="syncfusion:SfSegmentedItem">
            <Setter Property="BorderBrush" Value="#E0E0E0"/>
        </Style>
    </syncfusion:SfSegmentedControl.ItemContainerStyle>
</syncfusion:SfSegmentedControl>
```

### Theme-Aware Border Colors

```xaml
<Grid.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="CustomBorderBrush" Color="#D1D1D1"/>
            </ResourceDictionary>
            <ResourceDictionary x:Key="Dark">
                <SolidColorBrush x:Key="CustomBorderBrush" Color="#4A4A4A"/>
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Grid.Resources>

<syncfusion:SfSegmentedControl 
    BorderBrush="{ThemeResource CustomBorderBrush}"
    BorderThickness="2"/>
```

## Item Template Selector

The `ItemTemplateSelector` allows different templates for different items based on custom logic.

### Define Templates

```xaml
<Grid.Resources>
    <DataTemplate x:Key="selectedTemplate">
        <StackPanel Orientation="Horizontal">
            <Path Data="{Binding Icon}" Fill="{Binding IconColor}" Width="16" Height="16"/>
            <TextBlock Text="{Binding Name}" Margin="6,0,0,0"/>
        </StackPanel>
    </DataTemplate>
    
    <DataTemplate x:Key="defaultTemplate">
        <TextBlock Text="{Binding Name}" Margin="5" VerticalAlignment="Center"/>
    </DataTemplate>
</Grid.Resources>
```

### Create Selector Class

```csharp
public class SegmentTemplateSelector : DataTemplateSelector
{
    public DataTemplate SelectedTemplate { get; set; }
    public DataTemplate DefaultTemplate { get; set; }
    
    protected override DataTemplate SelectTemplateCore(object item, DependencyObject container)
    {
        if (container is SfSegmentedItem segmentItem && segmentItem.IsSelected)
        {
            return SelectedTemplate;
        }
        return DefaultTemplate;
    }
}
```

### Apply Selector

```xaml
<Grid.Resources>
    <local:SegmentTemplateSelector 
        x:Key="templateSelector"
        SelectedTemplate="{StaticResource selectedTemplate}"
        DefaultTemplate="{StaticResource defaultTemplate}"/>
</Grid.Resources>

<syncfusion:SfSegmentedControl 
    ItemsSource="{Binding Items}"
    ItemTemplateSelector="{StaticResource templateSelector}"/>
```

## Item Container Style Selector

The `ItemContainerStyleSelector` applies different styles to items based on custom logic.

### Define Styles

```xaml
<Grid.Resources>
    <Style x:Key="highPriorityStyle" TargetType="syncfusion:SfSegmentedItem">
        <Setter Property="Background" Value="#FFCDD2"/>
        <Setter Property="Margin" Value="5"/>
    </Style>
    
    <Style x:Key="normalPriorityStyle" TargetType="syncfusion:SfSegmentedItem">
        <Setter Property="Background" Value="#FFF9C4"/>
        <Setter Property="Margin" Value="5"/>
    </Style>
</Grid.Resources>
```

### Create Selector Class

```csharp
public class PriorityStyleSelector : StyleSelector
{
    public Style HighPriorityStyle { get; set; }
    public Style NormalPriorityStyle { get; set; }
    
    protected override Style SelectStyleCore(object item, DependencyObject container)
    {
        if (item is PriorityModel model && model.IsHighPriority)
        {
            return HighPriorityStyle;
        }
        return NormalPriorityStyle;
    }
}
```

### Apply Selector

```xaml
<Grid.Resources>
    <local:PriorityStyleSelector 
        x:Key="priorityStyleSelector"
        HighPriorityStyle="{StaticResource highPriorityStyle}"
        NormalPriorityStyle="{StaticResource normalPriorityStyle}"/>
</Grid.Resources>

<syncfusion:SfSegmentedControl 
    ItemsSource="{Binding Items}"
    ItemContainerStyleSelector="{StaticResource priorityStyleSelector}"
    SelectionAnimationType="None"/>
```

## Custom Templates

### Ellipse Style

Create rounded pill-shaped segments.

```xaml
<Grid.Resources>
    <Style TargetType="syncfusion:SelectedSegmentBorder" x:Key="ellipseStyle">
        <Setter Property="CornerRadius" Value="25"/>
        <Setter Property="Background" Value="#FFE521"/>
    </Style>
</Grid.Resources>

<syncfusion:SfSegmentedControl 
    Height="50"
    CornerRadius="25"
    ItemBorderThickness="0"
    SelectedSegmentStyle="{StaticResource ellipseStyle}">
    
    <syncfusion:SfSegmentedControl.Resources>
        <ResourceDictionary>
            <ResourceDictionary.ThemeDictionaries>
                <ResourceDictionary x:Key="Light">
                    <SolidColorBrush x:Key="SyncfusionSegmentedControlBackground" Color="#464646"/>
                    <SolidColorBrush x:Key="SyncfusionSegmentedItemBackground" Color="#464646"/>
                    <SolidColorBrush x:Key="SyncfusionSegmentedItemForeground" Color="White"/>
                    <SolidColorBrush x:Key="SyncfusionSegmentedItemSelectedForeground" Color="#464646"/>
                </ResourceDictionary>
            </ResourceDictionary.ThemeDictionaries>
        </ResourceDictionary>
    </syncfusion:SfSegmentedControl.Resources>
    
    <syncfusion:SfSegmentedControl.ItemContainerStyle>
        <Style TargetType="syncfusion:SfSegmentedItem">
            <Setter Property="CornerRadius" Value="25"/>
        </Style>
    </syncfusion:SfSegmentedControl.ItemContainerStyle>
    
    <x:String>Small</x:String>
    <x:String>Medium</x:String>
    <x:String>Large</x:String>
</syncfusion:SfSegmentedControl>
```

### Circle Style

Create circular segment indicators with border selection.

```xaml
<Grid.Resources>
    <Style TargetType="syncfusion:SelectedSegmentBorder" x:Key="circleStyle">
        <Setter Property="BorderThickness" Value="2"/>
        <Setter Property="BorderBrush" Value="{ThemeResource SystemBaseHighColor}"/>
        <Setter Property="Width" Value="50"/>
        <Setter Property="Height" Value="50"/>
        <Setter Property="Margin" Value="-5,0,0,0"/>
        <Setter Property="CornerRadius" Value="25"/>
        <Setter Property="Background" Value="Transparent"/>
        <Setter Property="Canvas.ZIndex" Value="2"/>
    </Style>
</Grid.Resources>

<syncfusion:SfSegmentedControl 
    BorderThickness="0"
    SelectedIndex="2"
    ItemBorderThickness="0"
    SelectionAnimationType="None"
    SelectedSegmentStyle="{StaticResource circleStyle}"
    ItemsSource="{Binding Colors}">
    
    <syncfusion:SfSegmentedControl.ItemContainerStyle>
        <Style TargetType="syncfusion:SfSegmentedItem">
            <Setter Property="Width" Value="40"/>
            <Setter Property="Height" Value="40"/>
            <Setter Property="CornerRadius" Value="20"/>
            <Setter Property="Padding" Value="0"/>
            <Setter Property="Margin" Value="8,0,8,0"/>
        </Style>
    </syncfusion:SfSegmentedControl.ItemContainerStyle>
    
    <syncfusion:SfSegmentedControl.ItemTemplate>
        <DataTemplate>
            <Border Width="40" Height="40" Background="{Binding Color}" CornerRadius="20"/>
        </DataTemplate>
    </syncfusion:SfSegmentedControl.ItemTemplate>
</syncfusion:SfSegmentedControl>
```

**ViewModel:**

```csharp
public class ColorModel
{
    public Brush Color { get; set; }
}

public class ColorViewModel
{
    public ObservableCollection<ColorModel> Colors { get; set; }
    
    public ColorViewModel()
    {
        Colors = new ObservableCollection<ColorModel>
        {
            new ColorModel { Color = new SolidColorBrush(Colors.Red) },
            new ColorModel { Color = new SolidColorBrush(Colors.Blue) },
            new ColorModel { Color = new SolidColorBrush(Colors.Green) },
            new ColorModel { Color = new SolidColorBrush(Colors.Orange) }
        };
    }
}
```

### Image with Text Style

Combine icons/images with text labels.

```xaml
<syncfusion:SfSegmentedControl 
    SelectedIndex="2"
    ItemsSource="{Binding FileTypes}">
    
    <syncfusion:SfSegmentedControl.ItemTemplate>
        <DataTemplate>
            <StackPanel Height="50">
                <Path 
                    Data="{Binding IconPath}"
                    Fill="{Binding IconColor}"
                    Stretch="Uniform"
                    Width="16"
                    Height="16"
                    Margin="0,8,0,0"/>
                <TextBlock 
                    Text="{Binding Name}"
                    Margin="0,6,0,0"
                    HorizontalAlignment="Center"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:SfSegmentedControl.ItemTemplate>
</syncfusion:SfSegmentedControl>
```

**Model:**

```csharp
public class FileTypeModel
{
    public string Name { get; set; }
    public string IconPath { get; set; } // SVG path data
    public Brush IconColor { get; set; }
}
```

### Top Indicator Style

Create a tab-like appearance with top border indicator.

```xaml
<Grid.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="IndicatorBrush" Color="#6C58EA"/>
            </ResourceDictionary>
            <ResourceDictionary x:Key="Dark">
                <SolidColorBrush x:Key="IndicatorBrush" Color="#9B8AFF"/>
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
        
        <Style TargetType="syncfusion:SelectedSegmentBorder" x:Key="topIndicatorStyle">
            <Setter Property="Margin" Value="0,-6,0,0"/>
            <Setter Property="BorderThickness" Value="0,4,0,0"/>
            <Setter Property="BorderBrush" Value="{ThemeResource IndicatorBrush}"/>
            <Setter Property="Background" Value="Transparent"/>
        </Style>
    </ResourceDictionary>
</Grid.Resources>

<syncfusion:SfSegmentedControl 
    SelectedIndex="2"
    BorderThickness="0"
    ItemBorderThickness="0,4,0,0"
    SelectedSegmentStyle="{StaticResource topIndicatorStyle}"
    ItemsSource="{Binding Tabs}">
    
    <syncfusion:SfSegmentedControl.Resources>
        <ResourceDictionary>
            <ResourceDictionary.ThemeDictionaries>
                <ResourceDictionary x:Key="Light">
                    <SolidColorBrush x:Key="SyncfusionSegmentedItemBackground" Color="Transparent"/>
                    <SolidColorBrush x:Key="SyncfusionSegmentedItemHoverBackground" Color="#F0F0F0"/>
                    <SolidColorBrush x:Key="SyncfusionSegmentedItemSelectedHoverBackground" Color="#F0F0F0"/>
                </ResourceDictionary>
            </ResourceDictionary.ThemeDictionaries>
        </ResourceDictionary>
    </syncfusion:SfSegmentedControl.Resources>
    
    <syncfusion:SfSegmentedControl.ItemContainerStyle>
        <Style TargetType="syncfusion:SfSegmentedItem">
            <Setter Property="BorderBrush" Value="Transparent"/>
            <Setter Property="Padding" Value="12,8"/>
            <Setter Property="Margin" Value="2,0"/>
        </Style>
    </syncfusion:SfSegmentedControl.ItemContainerStyle>
    
    <syncfusion:SfSegmentedControl.ItemTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="{Binding Icon}" Width="16" Height="16"/>
                <TextBlock Text="{Binding Name}" Margin="8,0,0,0" VerticalAlignment="Center"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:SfSegmentedControl.ItemTemplate>
</syncfusion:SfSegmentedControl>
```

### Gradient Background Style

Apply gradient backgrounds to segments.

```xaml
<syncfusion:SfSegmentedControl.ItemTemplate>
    <DataTemplate>
        <Grid Width="80" Height="40">
            <Grid.Background>
                <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
                    <GradientStop Color="{Binding StartColor}" Offset="0"/>
                    <GradientStop Color="{Binding EndColor}" Offset="1"/>
                </LinearGradientBrush>
            </Grid.Background>
            <TextBlock 
                Text="{Binding Name}"
                HorizontalAlignment="Center"
                VerticalAlignment="Center"
                Foreground="White"/>
        </Grid>
    </DataTemplate>
</syncfusion:SfSegmentedControl.ItemTemplate>
```

## Best Practices

### 1. Match Corner Radii

```xaml
<!-- Control, Items, and Selection should have coordinated corner radii -->
<syncfusion:SfSegmentedControl CornerRadius="10">
    <syncfusion:SfSegmentedControl.ItemContainerStyle>
        <Style TargetType="syncfusion:SfSegmentedItem">
            <Setter Property="CornerRadius" Value="8"/>
        </Style>
    </syncfusion:SfSegmentedControl.ItemContainerStyle>
    <syncfusion:SfSegmentedControl.SelectedSegmentStyle>
        <Style TargetType="syncfusion:SelectedSegmentBorder">
            <Setter Property="CornerRadius" Value="8"/>
        </Style>
    </syncfusion:SfSegmentedControl.SelectedSegmentStyle>
</syncfusion:SfSegmentedControl>
```

**Why:** Creates visual harmony and polished appearance.

### 2. Use Theme Resources

```xaml
<Setter Property="Background" Value="{ThemeResource SystemAccentColor}"/>
```

**Why:** Automatically adapts to system theme changes.

### 3. Keep ItemTemplates Simple

```xaml
<!-- Good: Simple and performant -->
<DataTemplate>
    <StackPanel Orientation="Horizontal">
        <SymbolIcon Symbol="{Binding Icon}"/>
        <TextBlock Text="{Binding Name}"/>
    </StackPanel>
</DataTemplate>

<!-- Avoid: Too complex -->
<DataTemplate>
    <Grid>
        <!-- Multiple nested panels, animations, complex bindings -->
    </Grid>
</DataTemplate>
```

**Why:** Complex templates impact performance and rendering.

### 4. Set Explicit Sizes for Custom Templates

```xaml
<syncfusion:SfSegmentedControl.ItemTemplate>
    <DataTemplate>
        <StackPanel Width="80" Height="40">
            <!-- Content -->
        </StackPanel>
    </DataTemplate>
</syncfusion:SfSegmentedControl.ItemTemplate>
```

**Why:** Prevents layout issues and ensures consistent sizing.

### 5. Test Both Themes

```xaml
<!-- Define resources for both Light and Dark -->
<ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Light">
        <!-- Light theme resources -->
    </ResourceDictionary>
    <ResourceDictionary x:Key="Dark">
        <!-- Dark theme resources -->
    </ResourceDictionary>
</ResourceDictionary.ThemeDictionaries>
```

**Why:** Ensures good appearance in both light and dark modes.

### 6. Use Margin for Spacing

```xaml
<syncfusion:SfSegmentedControl.ItemContainerStyle>
    <Style TargetType="syncfusion:SfSegmentedItem">
        <Setter Property="Margin" Value="4,0"/>
    </Style>
</syncfusion:SfSegmentedControl.ItemContainerStyle>
```

**Why:** Creates visual separation between segments.

### 7. Consider Accessibility

```xaml
<!-- Ensure sufficient color contrast -->
<Setter Property="Foreground" Value="{ThemeResource TextFillColorPrimaryBrush}"/>
```

**Why:** Makes segments readable for users with visual impairments.

### 8. Avoid Overriding All Theme Keys

```xaml
<!-- Good: Override only what's needed -->
<SolidColorBrush x:Key="SyncfusionSegmentedItemSelectedBackground" Color="#6C58EA"/>

<!-- Avoid: Overriding everything -->
<!-- This breaks theme consistency -->
```

**Why:** Maintains consistency with system theme and other controls.
