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

## Sending an email
* 1st step would be to design the mail form on the storyboard scene and add the subject,body fields as outlets and connect the send mail button as an a action
* Now we need to import the MessageUI framework to send a mail from the view controller, the MessageUI framework en composes all the email related classes and protocols
* Also make the view controller to conform to the MFMailComposeViewControllerDelegate, what we are actually doing is we will be presenting a view controller that will be responsible for sending the email and that controller will respond back to its delegate with information such as mail was sent successfully, mail was canceled, error in sending email etc, so we are essentially conforming to the delegate protocol as we want our view controller to be the delegate of that mail controller which we present eventually
* Now the next step would be to create the mail view controller will the subject, recipients, body
* Next we set the delegate of this newly created mail controller to be the view controller itself
* Now we present the mail controller on the screen
* Now we add the delegate methods to which the mail controller will respond back with information
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
* Drag and drop 4 buttons on the storyboard scene from the object library and style and position the segment control the way you want, the 4 buttons are start, restart, play(pause), stop. Also drag and drop a slider onto the screen. The first button is use to load the music and start playing it. Its used for nothing else. The other 3 buttons will be the most used ones. The restart button will take the music to start and play it from first, the play(pause) is a single toggle button that is use for playing and pausing, the stop button will take the music to start and stop playing it.
* Now lets add the outlet and the actions, add an outlet for the play(pause) button because we will be changing the button title as we play and pause the music file. Add 3 actions for the restart, stop and the start button. Add an outlet and an action(changeAudioTime) for the slider.
* Get the path of the music file by using the following code.
```let path = NSBundle.mainBundle().pathForResource("musicname", ofType: "mp3")```
* Once you have the path of the resource create an instance of NSURL from the path
```let audioUrl = NSURL(fileURLWithPath: path)```
* Next we need to import the AVFoundation framework in the view controller file, this allows us to use the speakers on the iphone to play the music file that we have. There are 2 steps to be done here 1st is to place the code ```import AVFoundation``` in top of view controller file and the other step is to click on the project settings in the file explorer, the go to build phases, and make sure you are selected on target, the click the + button in link binary with libraries, type AVFoundation in the dialog and click add.
* Next step is to create a audio player and give it the contents of the music file that we have with us.
* To start playing the music we can call the play method on the audio player, to pause playing the music we can call the stop method on the audio player, to change the timings of the music we can use the currentTime property of the audio player.
* In the code below we start playing the music once the view is loaded into the memory, we can't present the alert controller here because it can be presented only when the view controller hierarchy is set up, that is why we present the alert controller in the viewDidAppear method which is called after the view hierarchy is setup, ie after the views appear on the screen.
* Always remember that you must stop playing the audio before manipulating with the currentTime of the audio player
* 
```swift
import UIKit
import AVFoundation

class ViewController: UIViewController {

    @IBOutlet weak var pauseOrPlayButton: UIButton!
    
    @IBOutlet weak var audioTimer: UISlider!
    
    let audioPlayer: AVAudioPlayer? = {
        if let audioPath = NSBundle.mainBundle().pathForResource("audio", ofType: "wav") {
            let audioUrl = NSURL(fileURLWithPath: audioPath)
            
            do {
                let player = try AVAudioPlayer(contentsOfURL: audioUrl)
                return player
            } catch let error as NSError {
                print(error.localizedDescription)
                return nil
            }

        } else {
            print("Music file not found.")
            return nil

        }
    }()
    
    override func preferredStatusBarStyle() -> UIStatusBarStyle {
        return .LightContent
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        //Set a custom image for the slider circle for both the states
        audioTimer.setThumbImage(UIImage(named: "slider-thumbnail"), forState: .Normal)
        audioTimer.setThumbImage(UIImage(named: "slider-thumbnail"), forState: .Highlighted)
        
        
        if let player = audioPlayer {
            player.play()
            //duration property is of type NSTimeInterval aka Double, therefore convert it to float
            audioTimer.maximumValue = Float(player.duration)
        }
        
        //Every 0.1 seconds iOS will call the updateAudioTimer method to keep the slider in sync
        NSTimer.scheduledTimerWithTimeInterval(0.1, target: self, selector: #selector(ViewController.updateAudioTimer), userInfo: nil, repeats: true)
    }
    
    override func viewDidAppear(animated: Bool) {
        super.viewDidAppear(animated)
        
        if audioPlayer == nil {
            let alert = UIAlertController(title: "Opps", message: "Audio Error", preferredStyle: .Alert)
            alert.addAction(UIAlertAction(title: "Ok", style: .Default, handler: nil))
            presentViewController(alert, animated: true, completion: nil)
        }
    }

    @IBAction func restartAudio(sender: AnyObject) {
        if let player = audioPlayer {
            player.stop()
            player.currentTime = 0
            player.play()
        }
    }
    
    @IBAction func pauseOrPlayAudio(sender: AnyObject) {
        if let player = audioPlayer {
            if player.playing {
                //User pressed the pause button
                player.stop()
                pauseOrPlayButton.setTitle("Play", forState: .Normal)
            } else {
                player.play()
                pauseOrPlayButton.setTitle("Pause", forState: .Normal)
            }
        }
    }

    @IBAction func stopAudio(sender: AnyObject) {
        if let player = audioPlayer {
            player.stop()
            player.currentTime = 0
            pauseOrPlayButton.setTitle("Play", forState: .Normal)
        }
    }

    @IBAction func changeAudioTime(sender: AnyObject) {
        if let player = audioPlayer {
            player.stop()
            player.currentTime = NSTimeInterval(audioTimer.value)
            //player.prepareToPlay()
            player.play()
        }
    }
    
    func updateAudioTimer() {
        if let player = audioPlayer {
            //currentTime property is of type NSTimeInterval aka Double, therefore convert it to float
            audioTimer.value = Float(player.currentTime)
        }
    }
}
```

## See the fonts
```swift
for family: String in UIFont.familyNames() {
    print("\(family)")
    for names: String in UIFont.fontNamesForFamilyName(family) {
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
