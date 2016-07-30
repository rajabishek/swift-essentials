## Add a segmented controller

1. Drag a segmented control on the storyboard scene from the object library and style and position the segment control the way you want
2. Ctrl drag from the storyboard to the view controller code using the assistant editor with the segment control as an outlet and call it segmentControl
3. Also Ctrl drag from the storyboard to the view controller code using the assistant editor with the segment control as an action with the value changed event and call the function as valueHasBeenChanged
4. The valueHasBeenChanged method will be called every time we choose a new segment from the control segment, to see which segment was chosen we can do segmentControl.selectedSegmentIndex which returns an integer of the index of the selected segment.
5. First segment has index 0, second has index 1 and so on.

## Add tint color to navigation bar

1. In the viewDidLoad method of the view controller you can do ```self.navigationController.navigationBar.tintColor = UIColor.redColor()```
2. Since the release of iOS 5, Apple introduced the Appearance API. Developers can easily customize the visual appearance of most UIKit controls, including navigation bar across the entire application, through an appearance proxy of a specific class. For example, to customize the appearance of navigation bar, you use appearance() to get the appearance proxy of the class. The following can be added to didFinishLaunchingWithOptions method of app delegate to change the tint color globally.
```swift 
UINavigationBar.appearance().barTintColor = UIColor.brownColor()
//To change the title font attributes on the navigation bar
if let barFont = UIFont(name: "Avenir-Light", size: 24.0) {
    UINavigationBar.appearance().titleTextAttributes = [NSForegroundColorAttributeName:UIColor.whiteColor(), NSFontAttributeName:barFont]
}
```
3. Obviously if you add in both the places 1 would override 2 because that is a more specific one with respect to a view controller

## Customizing a UIButton

1. Drag a button onto the storyboard scene
2. Drag the button background to the image assets catalog - either all 3 sizes(@1x,@2x,@3x) or a single png vector format @1x
3. Set the image property on the button in attributes panel of storyboard to the image background asset added to the image catalog


## Rotating views in iOS
1. Drag any view on the storyboard scene from the object library and style and position the segment control the way you want
2. Ctrl drag from the storyboard to the view controller code using the assistant editor with the view as an outlet and call it randomView
3. Now to actually rotate the view in the viewDidLoad method add the code. We give the degree in radians to rotate the view.
```randomView.CGAffineTransformMakeRotation(3.14) ```

## Sending an email
1. 1st step would be to design the mail form on the storyboard scene and add the subject,body fields as outlets and connect the send mail button as an a action
2. Now we need to import the MessageUI framework to send a mail from the view controller, the MessageUI framework en composes all the email related classes and protocols
3. Also make the view controller to conform to the MFMailComposeViewControllerDelegate, what we are actually doing is we will be presenting a view controller that will be responsible for sending the email and that controller will respond back to its delegate with information such as mail was sent successfully, mail was canceled, error in sending email etc, so we are essentially conforming to the delegate protocol as we want our view controller to be the delegate of that mail controller which we present eventually
4. Now the next step would be to create the mail view controller will the subject, recipients, body
5. Next we set the delegate of this newly created mail controller to be the view controller itself
6. Now we present the mail controller on the screen
7. Now we add the delegate methods to which the mail controller will respond back with information
```
import Foundation
import UIKit
import MessageUI
 
class ViewController: UIViewController, MFMailComposeViewControllerDelegate {
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }
    
    @IBAction func sendEmailButtonTapped(sender: AnyObject) {
        let mailComposeViewController = configuredMailComposeViewController()
        if MFMailComposeViewController.canSendMail() {
            self.presentViewController(mailComposeViewController, animated: true, completion: nil)
        } else {
            self.showSendMailErrorAlert()
        }
    }
    
    func configuredMailComposeViewController() -> MFMailComposeViewController {
        let mailComposeViewController = MFMailComposeViewController()

        // Extremely important to set the --mailComposeDelegate-- property, NOT the --delegate-- property
        mailComposeViewController.mailComposeDelegate = self 
        
        mailComposeViewController.setToRecipients(["someone@somewhere.com"])
        mailComposeViewController.setSubject("Sending you an in-app e-mail...")
        mailComposeViewController.setMessageBody("Sending e-mail in-app is not so bad!", isHTML: false)
        
        return mailComposeViewController
    }
    
    func showSendMailErrorAlert() {
        let sendMailErrorAlert = UIAlertView(title: "Could Not Send Email", message: "Your device could not send e-mail.  Please check e-mail configuration and try again.", delegate: self, cancelButtonTitle: "OK")
        sendMailErrorAlert.show()
    }
    
    // MARK: MFMailComposeViewControllerDelegate Method
    func mailComposeController(controller: MFMailComposeViewController!, didFinishWithResult result: MFMailComposeResult, error: NSError!) {
    	switch (result) {
	        
	        case MFMailComposeResultSent:
	            print("The email message was queued in the user’s outbox. It is ready to send the next time the user connects to email.")
	        
	        case MFMailComposeResultSaved:
	            print("The email message was saved in the user’s Drafts folder.")
	        
	        case MFMailComposeResultCancelled:
	            print("The user canceled the operation. No email message was queued.")
	        
	        case MFMailComposeResultFailed:
	            print("The email message was not saved or queued, possibly due to an error.")
	        
	        default:
	            print("An error occurred when trying to compose this email")
	    }
        controller.dismissViewControllerAnimated(true, completion: nil)
    }
}
```

## Using Audio in iOS
1. Drag and drop 4 buttons on the storyboard scene from the object library and style and position the segment control the way you want, the 4 buttons are start, restart, play(pause), stop. The first button is use to load the music and start playing it. Its used for nothing else. The other 3 buttons will be the most used ones. The restart button will take the music to start and play it from first, the play(pause) is a single toggle button that is use for playing and pausing, the stop button will take the music to start and stop playing it.
2. Ctrl drag from the storyboard to the view controller code using the assistant editor with the segment control as an outlet and call it segmentControl
3. Now lets add the outlet and the actions, add an outlet for the play(pause) button because we will be changing the button title as we play and pause the music file. Add 3 actions for the restart, stop and the start button.
4. Get the path of the music file by using the following code.
```let path = NSBundle.mainBundle().pathForResource("musicname", ofType: "mp3")```
5. Once you have the path of the resource create an instance of NSURL from the path
```let audioUrl = NSURL(fileURLWithPath: path)```

