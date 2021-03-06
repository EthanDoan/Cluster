![Cluster](https://raw.githubusercontent.com/efremidze/Cluster/master/Images/logo.png)

<p align="center">
<a href="https://travis-ci.org/efremidze/Cluster" target="_blank">
<img alt="Build Status" src="https://travis-ci.org/efremidze/Cluster.svg?style=flat">
</a>
<a href="https://swift.org" target="_blank">
<img alt="Language" src="https://img.shields.io/badge/Swift-4-orange.svg?style=flat">
</a>
<a href="http://cocoapods.org/pods/Cluster" target="_blank">
<img alt="Version" src="https://img.shields.io/cocoapods/v/Cluster.svg?style=flat">
</a>
<a href="http://cocoapods.org/pods/Cluster" target="_blank">
<img alt="License" src="https://img.shields.io/cocoapods/l/Cluster.svg?style=flat">
</a>
<a href="http://cocoapods.org/pods/Cluster" target="_blank">
<img alt="Platform" src="https://img.shields.io/cocoapods/p/Cluster.svg?style=flat">
</a>
<a href="https://github.com/Carthage/Carthage" target="_blank">
<img alt="Carthage compatible" src="https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat">
</a>
</p>

----------------

**Cluster** is an easy map annotation clustering library. This repository uses an efficient method (QuadTree) to aggregate pins into a cluster.

<img src="https://raw.githubusercontent.com/efremidze/Cluster/master/Images/demo.gif" width="320">

If you want to try it, simply run:

```
$ pod try Cluster
```

## Features

- [x] Adding/Removing Annotations
- [x] Clustering Annotations
- [x] Multiple Managers
- [x] Dynamic Cluster Disabling
- [x] Custom Cell Size
- [x] Custom Annotation Views
- [x] Animation Support
- [x] [Documentation](https://efremidze.github.io/Cluster)

## Requirements

- iOS 8.0+
- Xcode 9.0+
- Swift 4 (Cluster 2.x), Swift 3 (Cluster 1.x)

## Usage

Follow the instructions below:

### Step 1: Initialize a ClusterManager object

```swift
let clusterManager = ClusterManager()
```

### Step 2: Add annotations

```swift
let annotation = Annotation()
annotation.coordinate = CLLocationCoordinate2D(latitude: latitude, longitude: longitude)
annotation.style = .color(color, radius: 25) // .image(UIImage(named: "pin"))
clusterManager.add(annotation)
```

### Step 3: Return the pins and clusters

```swift
func mapView(_ mapView: MKMapView, viewFor annotation: MKAnnotation) -> MKAnnotationView? {
    var view = mapView.dequeueReusableAnnotationView(withIdentifier: identifier)
    if view == nil {
        view = ClusterAnnotationView(annotation: annotation, reuseIdentifier: identifier, style: style)
    } else {
        view?.annotation = annotation
    }
    return view
}
```

### Step 4: Reload the annotations

```swift
func mapView(_ mapView: MKMapView, regionDidChangeAnimated animated: Bool) {
    clusterManager.reload(mapView) { finished in
        // handle completion
    }
}
```

## ClusterManagerDelegate

The  `ClusterManagerDelegate` protocol manages displaying annotations. 

```swift
// The size of each cell on the grid at a given zoom level.
func cellSize(for zoomLevel: Double) -> Double { ... }

// Whether to cluster the given annotation.
func shouldClusterAnnotation(_ annotation: MKAnnotation) -> Bool { ... }
```

## Customization

The `ClusterManager` exposes several properties to customize clustering:

```swift
var zoomLevel: Double // The current zoom level of the visible map region.
var maxZoomLevel: Double // The maximum zoom level before disabling clustering.
var minCountForClustering: Int // The minimum number of annotations for a cluster. The default is `2`.
var shouldRemoveInvisibleAnnotations: Bool // Whether to remove invisible annotations. The default is `true`.
var shouldDistributeAnnotationsOnSameCoordinate: Bool // Whether to arrange annotations in a circle if they have the same coordinate. The default is `true`.
var clusterPosition: ClusterPosition // The position of the cluster annotation. The default is `.nearCenter`.
```

### Annotations

The `Annotation` class exposes a `style` property that allows you to customize the appearance.

```swift
var style: ClusterAnnotationStyle // The style of the cluster annotation view.
```

You can further customize the annotations by subclassing `ClusterAnnotationView` and overriding `configure`:

```swift
override func configure() {
    super.configure()

    // customize
}
```

## Removing Annotations

To remove annotations, you can call `remove(_ annotation: MKAnnotation)`. However the annotations will still display until you call `reload()`.

In the case that `shouldRemoveInvisibleAnnotations` is set to `false`, annotations that have been removed may still appear on map until calling `reload()` on visible region.

## Installation

### CocoaPods
To install with [CocoaPods](http://cocoapods.org/), simply add this in your `Podfile`:
```ruby
use_frameworks!
pod "Cluster"
```

### Carthage
To install with [Carthage](https://github.com/Carthage/Carthage), simply add this in your `Cartfile`:
```ruby
github "efremidze/Cluster"
```

## Communication

- If you **found a bug**, open an issue.
- If you **have a feature request**, open an issue.
- If you **want to contribute**, submit a pull request.

## Mentions

- [Natasha The Robot's Newsleter 128](https://swiftnews.curated.co/issues/128#start)
- [Top 5 iOS Libraries May 2017](https://medium.cobeisfresh.com/top-5-ios-libraries-may-2017-6e3ac5077473)

## Credits

* https://github.com/ribl/FBAnnotationClusteringSwift
* https://github.com/choefele/CCHMapClusterController
* https://github.com/googlemaps/google-maps-ios-utils
* https://github.com/hulab/ClusterKit

## License

Cluster is available under the MIT license. See the LICENSE file for more info.
