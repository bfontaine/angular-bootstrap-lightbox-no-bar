# angular-bootstrap-lightbox-no-bar

Note: this is a fork of the [original project][origin] that doesn’t depend on
`angular-loading-bar` nor `ngTouch`.

[origin]: https://github.com/compact/angular-bootstrap-lightbox

What follows is the original README, slightly modified to remove references to
`angular-loading-bar` and `ngTouch`:


This lightbox displays images using an AngularUI Bootstrap Modal.

When the lightbox is opened, navigating to the previous/next image can be
achieved by clicking buttons above the image, clicking the left/right arrow
keys. The escape key for closing the lightbox modal is automatically binded by
AngularUI Bootstrap.

Each image is scaled to fit inside the window. An optional image caption
overlays the top left corner of the image.


## Demo

[Demo](http://bfontaine.github.io/angular-bootstrap-lightbox-no-bar/)

## Install

```
bower install angular-bootstrap-lightbox-no-bar --save
```

Stylesheet with dependencies:

```html
<link rel="stylesheet" href="bower_components/bootstrap/dist/css/bootstrap.css">
<link rel="stylesheet" href="bower_components/angular-bootstrap-lightbox-no-bar/dist/angular-bootstrap-lightbox-no-bar.css">
```

Script with dependencies:

```html
<script src="bower_components/angular/angular.js"></script>
<script src="bower_components/angular-bootstrap/ui-bootstrap-tpls.js"></script>
<script src="bower_components/angular-bootstrap-lightbox-no-bar/dist/angular-bootstrap-lightbox-no-bar.js"></script>
```

Add the module as a dependency:

```js
angular.module('app', ['bootstrapLightbox']);
```

## Example

Gallery:

```html
<ul ng-controller="GalleryCtrl">
  <li ng-repeat="image in images">
    <a ng-click="openLightboxModal($index)">
      <img ng-src="{{image.thumbUrl}}" class="img-thumbnail" alt="">
    </a>
  </li>
</ul>
```

Controller:

```js
angular.module('app').controller('GalleryCtrl', function ($scope, Lightbox) {
  $scope.images = [
    {
      'url': '1.jpg', // required property
      'width': 123,   // required property
      'height': 456,  // required property
      'thumbUrl': 'thumb1.jpg',
      'caption': 'Optional caption'
    },
    {
      'url': '2.gif',
      'width': 789,
      'height': 1012,
      'thumbUrl': 'thumb2.jpg'
    },
    {
      'url': '3.png',
      'width': 345,
      'height': 678,
      'thumbUrl': 'thumb3.png'
    }
  ];

  $scope.openLightboxModal = function (index) {
    Lightbox.openModal($scope.images, index);
  };
});
```

## Configuration

```js
angular.module('app').config(function (LightboxProvider) {
  // set a custom template
  LightboxProvider.templateUrl = 'lightbox.html';

  /**
   * Calculate the max and min limits to the width and height of the displayed
   *   image (all are optional). The max dimensions override the min
   *   dimensions if they conflict.
   * @param  {Object} dimensions Contains the properties windowWidth,
   *   windowHeight, imageWidth, imageHeight.
   * @return {Object} May optionally contain the properties minWidth,
   *   minHeight, maxWidth, maxHeight.
   */
  LightboxProvider.calculateImageDimensionLimits = function (dimensions) {
    return {
      'minWidth': 100,
      'minHeight': 100,
      'maxWidth': dimensions.windowWidth - 102,
      'maxHeight': dimensions.windowHeight - 136
    };
  };

  /**
   * Calculate the width and height of the modal. This method gets called
   *   after the width and height of the image, as displayed inside the modal,
   *   are calculated. See the default method for cases where the width or
   *   height are 'auto'.
   * @param  {Object} dimensions Contains the properties windowWidth,
   *   windowHeight, imageDisplayWidth, imageDisplayHeight.
   * @return {Object} Must contain the properties width and height.
   */
  LightboxProvider.calculateModalDimensions = function (dimensions) {
    return {
      'width': Math.max(500, dimensions.imageDisplayWidth + 42),
      'height': Math.max(500, dimensions.imageDisplayHeight + 76)
    };
  };
});
```
