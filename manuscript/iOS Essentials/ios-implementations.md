## Add a segmented controller

* Drag a segmented control on the storyboard scene from the object library and style and position the segment control the way you want
* Ctrl drag from the storyboard to the view controller code using the assistant editor with the segment control as an outlet and call it segmentControl
* Also Ctrl drag from the storyboard to the view controller code using the assistant editor with the segment control as an action with the value changed event and call the function as valueHasBeenChanged
* The valueHasBeenChanged method will be called every time we choose a new segment from the control segment, to see which segment was chosen we can do segmentControl.selectedSegmentIndex which returns an integer of the index of the selected segment.
* First segment has index 0, second has index 1 and so on.

## Add tint color to navigation bar

* In the viewDidLoad method of the view controller you can do ```self.navigationController.navigationBar.tintColor = UIColor.redColor()```
* Since the release of iOS 5, Apple introduced the Appearance API. Developers can easily customize the visual appearance of most UIKit controls, including navigation bar across the entire application, through an appearance proxy of a specific class. For example, to customize the appearance of navigation bar, you use appearance() to get the appearance proxy of the class. The following can be added to didFinishLaunchingWithOptions method of app delegate to change the tint color globally.
```swift 
UINavigationBar.appearance().barTintColor = UIColor.brownColor()
//To change the title font attributes on the navigation bar
if let barFont = UIFont(name: "Avenir-Light", size: 24.0) {
    UINavigationBar.appearance().titleTextAttributes = [NSForegroundColorAttributeName:UIColor.whiteColor(), NSFontAttributeName:barFont]
}
```
* Obviously if you add in both the places 1 would override 2 because that is a more specific one with respect to a view controller

## Customizing a UIButton

* Drag a button onto the storyboard scene
* Drag the button background to the image assets catalog - either all 3 sizes(@1x,@2x,@3x) or a single png vector format @1x
* Set the image property on the button in attributes panel of storyboard to the image background asset added to the image catalog


## Rotating views in iOS
* Drag any view on the storyboard scene from the object library and style and position the segment control the way you want
* Ctrl drag from the storyboard to the view controller code using the assistant editor with the view as an outlet and call it randomView
* Now to actually rotate the view in the viewDidLoad method add the code. We give the degree in radians to rotate the view.
```randomView.CGAffineTransformMakeRotation(3.14) ```



## See the fonts
```swift
for family in UIFont.familyNames() {
    print(family)
    for names in UIFont.fontNamesForFamilyName(family) {
        print("== \(names)")
    }
}
```
The following code is used to add line spacing to a textview
```swift
let paragraphStyle: NSMutableParagraphStyle = NSMutableParagraphStyle()
paragraphStyle.lineHeightMultiple = 20.0
paragraphStyle.maximumLineHeight = 20.0
paragraphStyle.minimumLineHeight = 20.0
let attributes = [NSFontAttributeName: UIFont(name: "Karla-Regular", size: 14.0)!, NSParagraphStyleAttributeName: paragraphStyle]
cell.textView.attributedText = NSAttributedString(string: "you text string goes here", attributes: attributes)
```

## Leveraging Alamofire & SwiftyJSON for REST API Calls
* The session.dataTaskWithRequest works just fine for simple cases but these days lots of apps have tons of web service calls that are just begging for better handling, like a higher level of abstraction, concise syntax, simpler streaming, pause/resume, progress indicators.
* In Objective-C, we had the AFNetworking addressing this problem. In Swift, Alamofire is our option for elegance.
* Remember when writing everything from scratch that the JSON parsing was pretty ugly, SwiftyJSON can help fix this problem.
* We’ll need to bypass ATS just like last time. Add an exception to App Transport Security for http://jsonplaceholder.typicode.com/ by adding the following to your info.plist
* First add the Alamofire library to your project, refer https://github.com/Alamofire/Alamofire#installation


## Dismissing the iOS Keyboard
* One way is to call resignFirstResponder on the ui element that is active
* Another way is to add touchesBegan method in the view controller


## Adding search feature in table view in iOS
* Create a UISearchController instance as an instance of the view controller, give the searchResultsController while creating as nil meaning we would like to use the same table for search results also
* Set the searchResultsUpdater property of search controller as self(view controller), meaning view controller would be responsible for updating the search results for the search controller. View controller has to conform to UISearchResultsUpdating protocol.
* Set the dimsBackgroundDuringPresentation property on search controller to false, usually the search controller will dim the background while showing the search results, but since we are using the same table view to show the results we would not want this behavior.
* On the view controller set the definesPresentationContext property to true, it means that the search bar must be hidden when navigating away from the view controller even though it is currently active.
* Set the scope button titles for the search bar
* Set the delegate of the search bar to the view controller. View controller has to conform to UISearchBarDelegate protocol.
* Add the search bar as the header of the table view
* updateSearchResultsForSearchController method is called every time a user changes the contents if the search bar text. It is method of the UISearchResultsUpdating protocol, it is the only compulsory method of the protocol.
* selectedScopeButtonIndexDidChange method is called every time a different scope button is pressed. This method is from the UISearchBarDelegate protocol, however it is not a compulsory method.
```swift
import UIKit

class DevicesTableViewController: UITableViewController {
    
    let devices = [Device(name: "Iphone 5c", type: "Mobile"), Device(name: "Oneplus x", type: "Mobile"), Device(name: "Macbook Pro", type: "Laptop"), Device(name: "Apple Watch", type: "Watch"), Device(name: "Moto 360", type: "Watch"), Device(name: "Samsung Galaxy", type: "Mobile"), Device(name: "Dell Vostro", type: "Laptop"), Device(name: "Dell Inspiron", type: "Laptop")]
    
    var filteredDevices = [Device]()
    
    let searchController = UISearchController(searchResultsController: nil)
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        searchController.searchResultsUpdater = self
        searchController.dimsBackgroundDuringPresentation = false
        definesPresentationContext = true
        searchController.searchBar.scopeButtonTitles = ["All", "Mobile", "Laptop", "Watch"]
        searchController.searchBar.delegate = self
        tableView.tableHeaderView = searchController.searchBar
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }

    // MARK: - Table view data source
    override func numberOfSectionsInTableView(tableView: UITableView) -> Int {
        return 1
    }

    override func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        if searchController.active {
            return filteredDevices.count
        }
        return devices.count
    }

    override func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCellWithIdentifier("Cell", forIndexPath: indexPath)

        // Configure the cell...
        let device = searchController.active ? filteredDevices[indexPath.row] : devices[indexPath.row]
        cell.textLabel?.text = device.name
        cell.detailTextLabel?.text = device.type

        return cell
    }
}

extension DevicesTableViewController: UISearchResultsUpdating, UISearchBarDelegate {
    
    func filterContentForSearchText(searchText: String, scope: String = "All") {
        
        if searchText == "" {
            filteredDevices = devices.filter { device in
                return (scope == "All") || (device.type == scope)
            }
        } else {
            filteredDevices = devices.filter { device in
                let categoryMatch = (scope == "All") || (device.type == scope)
                return categoryMatch && device.name.lowercaseString.containsString(searchController.searchBar.text!.lowercaseString)
            }
        }
        
        tableView.reloadData()
    }
    
    func updateSearchResultsForSearchController(searchController: UISearchController) {
        let searchBar = searchController.searchBar
        let scope = searchBar.scopeButtonTitles![searchBar.selectedScopeButtonIndex]
        filterContentForSearchText(searchController.searchBar.text!, scope: scope)
    }
    
    func searchBar(searchBar: UISearchBar, selectedScopeButtonIndexDidChange selectedScope: Int) {
        let searchBar = searchController.searchBar
        let scope = searchBar.scopeButtonTitles![searchBar.selectedScopeButtonIndex]
        filterContentForSearchText(searchController.searchBar.text!, scope: scope)
    }
}
```

## Diving deep into Code Data
* Core data is a schema-drive object graph management and persistence framework
* 
