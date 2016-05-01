# Parsing XML in iOS using NSXMLParser

We are going to populate a table view controller using a simple xml file from the web. The code examples in this article will be written using Swift. I assume that you have already setup the TableViewController to read the input and populate the table. We will be concentrating on parsing the xml file in article.

To parse an xml file in iOS we can make use of the NSXMLParser class from the foundation framework. Since we are working with food menu xml data lets say that our table view controller class is called FoodTableViewController.

## Downloading the XML data

The first thing that we have to do is download the XML data from the web. Lets use this simple XML file that represents a food menu. The NSURLSession class and related classes provide an API for downloading content.

```swift
func refreshData() {
    let defaultSession = NSURLSession(configuration: NSURLSessionConfiguration.defaultSessionConfiguration())
        
    UIApplication.sharedApplication().networkActivityIndicatorVisible = true
    
    let link = "http://www.w3schools.com/xml/simple.xml"
    if let url = NSURL(string: link) {
        let dataTask = defaultSession.dataTaskWithURL(url) {
            data, response, error in
            
            dispatch_async(dispatch_get_main_queue()) {
                UIApplication.sharedApplication().networkActivityIndicatorVisible = false
            }
            
            if let error = error {
                print(error.localizedDescription)
            } else if let httpResponse = response as? NSHTTPURLResponse {
                if httpResponse.statusCode == 200 {
                    let xmlParser = NSXMLParser(data: data!)
                    xmlParser.delegate = self
                    xmlParser.parse()
                }
            }
        }
        dataTask.resume()
    }
}
```
