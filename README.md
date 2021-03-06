# FeedCollectionViewController

[![Build Status](https://travis-ci.org/oliveroneill/FeedCollectionViewController.svg?branch=master)](https://travis-ci.org/oliveroneill/FeedCollectionViewController)
[![Version](https://img.shields.io/cocoapods/v/FeedCollectionViewController.svg?style=flat)](http://cocoapods.org/pods/FeedCollectionViewController)
[![License](https://img.shields.io/cocoapods/l/FeedCollectionViewController.svg?style=flat)](http://cocoapods.org/pods/FeedCollectionViewController)
[![Platform](https://img.shields.io/cocoapods/p/FeedCollectionViewController.svg?style=flat)](http://cocoapods.org/pods/FeedCollectionViewController)

A simple interface for creating data feeds so that data can be loaded
dynamically as the user scrolls. This is inspired by scrolling through photos
on Facebook or Instagram.

![Refresh Content by swiping down](Images/refresh.gif)    ![Images load as you scroll](Images/loads_as_scroll.gif)     ![Load bulk content when the user reaches the bottom of the feed](Images/infinite_scroll.gif)    ![Scroll through photos one at a time by tapping them](Images/view_photos.gif)

<sup>Images taken from the example project that uses colours in place of real content.</sup>

FeedCollectionViewController is a generic interface for setting up a simple
feed, whereas ImageFeedCollectionViewController is specifically set up for
images. ImageFeedCollectionViewController uses a [fork](https://github.com/oliveroneill/OOPhotoBrowser)
of [IDMPhotoBrowser](https://github.com/ideaismobile/IDMPhotoBrowser),
so that tapping on images lets you scroll through photos indefinitely.

## Example

To run the example project, clone the repo, and run `pod install` from the Example directory first.
The example project demonstrates the functionality without using any actual content, it creates
coloured images to illustrate its use with a large amount of content.

## Installation

FeedCollectionViewController is available through [CocoaPods](http://cocoapods.org). To install
FeedCollectionViewController, simply add the following line to your Podfile:

```ruby
pod "FeedCollectionViewController"
```

To install ImageFeedCollectionViewController, simply add the following line to
your Podfile:

```ruby
pod "ImageFeedCollectionViewController"
```

## Usage

The set up is quite similar to `UICollectionViewController`, you must specify a
reuse identifier and a `UICollectionViewCell` that should take its data from an
implemented `CellData`.

FeedCollectionViewController:

``` swift
    open func getReuseIdentifier(cell:CellData) -> String {
        // specifies the identifier of the cell, this can differ per cell
    }

    open func getCells(start:Int, callback: @escaping (([CellData]) -> Void)) {
        // get new cell data, this does not actually mean the cell is being shown
        // call `callback` with the new data. `start` is the query starting position
    }

    open func loadCell(cellView: UICollectionViewCell, cell:CellData) {
        // load the cell since it's now actually shown
    }
```

ImageFeedCollectionViewController:

``` swift
    open func getImageReuseIdentifier(cell: ImageCellData) -> String {
        // specifies the identifier of the cell, this can differ per cell
    }

    open func getImageCells(start:Int, callback: @escaping (([ImageCellData]) -> Void)) {
        // get new cell data, this does not actually mean the cell is being shown
        // call `callback` with the new data. `start` is the query starting position
    }

    open func loadImageCell(cellView: UICollectionViewCell, cell:ImageCellData) {
        // load the cell (ie. a thumbnail) since it's now actually shown
    }
```

**NOTE:** `getCells` and `getImageCells` are asynchronous calls and `callback`
will use the main thread. Therefore `callback` must not be called on the
`getImageCells` calling thread and should be moved onto it's own thread before
calling `callback`.

To customise views in the ImageFeedCollectionViewController, you must
implement `SingleImageView` and override relevant methods. The relevant
methods are the same as those in `IDMCaptionView`.
``` swift
    open override func setupCaption() {
        // Setup caption views
    }

    open override func sizeThatFits(_ size: CGSize) -> CGSize {
        // Return the height of the view, the width will be ignored
    }
```
To receive updates when the displayed photo changes, override `didShowPhoto(cell:ImageCellData)`

To receive image download failures from the photo browser, you can override
`imageFailed(cell:ImageCellData)`.

To customise the photo browser's toolbar, you can override
`setupToolbar(toolbar:UIToolbar, cell:ImageCellData)` and make modifications
as needed.

Custom error messages and views are also available for feed retrieval failure
through `getErrorMessage` or `showErrorText`.

## Testing
Testing is done through FBSnapshotTestCase, there are test result files included
in `Example/Tests/ReferenceImages_64/FeedCollectionViewController_Tests.Tests`.
These were run with iPhone SE Simulator, however you can re-run on your own
device by enabling `recordMode` in `setUp()` and then re-running the test with
it off.

## Todo
- Better tests. Currently I've only implemented snapshot tests

## Author

Oliver O'Neill

## License

FeedCollectionViewController is available under the MIT license. See the LICENSE file for more info.
