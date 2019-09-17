---
title: Analyze data with Turf.js and Mapbox GL JS
标题：用Turf.js和Mapbox GL JS分析数据
description: Using Turf.js, add spatial analysis to our map to solve problems. This guide walks through an example of Turf.js and Mapbox GL JS in a real-world context.
描述：使用Turf.js，为我们的地图添加空间分析以解决问题。本指南将介绍一个使用Turf.js和Mapbox GL JS的真实案例。
thumbnail: analysisWithTurf
短语：用Turf分析
level: 2
水平：2
topics:
- web apps
- analysis
主题：web应用、分析
language:
- JavaScript
语言：JavaScript
prereq: Familiarity with front-end development concepts.
条件：熟练掌握front-end发展概念
prependJs:
  - "import * as constants from '../../constants';"
  - "import UserAccessToken from '../../components/user-access-token';"
  - "import DemoIframe from '@mapbox/dr-ui/demo-iframe';"
contentType: tutorial
---

This guide walks through the basics of [Turf.js](http://turfjs.org), a JavaScript library for spatial analysis and statistics, and how to use it to add spatial analysis to your Mapbox GL JS maps.

本指南将介绍[Turf.js](http://turfjs.org)的基本内容，一种基于空间分析和数据的JavaScript库，以及如何通过它给你的Mapbox GL JS地图添加空间分析的内容。

Let's say you are part of a team that manages health and safety for the libraries in Lexington, KY. One important part of your preparedness mandate is to know __which hospital is closest to each library__ in case there's an emergency at one of your facilities. This example will walk you through making a map of libraries and hospitals; when a user clicks on a library, the map will show which hospital is nearest.

假设你是某团队的成员，管理着肯塔基洲列克星敦市各图书馆的健康和安全。战备状态的一个重要指令是，在某个图书馆发生紧急情况的时候，了解离每个图书馆的最近的医院。这个例子将会介绍制作一张图书馆和医院的地图；当用户点击某个图书馆，地图上会显示出最近的医院。

{{
  <DemoIframe src="/help/demos/turfjs-intro/demo-four.html" />
}}

## Getting started

There are a few resources you'll need to get started:

- [__An access token__](/help/glossary/access-token/). The token is used to associate a map with your account. You can find your access tokens on your [Access Tokens page](https://www.mapbox.com/account/access-tokens/) and below:
```js
mapboxgl.accessToken = '{{ <UserAccessToken /> }}';
```
- [__Mapbox GL JS__](https://www.mapbox.com/mapbox-gl-js/). A Mapbox JavaScript API for building maps.
- [__Turf.js__](http://turfjs.org/). Turf is the JavaScript library you'll be using today to add analysis to your map.
- __Data.__ This example uses two data files: hospitals in Lexington, KY and libraries in Lexington, KY.
- __A text editor.__ You'll be writing HTML, CSS, and JavaScript.

## 开始

以下是你开始的时候需要的一些资源：
- 访问令牌[__An access token__](/help/glossary/access-token/)。 这个令牌连接地图和你的账户。你可以在你的[Access Tokens page](https://www.mapbox.com/account/access-tokens/)找到你的访问令牌，如下所示：
```js
mapboxgl.accessToken = '{{ <UserAccessToken /> }}';
```
- [__Mapbox GL JS__](https://www.mapbox.com/mapbox-gl-js/)。 用于创建地图的Mapbox JavaScript应用程序接口。
- [__Turf.js__](http://turfjs.org/)。Turf是今天将要用于给地图添加分析的Javascript库。
- 数据（Data）。这个例子使用了两种数据文件：肯塔基洲列克星敦市的医院和肯塔基洲列克星敦市的图书馆。
- 文本编辑器（A text editor）。你将会编写HTML、CSS和JavaScript。

## Add structure

For this guide, include the latest versions of Mapbox GL JS and Turf.js. Add these libraries to your HTML file by copying the snippet below.

## 添加结构

本指南使用的是最新版本的Mapbox GL JS 和Turf.js。复制下面的片段，将这些程序库添加到你的HTML文件中。

```html
<link href='https://api.mapbox.com/mapbox-gl-js/{{constants.VERSION_MAPBOXGLJS}}/mapbox-gl.css' rel='stylesheet' />
<script src='https://api.mapbox.com/mapbox-gl-js/{{constants.VERSION_MAPBOXGLJS}}/mapbox-gl.js'></script>
<script src='https://api.mapbox.com/mapbox.js/plugins/turf/{{constants.VERSION_TURF}}/turf.min.js'></script>
```

Now, add your map element. First, in the `body`, create an empty `div` for your map.

现在，为你的地图添加元素。第一步，在`body`里面，为你的地图创建一个空的 `div` 。

```html
<div id='map'></div>
```

Next, add some CSS to a `style` element in the `head` so your map takes up the width of the page.

下一步，为 `head`里的`style`元素添加一些CSS，你的地图将会占据整个页面的宽度。

```css
body {
  margin: 0;
  padding: 0;
}

#map {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 100%;
}
```

## Initialize a map

Now that your page has some nice structure to it, go ahead and get a map on the page using Mapbox GL JS. This is where you'll need to use your access token. Add the following script inside the `<body>` after the HTML. Create a `new mapboxgl.Map` object called `map`. In this example, you'll be using the Mapbox Light template style, but you could also use a [custom style made with Mapbox Studio](/help/tutorials/create-a-custom-style). Finally, use `center` and `zoom` to set the view on Lexington, KY.

## 初始化地图

现在你的地图已经有了一些精美的结构，继续使用Mapbox GL JS在页面上得到一张地图。这里需要用到你的访问令牌。将下面的代码添加到HTML后面的 `<body>`里。创建一个叫作 `map`的`new mapboxgl.Map`对象。在本例中，你将使用Mapbox Light的template风格，但是你也可以使用Mapbox Studio的自定义风格[custom style made with Mapbox Studio](/help/tutorials/create-a-custom-style)。最后，使用 `center` 和 `zoom` 将视图设置到肯塔基洲列克星敦市。

```js
mapboxgl.accessToken = '{{ <UserAccessToken /> }}';
var map = new mapboxgl.Map({
  container: 'map', // container id
  style: 'mapbox://styles/mapbox/light-v{{constants.VERSION_LIGHT_STYLE}}', // stylesheet location
  center: [-84.5, 38.05], // starting position
  zoom: 12 // starting zoom
});
```

Sweet! Now your page has a map centered on Lexington, KY.

非常好！现在你的页面已经是以肯塔基洲列克星敦市为中心的地图了。

{{
  <DemoIframe src="/help/demos/turfjs-intro/demo-one.html" />
}}


## Load data

As mentioned above, this example uses two data files: libraries and hospitals in Lexington, KY, each of them is a GeoJSON FeatureCollection. In the next step, you'll add them to the style as layers and add code to make sure they're styled differently from each other.

## 装载数据
如上所述，本例将用到两个数据文件：肯塔基洲列克星敦市的图书馆和医院，它们各自成GeoJSON功能集。在下一步中，你将要将它们作为图层加载到风格中，添加代码以确保它们在风格中互相独立。

```js
var hospitals = {
  type: 'FeatureCollection',
  features: [
    { type: 'Feature', properties: { Name: 'VA Medical Center -- Leestown Division', Address: '2250 Leestown Rd' }, geometry: { type: 'Point', coordinates: [-84.539487, 38.072916] } },
    { type: 'Feature', properties: { Name: 'St. Joseph East', Address: '150 N Eagle Creek Dr' }, geometry: { type: 'Point', coordinates: [-84.440434, 37.998757] } },
    { type: 'Feature', properties: { Name: 'Central Baptist Hospital', Address: '1740 Nicholasville Rd' }, geometry: { type: 'Point', coordinates: [-84.512283, 38.018918] } },
    { type: 'Feature', properties: { Name: 'VA Medical Center -- Cooper Dr Division', Address: '1101 Veterans Dr' }, geometry: { type: 'Point', coordinates: [-84.506483, 38.02972] } },
    { type: 'Feature', properties: { Name: 'Shriners Hospital for Children', Address: '1900 Richmond Rd' }, geometry: { type: 'Point', coordinates: [-84.472941, 38.022564] } },
    { type: 'Feature', properties: { Name: 'Eastern State Hospital', Address: '627 W Fourth St' }, geometry: { type: 'Point', coordinates: [-84.498816, 38.060791] } },
    { type: 'Feature', properties: { Name: 'Cardinal Hill Rehabilitation Hospital', Address: '2050 Versailles Rd' }, geometry: { type: 'Point', coordinates: [-84.54212, 38.046568] } },
    { type: 'Feature', properties: { Name: 'St. Joseph Hospital', ADDRESS: '1 St Joseph Dr' }, geometry: { type: 'Point', coordinates: [-84.523636, 38.032475] } },
    { type: 'Feature', properties: { Name: 'UK Healthcare Good Samaritan Hospital', Address: '310 S Limestone' }, geometry: { type: 'Point', coordinates: [-84.501222, 38.042123] } },
    { type: 'Feature', properties: { Name: 'UK Medical Center', Address: '800 Rose St' }, geometry: { type: 'Point', coordinates: [-84.508205, 38.031254] } }
  ]
};
var libraries = {
  type: 'FeatureCollection',
  features: [
    { type: 'Feature', properties: { Name: 'Village Branch', Address: '2185 Versailles Rd' }, geometry: { type: 'Point', coordinates: [-84.548369, 38.047876] } },
    { type: 'Feature', properties: { Name: 'Northside Branch', ADDRESS: '1733 Russell Cave Rd' }, geometry: { type: 'Point', coordinates: [-84.47135, 38.079734] } },
    { type: 'Feature', properties: { Name: 'Central Library', ADDRESS: '140 E Main St' }, geometry: { type: 'Point', coordinates: [-84.496894, 38.045459] } },
    { type: 'Feature', properties: { Name: 'Beaumont Branch', Address: '3080 Fieldstone Way' }, geometry: { type: 'Point', coordinates: [-84.557948, 38.012502] } },
    { type: 'Feature', properties: { Name: 'Tates Creek Branch', Address: '3628 Walden Dr' }, geometry: { type: 'Point', coordinates: [-84.498679, 37.979598] } },
    { type: 'Feature', properties: { Name: 'Eagle Creek Branch', Address: '101 N Eagle Creek Dr' }, geometry: { type: 'Point', coordinates: [-84.442219, 37.999437] } }
  ]
};

map.on('load', function() {
  map.addLayer({
    id: 'hospitals',
    type: 'symbol',
    source: {
      type: 'geojson',
      data: hospitals
    },
    layout: {
      'icon-image': 'hospital-15',
      'icon-allow-overlap': true
    },
    paint: { }
  });
  map.addLayer({
    id: 'libraries',
    type: 'symbol',
    source: {
      type: 'geojson',
      data: libraries
    },
    layout: {
      'icon-image': 'library-15'
    },
    paint: { }
  });
});
```

Note that `hospitals` layer and `libraries` layer are added to the map _after_ the map by wrapping them in `map.on('load', function(){});`

可以注意到医院图层 `hospitals`和图书馆图层 `libraries`被打包成`map.on('load', function(){});`之后，已经被添加到地图中。

{{
  <DemoIframe src="/help/demos/turfjs-intro/demo-two.html" />
}}

## Add interactivity

Your map users will want to know the names of the libraries and hospitals displayed on the map, so next you'll add some popups. For this map, add some popups to these features that appear when the user hovers over the markers. Insert this into your script after you've created `hospitals` layer and `libraries` layer.

## 添加可交互性
你的地图用户将会想要知道地图上呈现的图书馆和医院的名字，因此，下一步你要添加一些弹出窗口。在这个地图中，给这些特征加一些弹出窗口，当用户盘旋在标记上方时，弹出窗口显现。在你创建了医院图层`hospitals`和图书馆图层`libraries`之后，将它插入到你的脚本中。

```js
var popup = new mapboxgl.Popup();

map.on('mousemove', function(e) {
  var features = map.queryRenderedFeatures(e.point, { layers: ['hospitals', 'libraries'] });
  if (!features.length) {
    popup.remove();
    return;
  }
  var feature = features[0];

  popup.setLngLat(feature.geometry.coordinates)
    .setHTML(feature.properties.Name)
    .addTo(map);

  map.getCanvas().style.cursor = features.length ? 'pointer' : '';
});
```

Next you'll make your map of the libraries and hospitals in Lexington even more useful by adding some analysis.

接下来你要添加一些分析，使你那包含列克星敦市图书馆和医院的地图更加有用。

{{
  <DemoIframe src="/help/demos/turfjs-intro/demo-three.html" />
}}

## Add Turf

[Turf](http://turfjs.org) is a JavaScript library for adding spatial and statistical analysis to your web maps. It contains many commonly-used GIS tools -- like buffer, union, and merge -- as well as statistical analysis functions -- like sum, median, and average.

Fortunately, Turf has some functions that will help you out here! You're going to update your map so that clicking on a library will show users which hospital is closest to that library.

As a first step, you'll need to add a new source with the id `'nearest-hospital'` when the map is loaded.

## 添加Turf

[Turf](http://turfjs.org)是一个JavaScript库，可以给你的web地图添加空间和数据分析。它包含了许多常用的GIS工具——比如缓冲、结合、合并，和数据分析功能——比如总数、中位数和平均数。

幸运的是，Turf有一些功能可以帮助你。你将要更新你的地图，以使当你点击图书馆的时候，它会向用户显示哪个是离图书馆最近的医院。

作为第一步，当地图加载的时候，你需要添加一个名叫 `'nearest-hospital'` 的资源。

```js
map.addSource('nearest-hospital', {
  type: 'geojson',
  data: {
    type: 'FeatureCollection',
    features: [
    ]
  }
});
```
You’ll make an “event handler” for when someone clicks on a library marker. When an event occurs, like a click on a marker, the event handler tells the map what to do in response. Before, you created event handlers for hovering over hospital and library markers; now you’re going to make one for clicks.

This is the structure of an event handler; anything you want to happen on click goes inside of the braces `{}`. In this case, you want to use Turf to identify the nearest hospital to the clicked library and make that marker larger to identify it.

你要制作一个“事件处理程序”，为了应对有人点击图书馆的标记。当事件发生，比如点击某个标记，这个事件处理程序告诉地图之后要做什么反应。在此之前，你要在医院和图书馆的标记上方创建事件处理程序；现在你将要为点击操作创建一个。

这是一个事件处理程序的结构；任何你想要在点击时发生的事情都包含在 `{}`框架内。在这种情况下，你需要使用Turf识别离被点击的图书馆最近的医院，同时使标记加大以增强识别性。

```js
map.on('click', function(e) {
  // Return any features from the 'libraries' layer whenever the map is clicked
  var libraryFeatures = map.queryRenderedFeatures(e.point, { layers: ['libraries'] });
  if (!libraryFeatures.length) {
    return;
  }
  var libraryFeature = libraryFeatures[0];

  // Using Turf, find the nearest hospital to library clicked
  var nearestHospital = turf.nearest(libraryFeature, hospitals);

  // If a nearest library is found
  if (nearestHospital !== null) {
    // Update the 'nearest-library' data source to include
    // the nearest library
    map.getSource('nearest-hospital').setData({
      type: 'FeatureCollection',
      features: [
        nearestHospital
      ]
    });
    // Create a new circle layer from the 'nearest-library' data source
    map.addLayer({
      id: 'nearest-hospital',
      type: 'circle',
      source: 'nearest-hospital',
      paint: {
        'circle-radius': 12,
        'circle-color': '#486DE0'
      }
    }, 'hospitals');
  }
});
```

{{
  <DemoIframe src="/help/demos/turfjs-intro/demo-four.html" />
}}

## Finished product

Nicely done! You have successfully created a map that calculates which hospital is closest to each library dynamically. Your finished HTML file should look like this:

## 最终产品
出色的完成了！你已经成功创建了一个地图，能动态计算出离每个图书馆最近的医院。你的HTML文件看起来应该像这样：

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset='utf-8' />
    <title></title>
    <meta name='viewport' content='initial-scale=1,maximum-scale=1,user-scalable=no' />
    <script src='https://api.tiles.mapbox.com/mapbox-gl-js/{{constants.VERSION_MAPBOXGLJS}}/mapbox-gl.js'></script>
    <link href='https://api.tiles.mapbox.com/mapbox-gl-js/{{constants.VERSION_MAPBOXGLJS}}/mapbox-gl.css' rel='stylesheet' />
    <script src='https://npmcdn.com/@turf/turf/turf.min.js'></script>
    <style>
      body {
        margin: 0;
        padding: 0;
      }

      #map {
        position: absolute;
        top: 0;
        bottom: 0;
        width: 100%;
      }
    </style>
</head>
<body>
<div id='map'></div>
<script>
mapboxgl.accessToken = '{{ <UserAccessToken /> }}';
var map = new mapboxgl.Map({
  container: 'map', // container id
  style: 'mapbox://styles/mapbox/light-v{{constants.VERSION_LIGHT_STYLE}}', // stylesheet location
  center: [-84.5, 38.05], // starting position
  zoom: 11 // starting zoom
});

var hospitals = {
  type: 'FeatureCollection',
  features: [
    { type: 'Feature', properties: { Name: 'VA Medical Center -- Leestown Division', Address: '2250 Leestown Rd' }, geometry: { type: 'Point', coordinates: [-84.539487, 38.072916] } },
    { type: 'Feature', properties: { Name: 'St. Joseph East', Address: '150 N Eagle Creek Dr' }, geometry: { type: 'Point', coordinates: [-84.440434, 37.998757] } },
    { type: 'Feature', properties: { Name: 'Central Baptist Hospital', Address: '1740 Nicholasville Rd' }, geometry: { type: 'Point', coordinates: [-84.512283, 38.018918] } },
    { type: 'Feature', properties: { Name: 'VA Medical Center -- Cooper Dr Division', Address: '1101 Veterans Dr' }, geometry: { type: 'Point', coordinates: [-84.506483, 38.02972] } },
    { type: 'Feature', properties: { Name: 'Shriners Hospital for Children', Address: '1900 Richmond Rd' }, geometry: { type: 'Point', coordinates: [-84.472941, 38.022564] } },
    { type: 'Feature', properties: { Name: 'Eastern State Hospital', Address: '627 W Fourth St' }, geometry: { type: 'Point', coordinates: [-84.498816, 38.060791] } },
    { type: 'Feature', properties: { Name: 'Cardinal Hill Rehabilitation Hospital', Address: '2050 Versailles Rd' }, geometry: { type: 'Point', coordinates: [-84.54212, 38.046568] } },
    { type: 'Feature', properties: { Name: 'St. Joseph Hospital', Address: '1 St Joseph Dr' }, geometry: { type: 'Point', coordinates: [-84.523636, 38.032475] } },
    { type: 'Feature', properties: { Name: 'UK Healthcare Good Samaritan Hospital', Address: '310 S Limestone' }, geometry: { type: 'Point', coordinates: [-84.501222, 38.042123] } },
    { type: 'Feature', properties: { Name: 'UK Medical Center', Address: '800 Rose St' }, geometry: { type: 'Point', coordinates: [-84.508205, 38.031254] } }
  ]
};
var libraries = {
  type: 'FeatureCollection',
  features: [
    { type: 'Feature', properties: { Name: 'Village Branch', Address: '2185 Versailles Rd' }, geometry: { type: 'Point', coordinates: [-84.548369, 38.047876] } },
    { type: 'Feature', properties: { Name: 'Northside Branch', Address: '1733 Russell Cave Rd' }, geometry: { type: 'Point', coordinates: [-84.47135, 38.079734] } },
    { type: 'Feature', properties: { Name: 'Central Library', Address: '140 E Main St' }, geometry: { type: 'Point', coordinates: [-84.496894, 38.045459] } },
    { type: 'Feature', properties: { Name: 'Beaumont Branch', Address: '3080 Fieldstone Way' }, geometry: { type: 'Point', coordinates: [-84.557948, 38.012502] } },
    { type: 'Feature', properties: { Name: 'Tates Creek Branch', Address: '3628 Walden Dr' }, geometry: { type: 'Point', coordinates: [-84.498679, 37.979598] } },
    { type: 'Feature', properties: { Name: 'Eagle Creek Branch', Address: '101 N Eagle Creek Dr' }, geometry: { type: 'Point', coordinates: [-84.442219, 37.999437] } }
  ]
};


map.on('load', function() {
  map.addLayer({
    id: 'hospitals',
    type: 'symbol',
    source: {
      type: 'geojson',
      data: hospitals
    },
    layout: {
      'icon-image': 'hospital-15',
      'icon-allow-overlap': true
    },
    paint: { }
  });

  map.addLayer({
    id: 'libraries',
    type: 'symbol',
    source: {
      type: 'geojson',
      data: libraries
    },
    layout: {
      'icon-image': 'library-15'
    },
    paint: { }
  });

  map.addSource('nearest-hospital', {
    type: 'geojson',
    data: {
      type: 'FeatureCollection',
      features: []
    }
  });
});

var popup = new mapboxgl.Popup();

map.on('mousemove', function(e) {

  var features = map.queryRenderedFeatures(e.point, { layers: ['hospitals', 'libraries'] });
  if (!features.length) {
    popup.remove();
    return;
  }

  var feature = features[0];

  popup.setLngLat(feature.geometry.coordinates)
  .setHTML(feature.properties.Name)
  .addTo(map);

  map.getCanvas().style.cursor = features.length ? 'pointer' : '';

});

map.on('click', function(e) {
  var libraryFeatures = map.queryRenderedFeatures(e.point, { layers: ['libraries'] });
  if (!libraryFeatures.length) {
    return;
  }

  var libraryFeature = libraryFeatures[0];

  var nearestHospital = turf.nearest(libraryFeature, hospitals);

  if (nearestHospital !== null) {

    map.getSource('nearest-hospital').setData({
      type: 'FeatureCollection',
      features: [nearestHospital]
    });

    map.addLayer({
      id: 'nearest-hospital',
      type: 'circle',
      source: 'nearest-hospital',
      paint: {
        'circle-radius': 12,
        'circle-color': '#486DE0'
      }
    }, 'hospitals');
  }
});

</script>
</body>
</html>
```


## Next steps

[Turf](http://turfjs.org) has dozens of tools that would help extend this map even further. For example, you could also use [turf.distance](http://turfjs.org/docs#distance) to determine not only which hospital is closest, but exactly how far away it is. The possibilities are virtually endless with Turf.js!

## 接下来的步骤

Turf](http://turfjs.org) 有许多可以帮助扩展地图功能的工具。例如，你可以使用[turf.distance](http://turfjs.org/docs#distance)来寻找最近的医院，还可以测算它们之间的距离。Turf.js的可能性实际上是无限的。
