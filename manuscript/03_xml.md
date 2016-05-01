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
The above code makes a http GET request to get the XML data from the web. Once the XML data is downloaded it is made available in data variable ( which is of type NSData? ) of the completion handler. To understand more about the above code, I would suggest you to read the following. They will give you a lot more insight on how we can use the NSURLSession class, which is a complete suite of networking API methods for uploading and downloading content via HTTP.

> https://www.objc.io/issues/5-ios7/from-nsurlconnection-to-nsurlsession/
> https://www.raywenderlich.com/110458/nsurlsession-tutorial-getting-started
> https://developer.apple.com/library/ios/documentation/Foundation/Reference/NSURLSession_class/

## Parsing the XML 
Now that we have the data we need to parse the XML to get some meaningful information that we require. We use the delegation pattern in iOS to parse the XML data. We create an instance of NSXMLParser class to help with the parsing. The parser will call certain methods on its delegate as it parses the XML document. Lets assign the FoodTableViewController class as the delegate for the parser. The NSXMLParserDelegate protocol defines the optional methods implemented by delegates of NSXMLParser objects.

We will be using 4 delegate methods to parse the file. The 4 delegate methods we will be focusing are 
> parser:didStartElement:namespaceURI:qualifiedName:attributes:
> parser:foundCharacters:
> parser:didEndElement:namespaceURI:qualifiedName:
> parserDidEndDocument:

### didStartElement method 
This method is called by the parser object on its delegate whenever it encounters an opening xml tag.

###foundCharacters method 
This method is called by the parser object on its delegate whenever it is reading data between an opening tag and closing tag. The data between a tag may be read all at once or it may be read in pieces.

### didEndElement method 
This method is called by the parser object on its delegate whenever it encounters a closing xml tag.

### didEndDocument method 
This method is called by the parser object on its delegate when it has completed parsing the entire document.

Consider the following XML data.

```xml
<note>
  <to>Tove</to>
  <from>Jani</from>
  <heading>Reminder</heading>
  <body>Don’t forget me this weekend!</body>
</note>
```

```swift
extension FoodTableViewController: NSXMLParserDelegate {
    
    func parser(parser: NSXMLParser, didStartElement elementName: String, namespaceURI: String?, qualifiedName qName: String?, attributes attributeDict: [String : String]) {
        print("Did start element: \(elementName)")
    }
    
    func parser(parser: NSXMLParser, foundCharacters string: String) {
        print("Found characters: \(string)")
    }
    
    func parser(parser: NSXMLParser, didEndElement elementName: String, namespaceURI: String?, qualifiedName qName: String?) {
        print("Did end element: \(elementName)")
    }
    
    func parserDidEndDocument(parser: NSXMLParser) {
        print("Completed parsing document")
    }
}
```

If we implement the delegate methods as shown above then the output that we get is.

```txt
Did start element: note
Found characters:

Did start element: to
Found characters: Tove
Did end element: to
Found characters:

Did start element: from
Found characters: Jani
Did end element: from
Found characters:

Did start element: heading
Found characters: Reminder
Did end element: heading
Found characters:

Did start element: body
Found characters: Don’t forget me this weekend!
Did end element: body
Found characters:

Did end element: note
Completed parsing document
```
We will be using the following XML file for our demo.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<breakfast_menu>
	<name>This is a simple name data</name>
	<food>
		<name>Belgian Waffles</name>
		<price>$5.95</price>
		<desc>Two of our famous Belgian Waffles with plenty of real maple syrup</desc>
		<calories>650</calories>
	</food>
	<food>
		<name>Strawberry Belgian Waffles</name>
		<price>$7.95</price>
		<desc>Light Belgian waffles covered with strawberries and whipped cream</desc>
		<calories>900</calories>
	</food>
	<food>
		<name>Berry-Berry Belgian Waffles</name>
		<price>$8.95</price>
		<desc>Light Belgian waffles covered with an assortment of fresh berries and whipped cream</desc>
		<calories>900</calories>
	</food>
	<food>
		<name>French Toast</name>
		<price>$4.50</price>
		<desc>Thick slices made from our homemade sourdough bread</desc>
		<calories>600</calories>
	</food>
	<food>
		<name>Homestyle Breakfast</name>
		<price>$6.95</price>
		<desc>Two eggs, bacon or sausage, toast, and our ever-popular hash browns</desc>
		<calories>950</calories>
	</food>
</breakfast_menu>
```

