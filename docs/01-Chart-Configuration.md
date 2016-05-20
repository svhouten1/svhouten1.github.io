---
title: Chart Configuration
anchor: chart-configuration
---

Chart.js provides a number of options for changing the behaviour of created charts. These configuration options can be changed on a per chart basis by passing in an options object when creating the chart. Alternatively, the global configuration can be changed which will be used by all charts created after that point.

### Creating a Chart with Options

To create a chart with configuration options, simply pass an object containing your configuration to the constructor. In the example below, a line chart is created and configured to not be responsive.

```javascript
var chartInstance = new Chart(ctx, {
    type: 'line',
    data: data,
    options: {
        responsive: false
    }
});
```

### Global Configuration

This concept was introduced in Chart.js 1.0 to keep configuration DRY, and allow for changing options globally across chart types, avoiding the need to specify options for each instance, or the default for a particular chart type.

Chart.js merges the options object passed to the chart with the global configuration using chart type defaults and scales defaults appropriately. This way you can be as specific as you would like in your individual chart configuration, while still changing the defaults for all chart types where applicable. The global general options are defined in `Chart.defaults.global`. The defaults for each chart type are discussed in the documentation for that chart type.

The following example would set the hover mode to 'single' for all charts where this was not overridden by the chart type defaults or the options passed to the constructor on creation.

```javascript
Chart.defaults.global.hover.mode = 'single';

// Hover mode is set to single because it was not overridden here
var chartInstanceHoverModeSingle = new Chart(ctx, {
    type: 'line',
    data: data,
});

// This chart would have the hover mode that was passed in
var chartInstanceDifferentHoverMode = new Chart(ctx, {
    type: 'line',
    data: data,
    options: {
        hover: {
            // Overrides the global setting
            mode: 'label'
        }
    }
})
```

#### Global Font Settings

There are 4 special global settings that can change all of the fonts on the chart. These options are in `Chart.defaults.global`.

Name | Type | Default | Description
--- | --- | --- | ---
defaultFontColor | Color | '#666' | Default font color for all text
defaultFontFamily | String | "'Helvetica Neue', 'Helvetica', 'Arial', sans-serif" | Default font family for all text
defaultFontSize | Number | 12 | Default font size (in px) for text. Does not apply to radialLinear scale point labels
defaultFontStyle | String | 'normal' | Default font style. Does not apply to tooltip title or footer. Does not apply to chart title

### Common Chart Configuration

The following options are applicable to all charts. The can be set on the [global configuration](#chart-configuration-global-configuration), or they can be passed to the chart constructor.

Name | Type | Default | Description
--- | --- | --- | ---
responsive | Boolean | true | Resizes when the canvas container does.
responsiveAnimationDuration | Number | 0 | Duration in milliseconds it takes to animate to new size after a resize event.
maintainAspectRatio | Boolean | true | Maintain the original canvas aspect ratio `(width / height)` when resizing
events | Array[String] | `["mousemove", "mouseout", "click", "touchstart", "touchmove", "touchend"]` | Events that the chart should listen to for tooltips and hovering
onClick | Function | null | Called if the event is of type 'mouseup' or 'click'. Called in the context of the chart and passed an array of active elements
legendCallback | Function | ` function (chart) { }` | Function to generate a legend. Receives the chart object to generate a legend from. Default implementation returns an HTML string.

### Title Configuration

The title configuration is passed into the `options.title` namespace. The global options for the chart title is defined in `Chart.defaults.global.title`.

Name | Type | Default | Description
--- | --- | --- | ---
display | Boolean | false | Display the title block
position | String | 'top' | Position of the title. Only 'top' or 'bottom' are currently allowed
fullWidth | Boolean | true | Marks that this box should take the full width of the canvas (pushing down other boxes)
fontSize | Number | 12 | Font size inherited from global configuration
fontStyle | String | "normal" | Font style inherited from global configuration
fontColor | Color | "#666" | Font color inherited from global configuration
fontStyle | String | 'bold' | Font styling of the title. 
padding | Number | 10 | Number of pixels to add above and below the title text
text | String | '' | Title text

#### Example Usage

The example below would enable a title of 'Custom Chart Title' on the chart that is created.

```javascript
var chartInstance = new Chart(ctx, {
    type: 'line',
    data: data,
    options: {
        title: {
            display: true,
            text: 'Custom Chart Title'
        }
    }
})
```

### Legend Configuration

The legend configuration is passed into the `options.legend` namespace. The global options for the chart legend is defined in `Chart.defaults.global.legend`.

Name | Type | Default | Description
--- | --- | --- | ---
display | Boolean | true | Is the legend displayed
position | String | 'top' | Position of the legend. Options are 'top' or 'bottom'
fullWidth | Boolean | true | Marks that this box should take the full width of the canvas (pushing down other boxes)
onClick | Function | `function(event, legendItem) {}` | A callback that is called when a click is registered on top of a label item
labels |Object|-| See the [Legend Label Configuration](#chart-configuration-legend-label-configuration) section below.

#### Legend Label Configuration

The legend label configuration is nested below the legend configuration using the `labels` key.

Name | Type | Default | Description
--- | --- | --- | ---
boxWidth | Number | 40 | Width of coloured box
fontSize | Number | 12 | Font size inherited from global configuration
fontStyle | String | "normal" | Font style inherited from global configuration
fontColor | Color | "#666" | Font color inherited from global configuration
fontFamily | String | "Helvetica Neue" | Font family inherited from global configuration
padding | Number | 10 | Padding between rows of colored boxes
generateLabels: | Function | `function(chart) {  }` | Generates legend items for each thing in the legend. Default implementation returns the text + styling for the color box. See [Legend Item](#chart-configuration-legend-item-interface) for details.

#### Legend Item Interface

Items passed to the legend `onClick` function are the ones returned from `labels.generateLabels`. These items must implement the following interface.

```javascript
{
    // Label that will be displayed
    text: String,

    // Fill style of the legend box
    fillStyle: Color,

    // If true, this item represents a hidden dataset. Label will be rendered with a strike-through effect
    hidden: Boolean,

    // For box border. See https://developer.mozilla.org/en/docs/Web/API/CanvasRenderingContext2D/lineCap
    lineCap: String,

    // For box border. See https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/setLineDash
    lineDash: Array[Number],

    // For box border. See https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/lineDashOffset
    lineDashOffset: Number,

    // For box border. See https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/lineJoin
    lineJoin: String,

    // Width of box border 
    lineWidth: Number,

    // Stroke style of the legend box
    strokeStyle: Color
}
```

#### Example

The following example will create a chart with the legend enabled and turn all of the text red in color.

```javascript
var chartInstance = new Chart(ctx, {
    type: 'bar',
    data: data,
    options: {
        legend: {
            display: true,
            labels: {
                fontColor: 'rgb(255, 99, 132)'
            }
        }
    }
});
```

### Tooltip Configuration

The global options for tooltips are defined in `Chart.defaults.global.tooltips`.

Name | Type | Default | Description
--- |:---:| --- | ---
enabled | Boolean | true |
custom | | null |
mode | String | 'single' | Sets which elements appear in the tooltip. Acceptable options are `'single'` or `'label'`. `single` highlights the closest element. `label` highlights elements in all datasets at the same `X` value.
backgroundColor | Color | 'rgba(0,0,0,0.8)' | Background color of the tooltip
 | | |
Label | | | There are three labels you can control. `title`, `body`, `footer` the star (\*) represents one of these three. *(i.e. titleFontFamily, footerSpacing)*
\*FontFamily | String | "'Helvetica Neue', 'Helvetica', 'Arial', sans-serif" |
\*FontSize | Number | 12 |
\*FontStyle | String | title - "bold", body - "normal", footer - "bold" |
\*Spacing | Number | 2 |
\*Color | Color | "#fff" |
\*Align | String | "left" | text alignment. See [MDN Canvas Documentation](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/textAlign)
titleMarginBottom | Number | 6 | Margin to add on bottom of title section
footerMarginTop | Number | 6 | Margin to add before drawing the footer
xPadding | Number | 6 | Padding to add on left and right of tooltip
yPadding | Number | 6 | Padding to add on top and bottom of tooltip
caretSize | Number | 5 | Size, in px, of the tooltip arrow
cornerRadius | Number | 6 | Radius of tooltip corner curves
multiKeyBackground | Color | "#fff" | Color to draw behind the colored boxes when multiple items are in the tooltip
 | | |
callbacks | Object | - |  V2.0 introduces callback functions as a replacement for the template engine in v1. The tooltip has the following callbacks for providing text. For all functions, 'this' will be the tooltip object create from the Chart.Tooltip constructor
**Callback Functions** | | | All functions are called with the same arguments
xLabel | String or Array[Strings] | | This is the xDataValue for each item to be displayed in the tooltip
yLabel | String or Array[Strings] | | This is the yDataValue for each item to be displayed in the tooltip
index | Number | | Data index.
data | Object | | Data object passed to chart.
`return`| String or Array[Strings] | | All functions must return either a string or an array of strings. Arrays of strings are treated as multiple lines of text.
 | | |
*callbacks*.beforeTitle | Function | none | Text to render before the title
*callbacks*.title | Function | `function(tooltipItems, data) { //Pick first xLabel }` | Text to render as the title
*callbacks*.afterTitle | Function | none | Text to render after the ttiel
*callbacks*.beforeBody | Function | none | Text to render before the body section
*callbacks*.beforeLabel | Function | none | Text to render before an individual label
*callbacks*.label | Function | `function(tooltipItem, data) { // Returns "datasetLabel: tooltipItem.yLabel" }` | Text to render as label
*callbacks*.afterLabel | Function | none | Text to render after an individual label
*callbacks*.afterBody | Function | none | Text to render after the body section
*callbacks*.beforeFooter | Function | none | Text to render before the footer section
*callbacks*.footer | Function | none | Text to render as the footer
*callbacks*.afterFooter | Function | none | Text to render after the footer section

### Hover Configuration

The hover configuration is passed into the `options.hover` namespace. The global hover configuration is at `Chart.defaults.global.hover`.

Name | Type | Default | Description
--- | --- | --- | ---
mode | String | 'single' | Sets which elements hover. Acceptable options are `'single'`, `'label'`, or `'dataset'`. `single` highlights the closest element. `label` highlights elements in all datasets at the same `X` value. `dataset` highlights the closest dataset.
animationDuration | Number | 400 | Duration in milliseconds it takes to animate hover style changes
onHover | Function | null | Called when any of the events fire. Called in the context of the chart and passed an array of active elements (bars, points, etc)

### Animation Configuration

The following animation options are available. The global options for are defined in `Chart.defaults.global.animation`.

Name | Type | Default | Description
--- |:---:| --- | ---
duration | Number | 1000 | The number of milliseconds an animation takes.
easing | String | "easeOutQuart" | Easing function to use.
onProgress | Function | none | Callback called on each step of an animation. Passed a single argument, an object, containing the chart instance and an object with details of the animation.
onComplete | Function | none | Callback called at the end of an animation. Passed the same arguments as `onProgress`

#### Animation Callbacks

The `onProgress` and `onComplete` callbacks are useful for synchronizing an external draw to the chart animation. The callback is passed an object that implements the following interface. An example usage of these callbacks can be found on [Github](https://github.com/chartjs/Chart.js/blob/master/samples/AnimationCallbacks/progress-bar.html). This sample displays a progress bar showing how far along the animation is.

```javscript
{
    // Chart object
    chartInstance,

    // Contains details of the on-going animation
    animationObject,
}
```

#### Animation Object

The animation object passed to the callbacks is of type `Chart.Animation`. The object has the following parameters.

```javascript
{
    // Current Animation frame number
    currentStep: Number,

    // Number of animation frames
    numSteps: Number,

    // Animation easing to use
    easing: String,

    // Function that renders the chart
    render: Function,

    // User callback
    onAnimationProgress: Function,

    // User callback
    onAnimationComplete: Function
}
```

### Element Configuration

The global options for elements are defined in `Chart.defaults.global.elements`.
Name | Type | Default | Description
--- |:---:| --- | ---
arc | Object | - | -
*arc*.backgroundColor | Color | `Chart.defaults.global.defaultColor` | Default fill color for arcs
*arc*.borderColor | Color | "#fff" | Default stroke color for arcs
*arc*.borderWidth | Number | 2 | Default stroke width for arcs
line | Object | - | -
*line*.tension | Number | 0.4 | Default bezier curve tension. Set to `0` for no bezier curves.
*line*.backgroundColor | Color | `Chart.defaults.global.defaultColor` | Default line fill color
*line*.borderWidth | Number | 3 | Default line stroke width
*line*.borderColor | Color | `Chart.defaults.global.defaultColor` | Default line stroke color
*line*.borderCapStyle | String | 'butt' | Default line cap style. See [MDN](https://developer.mozilla.org/en/docs/Web/API/CanvasRenderingContext2D/lineCap)
*line*.borderDash | Array | `[]` | Default line dash. See [MDN](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/setLineDash)
*line*.borderDashOffset | Number | 0.0 | Default line dash offset. See [MDN](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/lineDashOffset)
*line*.borderJoinStyle | String | 'miter' | Default line join style. See [MDN](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/lineJoin)
*line*.fill | Boolean | true |
point | Object | - | -
*point*.radius | Number | 3 | Default point radius
*point*.pointStyle | String | 'circle' | Default point style
*point*.backgroundColor | Color | `Chart.defaults.global.defaultColor` | Default point fill color
*point*.borderWidth | Number | 1 | Default point stroke width
*point*.borderColor | Color | `Chart.defaults.global.defaultColor` | Default point stroke color
*point*.hitRadius | Number | 1 | Extra radius added to point radius for hit detection
*point*.hoverRadius | Number | 4 | Default point radius when hovered
*point*.hoverBorderWidth | Number | 1 | Default stroke width when hovered
rectangle | Object | - | -
*rectangle*.backgroundColor | Color | `Chart.defaults.global.defaultColor` | Default bar fill color
*rectangle*.borderWidth | Number | 0 | Default bar stroke width
*rectangle*.borderColor | Color | `Chart.defaults.global.defaultColor` | Default bar stroke color
*rectangle*.borderSkipped | String | 'bottom' | Default skipped (excluded) border for rectangle. Can be one of `bottom`, `left`, `top`, `right`

If for example, you wanted all charts created to be responsive, and resize when the browser window does, the following setting can be changed:

```javascript
Chart.defaults.global.responsive = true;
```

Now, every time we create a chart, `options.responsive` will be `true`.

### Colors

When supplying colors to Chart options, you can use a number of formats. You can specify the color as a string in hexadecimal, RGB, or HSL notations. If a color is needed, but not specified, Chart.js will use the global default color. This color is stored at `Chart.defaults.global.defaultColor`. It is initially set to 'rgb(0, 0, 0, 0.1)';

You can also pass a [CanvasGradient](https://developer.mozilla.org/en-US/docs/Web/API/CanvasGradient) object. You will need to create this before passing to the chart, but using it you can achieve some interesting effects.

The final option is to pass a [CanvasPattern](https://developer.mozilla.org/en-US/docs/Web/API/CanvasPattern) object. For example, if you wanted to fill a dataset with a pattern from an image you could do the following.

```javascript
var img = new Image();
img.src = 'https://example.com/my_image.png';
img.onload = function() {
    var ctx = document.getElementById('canvas').getContext('2d');
    var fillPattern = ctx.createPattern(img, 'repeat');

    var chart = new Chart(ctx, {
        data: {
            labels: ['Item 1', 'Item 2', 'Item 3'],
            datasets: [{
                data: [10, 20, 30],
                backgroundColor: fillPattern
            }]
        }
    })
}

```