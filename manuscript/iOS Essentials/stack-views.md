#Stack View in iOS
iOS 9 includes a new UIView subclass called UIStackView, that simplifies the ways in which we defined layouts for most of the user interfaces. Its comes with full support in the interface builder and reduces a lot of headache that arise when working with complicated user interfaces and large number of auto layout constraints. The options that apple has given in terms of laying out interface elements on the screen has been pretty fabulous. 

In WWDC 2012 when apple introduced iOS 6 it introduced the concept of auto layouts to support multiple screen sizes. Autolayout changed the way how developer and designers worked and thought in terms of laying out the elements on the screen, everything was relationships and values that were with respect to other elements. 

In WWDC 2014 apple introduced adaptive layout that sits on top of auto layout  and allows for universal layout meaning that now we just have a single storyboard bor deigning for iphone or ipad. It introced the concept of sizes classes and simplified the whole process, essentialy allowing to developer layout that adapt to different screen sizes. 

In 2015 Apple introduced a new feature for laying out elements called as UIStackView. You should understand here that auto layout and adaptive layout are features that are required. Whereas stack view is a feature that in not absolutely neccessary for you to implement. By using it it just makes your life easy, its not that without stack views we can't build cerating type of user interfaces. If you use stack view its just that you can be quicker and maintaing the project in a better way.


#NSLayoutConstraints
Whenever possible, use Interface Builder to set your constraints. Interface Builder provides a wide range of tools to visualize, edit, manage, and debug your constraints. By analyzing your constraints, it also reveals many common errors at design time, letting you find and fix problems before your app even runs.

Interface Builder can manage an ever-growing number of tasks. You can build almost any type of constraint directly in interface builder (see Working with Constraints in Interface Builder). You can also specify size-class-specific constraints (see Debugging Auto Layout), and with new tools like stack views, you can even dynamically add or remove views at runtime (see Dynamic Stack View). However, some dynamic changes to your view hierarchy can still be managed only in code.

You have three choices when it comes to programmatically creating constraints: You can use layout anchors, you can use the NSLayoutConstraint class, or you can use the Visual Format Language.


