# Understanding Windows and Screens

A window handles the overall presentation of your app’s user interface. Windows work with views (and their owning view controllers) to manage interactions with—and changes to—the visible view hierarchy.

Every app has one window that displays the app’s user interface on an iOS-based device display. If an external display is connected to the device, an app can create a second window to present content on that display as well.

Reference: https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/WindowAndScreenGuide/WindowScreenRolesinApp/WindowScreenRolesinApp.html#//apple_ref/doc/uid/TP40012555-CH4-SW3

Read the above document to understand better on what a window is in a iOS app ? What is the role of a window. Read the following subheadings in detail.

The Role of the Window in an iOS App
A Window’s Root View Contains Your Content
Xcode and Storyboards Can Create, Configure, and Load Your Window

To create your app’s window programmatically. You would use code something like this to programmatically create a window, install the root view controller, and make the window visible:
```swift
func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
    //Create a new instance of window object and set its size to the full bounds of the screen object
    window = UIWindow(frame: UIScreen.mainScreen().bounds)
    
    //Set the root view controller of the window object
    //ViewController is a swift class file that extends the UIViewController
    window?.rootViewController = UINavigationController(rootViewController: ViewController())
    
    //Make the window visible on the screen
    window?.makeKeyAndVisible()
    
    return true
}
```

Before the initial view controller is displayed, your app delegate is called to give you a chance to configure the view controller. Here we are saying forget everything you have done so far (Instantiating a window, loading the main storyboard and instantiating the initial view controller and assigning it to the window’s rootViewController property and then making the window visible ), instead create a new instance of window object and set the rootViewController to be an object of UINavigationController and make it visible. In this way we are neglecting the storyboard and creating everything programatically.

