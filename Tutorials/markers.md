---
title: Add custom icons or markers
标题：增加用户图标或标记
description: You can add markers, style them, and add tooltips dynamically with Mapbox GL JS. This overview samples all the ways to add custom, interactive markers.
描述：你可以添加标记，设计样式，用Mapbox GL JS添加动态的工具提示。本概述示例了所有添加自定义、可交互的标记的方法。
thumbnail: markers
短语：标记
level: 1
水平：1
topics:
- web apps
language:
- Varies
prependJs:
  - "import * as constants from '../../constants';"
  - "import Note from '@mapbox/dr-ui/note';"
  - "import BookImage from '@mapbox/dr-ui/book-image';"
  - "import Icon from '@mapbox/mr-ui/icon';"
  - "import DemoIframe from '@mapbox/dr-ui/demo-iframe';"
  - "import { LegacyNote } from '../../components/legacy-note';"
contentType: tutorial
---

There are many different ways to style point data in [Mapbox Studio](https://www.mapbox.com/studio), [Mapbox GL JS](https://www.mapbox.com/mapbox-gl-js/api/), and the Mapbox [iOS](https://www.mapbox.com/ios-sdk) and [Android](https://www.mapbox.com/android-sdk) SDKs. In these tutorials, we’ll walk you through several ways to visualize your point data across platforms with custom icons and markers. There's no one right way to build your map, and this guide is designed to provide you with a better understanding of which methods make the most sense for your project.

在Mapbox Studio中，设计点数据的样式有很多种不同的方法，[Mapbox Studio](https://www.mapbox.com/studio), [Mapbox GL JS](https://www.mapbox.com/mapbox-gl-js/api/)，Mapbox [iOS](https://www.mapbox.com/ios-sdk) 和 [Android](https://www.mapbox.com/android-sdk) SDKs。本指南将介绍几种方法，通过拥有自定义图标和标记功能的平台，可视化你的点数据。创建自己的地图不只有一种正确的方法，本指南是为了给你提供一个更好的理解，关于哪种方式更契合你的项目。


## Use the marker playground

Use the [marker playground](/help/interactive-tools/marker-playground/) to add an image marker to the map and view the platform-specific code necessary to recreate this map in your own iOS, Android, or web application.

## 使用marker playground

使用[marker playground](/help/interactive-tools/marker-playground/)在地图上增加一张图片标记，查看必要的平台专属代码，在你的iOS、Android或者web应用上重新创建地图。

![screenshot of the marker playground](/help/img/interactive-tools/marker-playground.png)

## Add markers in Mapbox GL JS

If you already have GeoJSON data that you would like to visualize, you can use Mapbox GL JS to add custom markers to your map. When using the Mapbox GL JS Marker method, you can use any file type that can be used as an [image in CSS](https://developer.mozilla.org/en-US/docs/Web/CSS/image) as a marker on your map. To build a custom marker map entirely in Mapbox GL JS, follow the [Add custom markers in Mapbox GL JS](/help/tutorials/custom-markers-gl-js/) tutorial. This tutorial is code-based.

## 在Mapbox GL JS中添加markers

如果你已经拥有想要可视化的GeoJSON数据，你可以使用Mapbox GL JS为你的地图添加自定义标记。在使用Mapbox GL JS标记方法时，你可以使用任何能作为[image in CSS](https://developer.mozilla.org/en-US/docs/Web/CSS/image)使用的文件格式，作为你的地图的标记。在Mapbox GL JS中完整地创建自定义标记的地图，请参考[Add custom markers in Mapbox GL JS](/help/tutorials/custom-markers-gl-js/)教程。此教程是基于代码的。

{{
  <DemoIframe src="/help/demos/custom-markers-gl-js/index.html" />
}}

## Add custom icons in Mapbox Studio

If you are starting from scratch and need to draw data to be added to a map, start with the [Add points to a map](/help/tutorials/add-points-pt-1/) tutorial series. In this tutorial series, you'll learn how to draw data from scratch in the Mapbox Studio dataset editor, add custom icons to a map in the Mapbox Studio style editor, and make your map interactive using Mapbox GL JS. Making your map interactive will require writing code.

## 在Mapbox Studio中添加自定义图标
如果你开始的时候一团乱麻，需要整理添加到地图中的数据，那请先查看[Add points to a map](/help/tutorials/add-points-pt-1/)教程。在本系列教程中，你将会学习如何在Mapbox Studio的数据库编辑器中整理有用的数据，在Mapbox样式编辑器中给地图增加自定义图标，使用Mapbox GL JS添加你的地图的可交互性。添加地图的可交互性，需要编写代码。

{{<img src='/help/img/studio/add-points-preview.gif' alt='animation of points in the dataset editor and custom icons in the style editor' style={{ width: "50%" }} className='fr-mm w-full w360-mm pl24-mm' />}}

### Part 1: create a dataset

Learn how to create a new dataset in the Mapbox Studio dataset editor, draw new points, save your dataset, and export it as a tileset. **[Add points to a web map, Part 1: create a dataset {{<Icon name='arrow-right' inline={true} />}}](/help/tutorials/add-points-pt-1)**

## 第一步：创建数据库
在Mapbox数据库编辑器中学习如何创建新的数据库，添加新的点、保存你的数据库和将它输出为图块集。

### Part 2: create a style

Learn to create a new style with one of the Mapbox default styles, add a tileset as a layer, and style the points with custom icons in the Mapbox Studio style editor. **[Add points to a web map, Part 2: create a style {{<Icon name='arrow-right' inline={true} />}}](/help/tutorials/add-points-pt-2)**

## 第二步：创建样式
学习从Mapbox默认样式中创建一个新的样式，添加一个图块集作为一个图层，在Mapbox样式编辑器中使用自定义图标设计点样式。**[Add points to a web map, Part 2: create a style {{<Icon name='arrow-right' inline={true} />}}](/help/tutorials/add-points-pt-2)**

### Part 3: add interactivity

Learn how to use Mapbox GL JS to add popups when each point is clicked. This part requires writing code. **[Add points to a web map, Part 3: add interactivity {{<Icon name='arrow-right' inline={true} />}}](/help/tutorials/add-points-pt-3)**

## 第三步：添加可交互性
学习使用Mapbox GL JS为每个被点击的点增加弹出窗口。这部分需要编写代码。**[Add points to a web map, Part 3: add interactivity {{<Icon name='arrow-right' inline={true} />}}](/help/tutorials/add-points-pt-3)**

{{
  <Note
    imageComponent={<BookImage />}
  >
    <p>In this tutorial series you will add custom icons inside Mapbox Studio. Mapbox Studio only supports SVG images. Other image formats are supported when using markers in Mapbox GL JS.</p>
  </Note>
}}

## Add markers in iOS

The Mapbox Maps SDK for iOS offers a few different methods for visualizing point data on your map. Each method has its merits and drawbacks, so be sure to take a quick look, particularly if you expect to add a lot of markers or display highly customized markers.

See the [Adding Points to a Map](https://www.mapbox.com/ios-sdk/maps/overview/markers-and-annotations/) guide for the Mapbox Maps SDK for iOS for more information on how to use the available methods for visualizing point data on your map and to learn about each method's trade-offs.

## 在iOS中添加标记
IOS版的Mapbox地图软件开发包（The Mapbox Maps SDK for iOS）为可视化地图上的点数据提供了一系列不同的方式。每种方法各有利弊，所以当你需要添加大量图标或者显示高度自定义图标时，务必扫一眼。
请查看面向The Mapbox Maps SDK for iOS的[Adding Points to a Map](https://www.mapbox.com/ios-sdk/maps/overview/markers-and-annotations/)指南，以获取更多信息，关于如何使用有效的方法可视化你地图上的点数据，并权衡每种方法的利弊。

## Add markers in Android

The [Annotation plugin](https://docs.mapbox.com/android/plugins/overview/annotation) is the recommended way to add markers to a map when using the Maps SDK for Android. The Annotation plugin simplifies the way to set and adjust the visual properties of annotations on a Mapbox map.

[Directly using the Maps SDK for Android's sources and layers](https://docs.mapbox.com/android/maps/overview/data-driven-styling/) instead of the Annotation plugin, is the advanced way to add annotations. 

## 在Android中添加标记
在使用Android版的地图软件工具包给地图添加标记时，我们推荐注释插件[Annotation plugin](https://docs.mapbox.com/android/plugins/overview/annotation)。注释插件简化了设置和调整Mapbox地图上注释的视觉比例的方法。

[Directly using the Maps SDK for Android's sources and layers](https://docs.mapbox.com/android/maps/overview/data-driven-styling/)是比注释插件更加先进的添加注释的方法。

## Add markers in Mapbox.js

{{
  <LegacyNote
    legacyProduct='Mapbox.js'
    alternativeResource={{
      title: 'Add custom markers in Mapbox GL JS',
      link: '/help/tutorials/custom-markers-gl-js/'
    }}
  />
}}

If you want to know how to add markers with Mapbox.js, read our [Work with markers with Mapbox.js](/help/tutorials/markers-js) guide. In this guide, you will learn how to add markers, customize them, and make them interactive with [Mapbox.js](https://www.mapbox.com/mapbox.js).

## 在Mapbox.js中添加标记
如果你想了解如何使用Mapbox.js添加标记，阅读我们的 [Work with markers with Mapbox.js](/help/tutorials/markers-js)指南。在这个指南中，你将会学习如何添加标记、自定义它们，并用 [Mapbox.js](https://www.mapbox.com/mapbox.js)使它们变得可交互。

