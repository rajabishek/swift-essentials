#Add a segmented controller

1. Drag a segmented control on the storyboard scene from the object library and style and position the segement control the way you want
2. Ctrl drag from the stoyboard to the view controller code using the assistant editor with the segment control as an outlet and call it segmentControl
3. Also ctrl drag from the stoyboard to the view controller code using the assistant editor with the segment control as an action with the value changed event and call the function as valueHasBeenChanged
4. The valueHasBeenChanged method will be called everytime we choose a new segment from the control segment, to see which segment was chosen we can do segmentControl.selectedSegmentIndex which returns an integer of the index of the selected segment.
5. First segment has index 0, second has index 1 and so on.

#Add tint color to navigation bar

1. In the viewDidLoad method of the view controller you can do self.navigationController.navigationBar.tintColor = UIColor.redColor()
2. Since the release of iOS 5, Apple introduced the Appearance API. Developers can easily customize the visual appearance of most UIKit controls, including navigation bar across the entire application, through an appearance proxy of a specific class. For example, to customize the appearance of navigation bar, you use appearance() to get the appearance proxy of the class. The following can be added to didFinishLaunchingWithOptions method of app delegate to change the tint color globally.
```swift 
UINavigationBar.appearance().barTintColor = UIColor.brownColor()
//To change the title font attributes on the navigation bar
if let barFont = UIFont(name: "Avenir-Light", size: 24.0) {
    UINavigationBar.appearance().titleTextAttributes = [NSForegroundColorAttributeName:UIColor.whiteColor(), NSFontAttributeName:barFont]
}
```

