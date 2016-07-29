#Add a segmented controller

1. Drag a segmented control on the storyboard scene from the object library and style and position the segement control the way you want
2. Ctrl drag from the stoyboard to the view controller code using the assistant editor with the segment control as an outlet and call it segmentControl
3. Also ctrl drag from the stoyboard to the view controller code using the assistant editor with the segment control as an action with the value changed event and call the function as valueHasBeenChanged
4. The valueHasBeenChanged method will be called everytime we choose a new segment from the control segment, to see which segment was chosen we can do segmentControl.selectedSegmentIndex which returns an integer of the index of the selected segment.
5. First segment has index 0, second has index 1 and so on.
