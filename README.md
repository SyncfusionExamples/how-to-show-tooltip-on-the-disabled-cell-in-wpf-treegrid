# How to show tooltip on the disabled cell in WPF TreeGrid (SfTreeGrid)?

## About the sample
This example illustrates how to show tooltip on the disabled cell in [WPF TreeGrid](https://www.syncfusion.com/wpf-controls/treegrid) (SfTreeGrid)?

[WPF TreeGrid](https://www.syncfusion.com/wpf-controls/treegrid) (SfTreeGrid) allows you to show the ToolTip through the **OnMouseEnter** event. But the tooltip will not be displayed for disabled cells in TreeGrid. You can show tooltip for the disabled [TreeGridCell](https://help.syncfusion.com/cr/wpf/Syncfusion.UI.Xaml.TreeGrid.TreeGridCell.html) and [TreeGridExpanderCell](https://help.syncfusion.com/cr/wpf/Syncfusion.UI.Xaml.TreeGrid.TreeGridExpanderCell.html) in TreeGrid by writing style and enabling ToolTipService.ShowOnDisabled. 

```XML

<Window.Resources>
    <local:ToolTipConverter x:Key="converter" />

    <Style x:Key="toolTipTreeGridExpanderCell" TargetType="syncfusion:TreeGridExpanderCell">
        <Setter Property="ToolTip"
                Value="{Binding Converter={StaticResource converter}, RelativeSource={RelativeSource Mode=Self}}" />
        <Setter Property="ToolTipService.IsEnabled" 
                Value="True" />
        <Setter Property="ToolTipService.ShowOnDisabled" 
                Value="True" />
    </Style>

    <Style x:Key="toolTipTreeGridCell" TargetType="syncfusion:TreeGridCell">
        <Setter Property="ToolTip"
                Value="{Binding Converter={StaticResource converter}, RelativeSource={RelativeSource Mode=Self}}" />
        <Setter Property="ToolTipService.IsEnabled" 
                Value="True" />
        <Setter Property="ToolTipService.ShowOnDisabled" 
                Value="True" />
    </Style>
</Window.Resources>

<Grid>
    <syncfusion:SfTreeGrid Name="treeGrid"
                           AutoGenerateColumns="True"
                           ShowToolTip="True"
                           ExpanderCellStyle="{StaticResource toolTipTreeGridExpanderCell}"
                           CellStyle="{StaticResource toolTipTreeGridCell}"
                           ChildPropertyName="ReportsTo"
                           ParentPropertyName="ID"
                           ItemsSource="{Binding Employees}" />
</Grid>

```

Here, [TreeGridCell](https://help.syncfusion.com/cr/wpf/Syncfusion.UI.Xaml.TreeGrid.TreeGridCell.html) and [TreeGridExpanderCell](https://help.syncfusion.com/cr/wpf/Syncfusion.UI.Xaml.TreeGrid.TreeGridExpanderCell.html) ToolTip are displayed using the converter, where the converter returns the cell value based on the underlying property.

```C#

public class ToolTipConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        var treeGridcell = value as TreeGridCell;
        string cellvalue = string.Empty;

        var column = treeGridcell?.ColumnBase?.TreeGridColumn;
        var treeGrid = column?.GetType()
                              .GetProperty("TreeGrid", System.Reflection.BindingFlags.Instance | System.Reflection.BindingFlags.NonPublic)
                              ?.GetValue(column) as SfTreeGrid;
        if (treeGridcell != null &&
            treeGridcell.ColumnBase != null &&
            !treeGridcell.ColumnBase.IsEditing)
        {
            string formattedValue = "";
            object value1 = null;

            if (treeGridcell.ColumnBase.ColumnElement != null)
            {
                value1 = treeGridcell.ColumnBase.ColumnElement.DataContext;
            }

            if (treeGridcell.ColumnBase.TreeGridColumn != null &&
                treeGrid != null &&
                treeGrid.View != null &&
                treeGridcell.ColumnBase.TreeGridColumn.MappingName != null)
            {
                var tempFormatValue = treeGrid.View
                                              .GetPropertyAccessProvider()
                                              .GetFormattedValue(value1, treeGridcell.ColumnBase.TreeGridColumn.MappingName);

                formattedValue = tempFormatValue?.ToString();
                cellvalue = formattedValue;
            }
        }
        return cellvalue;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

The following screenshot shows the tooltip display for the disabled cell in TreeGrid,
![shows the tooltip display for the disabled cell in SfTreeGrid](Showstootip.png)

Take a moment to peruse the [WPF TreeGrid – ToolTip](https://help.syncfusion.com/wpf/treegrid/tooltip) documentation, where you can find about tooltip with code examples.
## Requirements to run the demo
Visual Studio 2015 and above versions
