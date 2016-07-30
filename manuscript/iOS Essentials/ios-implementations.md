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
3. Obviosly if you add in both the places 1 would override 2 because that is a more specific one wrt a view controller

#Customising a UIButton

1. Drag a button onto the storyboard scene
2. Drag the button background to the image assets catalog - either all 3 sizes(@1x,@2x,@3x) or a single png vector format @1x
3. Set the image property on the button in attibutes panel of storyboard to the image background asset added to the image catalog


#Rotating views in iOS
1. Drag any view on the storyboard scene from the object library and style and position the segement control the way you want
2. Ctrl drag from the stoyboard to the view controller code using the assistant editor with the view as an outlet and call it randomView
3. Now to actually rotate the view in the viewDidLoad method add the code. We give the degree in radians to rotate the view.
```swift randomView.CGAffineTransformMakeRotation(3.14) ```

#Sending an email


