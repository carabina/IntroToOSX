# Introduction to developing for OS X: Your First App

This introductory series about building apps on OS X has covered a lot of ground. If you haven't already read them, go and take a look now so that you have a grounding from which to build in this tutorial. This is what you've already learned:

- __Tooling__ In part one you learned about the many facets of Xcode - and had a glimpse of how you could use it to start developing for OS X.
- __App Anatomy__ The second part covered a lot of the theory behind how OS X app are constructed - from the data layer, through the binary assets, to designing the UI.

In this final part, you're going to dive deep into the world of OS X development, by creating your first ever app!

The app you're going to build is a magic-8 ball app, to help you make all those difficult decisions in day-to-day life. You'll start from scratch - first creating a "Hello World!" app before moving on to creating an app to solve all your problems!

> __Note:__ This app requires OS X El Capitan, so if you don't have it - make sure you upgrade to it before trying to follow the tutorial.

## Creating the Project

Open Xcode and click __Create a new Xcode project__ to start your new app. Choose __OS X \ Application \ Cocoa Application__:

![Project Template](images/01_project_template.png)

Set the product name __MagicEight__, the language to __Swift__ and ensure that the __Use Storyboards__  is checked:

![Project Options](images/02_project_options.png)

Choose a location on disk to save your project and then click __Create__ to open your empty project.

Build and run __MagicEight__ to check that everything is working:

![1st Build and Run](images/03_bar_01.png)

Great - so the app runs, but it does _absolutely nothing_ yet. In the next section you're going to turn your app into the first app you should always write when learning a new platform—hello world!

## Hello World!

The user interface for an OS X app is provided by the storyboard file, using interface builder. This allows you to design the UI in a very visual manner, and reduces the amount of complex code you have to write.

Open __Main.storyboard__ by selecting it in the Project Navigator in the left-hand pane:

![Storyboard](images/04_project_navigator.png)

You'll now see the storyboard open in the center panel:

![Storyboard](images/05_storyboard.png)

The template storyboard contains three components:

- __Menu Bar__ Controls the menu that appears when the app runs.
- __Window Controller__ Manages the default window that appears. This uses one or more view controllers to manage their content.
- __View Controller__ Responsible for a region of the window - both for display and handling user interaction.

During this introductory tutorial you'll concentrate your efforts on the view controller.

First up, you need to add a label to the view controller to display your welcome text. A label is actually a special type of `NSTextField` that's designed for displaying a single line of non-editable text - perfect for "Hello World".

To add your first label, follow this set of steps. There's an annotated screenshot so you can see to do:

1. If it's not already visible use the icon in the toolbar to show the utilities panel on the right-hand side.
2. Use the icon towards the bottom of the panel to open the __Object Library__. This, as the name suggests, is a catalog of things you can drag onto the storyboard canvas to build your layout.
3. Use the search box at the bottom to search for __label__.
4. You'll see the label object highlighted in the library.

![Object Library](images/06_object_library.png)

Now that you've found a label, you can set about using it. Drag the label from the object library onto the view controller:

![Label on View Controller](images/07_label_vc.png)

To change the text content of the label, select the __Attributes Inspector__ tab of the Utilities panel, locate the __Title__ field and enter __Hello World!__:

![Hello world](images/08_hello_world.png)

You can use the attributes inspector to control the appearance and behavior of the different controls that OS X offers. Find the __Font__ entry, and click the __T__ icon to the right to open the font panel. Change the style to __Thin__ and the size to __40__. Click __Done__ to save the changes:

![Font](images/09_font.png)

Cast your eyes back over to the view controller canvas and you'll see something strange:

![Wrong Size](images/10_wrong_size.png)

Although you've adjusted the text and size of the label, it has remained the same size, causing nearly all of the text to disappear. Why is this?

Although you placed the label on the view controller's canvas, you haven't actually told OS X _how_ it should position it. The view controller appears at one size in the storyboard, but once it's running in the app, the user can change the window size - so you need to specify how the label should be positioned for _any_ window size.

This sounds like it might be a really difficult task, but luckily Apple has your back. OS X uses a powerful layout engine called Auto Layout, in which the relationships between different components of the view are expressed as rules. These rules are then interpreted at runtime to calculate the size and position of each element within the view.

> __Note:__ Auto Layout is a complex topic, and this introductory tutorial won't cover it in any great depth. If you'd like to read more on the topic then there are some excellent tutorials on the site targeted to Auto Layout on iOS—luckily the engine is almost identical on the two platforms. TODO: Links

Ensure that the label is selected in the view controller, and then click the __Align__ button in the lower toolbar. Select __Horizontally in Container__, ensuring the value is set to __0__ before clicking __Add 1 Constraint__ to add the constraint:

![Align](images/11_align.png)

This will ensure that the label will always appear in the center of the window, irrespective of its width. You'll notice that some angry red lines have appeared in the storyboard:

![Red Lines](images/12_angry_red.png)

This is because you've provided constraints (i.e. rules) to specify the location of the label in the horizontal direction, but you've not provided any information about the vertical axis yet.

Once again, ensure that the label is selected in the view controller, and this time click the __Pin__ menu on the Auto Layout toolbar at the bottom. Enter __30__ in the top constraint box and ensure that the I-bar beneath it is solid red. Set the value of Update Frames to __Items of New Constraints__ before clicking __Add 1 Constraint__:

![Pin](images/13_pin.png)

Immediately you'll notice the view controller update—you can now see the entire label, and the angry red lines have changed to calming blue:

![Happy](images/14_happy_storyboard.png)

And that's your "Hello World" app done (for now). Use the "play" button to build and run, and inspect your handiwork:

![BAR2](images/15_bar_02.png)

Congratulations! Not very personal though is it? In the next section you'll discover how to make the traditional greeting a little more friendly.


## Handling User Input

In this section you're going to add a text field to allow the user to enter their name so that you can welcome them personally.

Before you can jump in to some code, you need to add the new user interface elements and configure their layout.

### Control Layout

As you did before, use the object library to locate and then drag a __Text Field__ and a __Push Button__ onto the view controller. Position them above the "Hello World!" label:

![Positioning](images/16_position_textfield.png)

Remember that placing the controls on the canvas isn't enough for OS X to understand how you want them positioned as the window changes size. You need to add some constraints to convey your wishes.

Select the text field, and then __Control-Click__ the button to select both simultaneously. Then click the __Stack__ icon on the Auto Layout toolbar at the bottom of the storyboard:

![Stack](images/17_stack.png)

This has created a new stack view containing the text field and the button. A stack view automatically generates the layout constraints required to position the contained views in a line. You can use the attributes inspector to configure many common properties of the stack view.

> __NOTE:__ `NSStackView` has been in OS X since 10.9, but received a significant update in 10.11 (El Capitan)—in line with its introduction (`UIStackView`) in iOS. Stack views are similar on both platforms, so you can check out the iOS tutorial on stack views to get up to speed. Or hang tight—there's a tutorial on `NSStackView` on its way.

Once you've started stacking it's difficult to stop. This time, you're going to stack your newly created stack view with the "Hello World!" label—in a vertical stack view.

Use the button to the left of the lower toolbar to show the Document Outline and then locate the "Hello World" control and the existing stack view. __Command-click_ them to select both:

![Document Outline](images/18_document_outline.png)

As you did before, use the __Stack__ button on the bottom toolbar to create a new stack view:

![Stack](images/19_stack.png)

While this new stack view is selected, open the __Attributes Inspector__ and set the Alignment to __Center X__, and the spacing to __20__:

![Stack Config](images/20_stack_config.png)

This ensures that the label and the upper stack view are nicely centered and there's a gap of 20 points between them.

A couple more bits of layout to complete before you can turn your attention to the task of handling user input.

The stack view handles the positioning of its content relative to each other, but it needs to be positioned within the view controller's view. Select the outer stack view and use the __Align__ auto layout menu to center it vertically within its container:

![Align](images/21_align.png)

And use the __Pin__ menu to pin the stack view to the top, with a spacing of __30__:

![Pin](images/22_pin.png)

Finally, to ensure that the text field always has the space for the user's name, you will fix its width.

Select the text field and use the __Pin__ menu in the bottom toolbar to specify a __Width__ of __100__. Click __Add 1 Constraint__ to save the new constraint:

![Text field Width](images/23_textfield_width.png)

With that your layout is pretty much complete—it should look like the following:

![Layout](images/24_layout.png)

Now you can turn your attention back to those new controls you added.


### Outlets and Actions

Select the text field and open the __Attributes Inspector__. In the Placeholder field type __Name__:

![Name](images/25_name.png)

This grayed out text will instruct the user what the field is for, and will disappear as soon as they start typing.

Select the button, and again open the __Attributes Inspector__. Set the Title to __Welcome__:

![Welcome](images/26_welcome.png)

That's the cosmetic aspect done. Time to wire these two controls into code.

OS X provides a way to interact with the UI designed in a storyboard from code using outlets and actions:

- An outlet is a property in code that is connected to a component in the storyboard. This allows you to access the properties of controls within a storyboard from your code.
- Actions are functions in code that are invoked when the user interacts with the components in the UI - e.g. by clicking on a button.

You are going to add outlets for the text field and the label, and an action that will be called when the user clicks the welcome button.

The view controller you've been working on already has a skeleton class associated with it in code—in __ViewController.swift__. This is where you'll add the outlets and action.

Open the assistant editor using the button in the toolbar:

![Assistant Editor](images/27_assistant_editor.png)

This will split the screen and show a code file alongside the storyboard. It should be displaying __ViewController.swift__, but if it isn't use the jump bar to select __Automatic \ ViewController.swift__:

![Jump Bar](images/28_jump_bar.png)

__Right-click-drag__ (or __Control-drag__) from the text field in the storyboard over to the line above `override func viewDidLoad()` in __ViewController.swift__:

![Drag](images/29_textfield_outlet.png)

Ensure that __Outlet__ is selected and call the new outlet __nameTextField__:

![Name text field](images/30_name_textfield.png)

This will add the following line of code to the `ViewController` class and updates the storyboard to automatically connect the text field to this property when loading the view controller:

```swift
@IBOutlet weak var nameTextField: NSTextField!
```

Repeat exactly the same process for the "Hello World!" label, this time specifying that the outlet should be called __welcomeLabel__:

![Welcome label](images/31_welcome_label.png)

Now time to turn your attention to the button. Once again __Control-drag__ from the button in the storyboard over to the `ViewController` class:

![Drag](images/32_button_action.png)

Change the Connection to __Action__ and name it __handleWelcome__:

![Action settings](images/33_action_settings.png)

Click __Connect__ to make the connection and Xcode will add the following empty method definition to the `ViewController` class:

```swift
@IBAction func handleWelcome(sender: AnyObject) {
}
```

This method will be called every time the user clicks the button, but it doesn't currently do anything.

Add the following line to the `handleWelcome(_:)` method body:

```swift
welcomeLabel.stringValue = "Hello \(nameTextField.stringValue)!"
```

This updates the `stringValue` property of the `welcomeLabel` to a welcome message constructed using the `stringValue` of the `nameTextField`. As you might have guessed, `stringValue` represents the value currently displayed by a text-based control—be it user-entered or defined in the storyboard.

That's all the code you need to get your personalized version of "Hello World" going! Build and run to give it a try. Once the app has started, try entering your name in the __Name__ box and clicking the __Welcome__ button. You'll see your very own version of "Hello world!":

![Hello sam](images/34_hello_sam.png)

Pretty cool eh? Well, the fun doesn't stop there. Next up you're going to learn about adding images to your app, to create an amazing magic-8 ball utility!


## Assets

You were promised a magic 8-ball app at the start of this tutorial, and so far you've seen no mention of a ball. Well, that's all about to change.

An 8-ball is pretty distinctive, and it wouldn't be a very exciting 8-ball app without some visualization. [Download](TODO: URL) the images you'll need to make your app look swish. Uncompress the zip file and you'll find two images inside—representing the two sides of a magic 8-ball:

![Assets](images/35_assets.png)

Image assets are stored in an asset catalog within an OS X app. This catalog manages the different imagery and the different resolutions required for the app icons and the in-app images. 

Open __Assets.xcassets__ by selecting it in the project navigator:

![Asset catalog](images/36_asset_catalog.png)

You can see that the catalog currently only contains one item - __AppIcon__. This is where you would put the artwork to give your app a cool icon.

You need to add the downloaded images to the asset catalog. Locate the images in finder, and __drag__ them both into the asset catalog:

![Drag to asset catalog](images/37_drag_asset_catalog.png)

This will create two new entries in the asset catalog:

![Added assets](images/38_added_assets.png)

Notice that there are three "cells" for the images, and that they appear in the leftmost "1x" one. These cells represent the different screen scales - standard (1x), retina (2x) and retina-HD (3x). Normally you would provide assets for each of these cells, but in this simple project you will provide just one.

The image you've been provided is actually designed to be used at retina resolutions (2x), so drag the image from the left-hand cell to the central (2x) cell:

![Drag Retina](images/39_drag_retina.png)

Repeat for both of the 8-balls assets, so that the asset catalog looks like this:

![Finished Asset Catalog](images/40_finished_asset_catalog.png)

Those images are now available to use in your app—both from code and inside the storyboard. Time to put them to use.


### Displaying Images

You've already seen how to display text, buttons and text input fields in your app, but how about images? Enter `ImageView`.

Open __Main.storyboard__ and use the object library to search for __image view__:

![Image View](images/41_image_view.png)

Drag an image view onto the view controller canvas, at the bottom of the stack view. Notice how a horizontal blue line will appear denoting where in the stack view that the new image view will appear:

![Add to stack](images/42_add_to_stack.png)

Open the __Attributes Inspector__ and set the Image property to __8ball__:

![Set 8Ball](images/43_set_8ball.png)

This will update the canvas with the image, read from the asset catalog:

![VC with image](images/44_vc_with_image.png)

Build and run the app to see how the image appears:

![BAR 3](images/45_bar_03.png)

Well, the image has appeared, but you can't see it all. You can drag to resize the window in the usual way, but it would be much better if the window was correctly sized when the app starts, and then keeps a fixed size.


## Window Size

The initial size of MagicEight's window is determined by the size of the view controller in the storyboard. Select the view controller's view in __Main.storyboard__ and open the __Size Inspector__. Set the Width to __350__ and the height to __480__:

![VC Size](images/46_vc_size.png)

Notice that the storyboard canvas now looks strange:

![Strange VC](images/47_strange_vc.png)

This is because the layout constraints need to be re-evaluated now that the view controller's view has changed size.

Use the __Resolve Auto Layout Issues__ menu on the bottom auto layout menu bar and select __All View in View Controller \ Update Frames__:

![Fix AL](images/48_fix_al.png)

This will re-run the layout engine over the storyboard, and update the preview:

![Fixed VC](images/49_fixed_vc.png)

Now build and run to see how the app looks now:

![BAR 4](images/50_bar_04.png)

That looks much better. Try resizing the window—you'll see that you can still manually drag it to sizes and shapes that don't work for MagicEight's layout.

You have a couple of options here—you can either update the layout so that it adapts nicely as the window changes size (e.g. to resize the image for small windows), or you can fix the window size. This is a simple app, so you're going to take the second, easier approach.

### Window Sizing

The window controller is responsible for managing the size of the window. Open __Main.storyboard__ and select the window inside the window controller:

![Select Window](images/51_select_window.png)

Open the __Size Inspector__ and set the Content Size to a Width of __350__ and a height of __480__. Then check __Minimum Content Size__ and __Maximum Content Size__ ensuring that the the sizes are the same as the content size you entered:

![Content Size](images/52_content_size.png)

Now build and run MagicEight again, and attempt to resize the window. You'll notice that you no longer have control over the window size—it's now fixed. Mega—just what you wanted.

You can now turn your attention back to the task in-hand—building the "magic" of the magic 8-ball. Well, nearly—there's a bit more layout to do first.

## Handling clicks

When the user clicks on the 8-ball you will switch the image to the other side and display a small piece of advice. You need a new label to display this advice.

Open __Main.storyboard__ and find the view controller scene. Use the __Object Library__ to locate a __wrapping label__ and drag it from the library beneath the stack view:

![wrapping label](images/53_wrapping_label.png)

You want this label to be positioned at the center of the magic-8 image, so you need to add some auto layout constraints to ensure that happens.

__Control-drag__ from the __Multiline Label__ to the __Image View__ in the __Document Outline__:

![Adding constraints](images/54_adding_constraints.png)

Hold __shift_ and select __Center Vertically__ and __Center Horizontally__ before clicking __Add Constraints__:

![Center Constraints](images/55_center_constraints.png)

The label doesn't automatically reposition—but you've handled this before. Use the __Resolve Auto Layout Issues__ menu on the bottom auto layout toolbar to select __All View in View Controller \ Update Frames__:

![Fix AL](images/56_fix_al.png)

This repositions the label, and it is not immediately obvious that you need to do some work on the appearance.

Select the multiline label, and use the __Attributes Inspector__ to set the following:

- __Title__: __Piece of Advice__
- __Alignment__: __Center__
- __Text Color__: __Keyboard Focus Indicator__
- __Font__: __System 20__

![Label Config](images/57_label_config.png)

Head over to the __Size Inspector__ and set the Preferred Width to __Explicit__ with a value of __75__:

![Label size](images/58_label_size.png)

To get an idea of what the end product might look like, select the image view, and use the __Attributes Inspector__ to set the Image to __magic8ball__:

![Change Image](images/59_change_image.png)

Once again, use the __Resolve Auto Layout Issues__ to update the layout:

![Magic8 Layout](images/60_magic8_layout.png)

This is looking pretty good. You need to respond each time the user clicks on the 8-ball—time to discover gesture recognizers.


### Gesture Recognizers

When you added the button you were able to create an action in code that would run every time the user clicks the button. That was possible because buttons are designed to handle clicks—but the same is not true of image views. Apple has provided a set of gesture recognizers that can be added to any view. These recognizers convert low-level mouse and trackpad input into semantic events—such as click, zoom and rotate.

You will add a click gesture recognizer to the image view, and then create a corresponding action in the view controller's code.

In __Main.storyboard__, head over to the __Object Library__ and search for __click gesture__. Drag the __Click Gesture Recognizer__ out of the library and onto the image view. It will appear like this in the document outline:

![Gesture Recogniser](images/61_gesture_recogniser.png)

This gesture recognizer will be activated whenever the user clicks on the image—exactly what you want. You now need to create some new connections, and then you'll be ready to switch over to code for the final sprint.

### IB Connections

In the same way you did before, open up the assistant editor and ensure that it is showing __ViewController.swift__. Then create two new outlets by __control-dragging__ from the storyboard to the code: one for the image view (call it __ballImageView__) and one for the multiline label (call it __adviceLabel__). This adds the following new outlets:

```swift
@IBOutlet weak var ballImageView: NSImageView!
@IBOutlet weak var adviceLabel: NSTextField!
```

You also need an action to wire up the newly-created gesture recognizer. __Control-drag__ from the click gesture recognizer in the document outline, over to the code:

![Gesture Action](images/62_gesture_action.png)

Change the Connection to __Action__ and name it __handleBallClick__:

![Action config](images/63_action_config.png)

Click __Connect__ and Xcode will add the following function definition to the `ViewController` class:

```swift
@IBAction func handleBallClick(sender: AnyObject) {
}
```

You've now finished all the work in Interface Builder, so you can switch back to the standard editor, and open __ViewController.swift__ using the project outline.

## Manipulating the UI from code

When the user clicks on the 8-ball you want to switch between showing some advice, or showing the "8". This means that the `handleBallClick(_:)` will manipulate both the image view and the advice label.

Add the following code to `handleBallClick(_:)`:

```swift
// 1:
if(adviceLabel.hidden) {
  // 2:
  adviceLabel.hidden = false
  ballImageView.image = NSImage(named: "magic8ball")
} else {
  // 3:
  adviceLabel.hidden = true
  ballImageView.image = NSImage(named: "8ball")
}
```

1. Check whether the `adviceLabel` is currently visible. `hidden` is a boolean property on `NSView` (and hence `NSTextField`) that allows you to specify whether the view should be visible or not.
2. If the advice label is currently hidden then show it, and change the image to the magic-side. `NSImage(named:)` loads the image from the asset catalog, and the `image` property on `NSImageView` specifies the image to display.
3. Conversely, if the advice label is currently visible then hide it and switch the ball back to the "8" side.

Build and run and click around the ball to see it switching between showing the "8" and the piece of advice. Pretty neat right? Notice how when you first start the app the advice is already showing? That's not really what you want, but it's a simple fix.

## Initial Setup

When the app first starts you want to ensure that the advice label is hidden, and the 8-ball image is showing. View controllers have a perfect way of configuring this initial setup—in the form of `viewDidLoad()`.

Once the view controller has finished loading all the view components from the storyboard, it calls the `viewDidLoad()` method to give you a chance to provide some final pieces of configuration.

In __ViewController.swift__, find the `viewDidLoad()` method and add the following body:

```swift
adviceLabel.hidden = true
ballImageView.image = NSImage(named: "8ball")
```

You'll recognize this code from the click handler action—it just hides the advice label and sets the image to __8ball__.

Build and run to check that it worked as expected.


## Advice Generator

At the moment, no matter how many times you "shake" the ball, it always gives you the same advice. That's not especially helpful. Time to add a bit of randomness.

Add the following code as a property inside `ViewController`—just below the class definition line:

```swift
let adviceList = [
  "Yes",
  "No",
  "Ray says 'do it!'",
  "Maybe",
  "Try again later",
  "How can I know?",
  "Totally",
  "Never",
]
```

This is an array of strings the make up all the different options for advice that the ball can dispense.

Head to the very bottom of the file (not within the `ViewController` class) and add the following extension:

```swift
extension Array {
  var randomElement: Element? {
    if count < 1 { return .None }
    let randomIndex = arc4random_uniform(UInt32(count))
    return self[Int(randomIndex)]
  }
}
```

This adds a new property to the standard library's `Array` type that will return a random element. If the array is empty it returns `nil`, otherwise it generates a random index using `arc4random_uniform()` before returning the corresponding element.

Update the first branch (i.e. `adviceLabel.hidden == true`) of the `if` statement in `handleBallClick(_:)` to match the following:

```swift
if let advice = adviceList.randomElement {
  adviceLabel.stringValue = advice
  adviceLabel.hidden = false
  ballImageView.image = NSImage(named: "magic8ball")
}
```

This attempts to get a random piece of advice to display, and if successful updates the `stringValue` on `adviceLabel` to show it.

Build and run, and click the 8-ball a few times to start benefiting from the ball's wisdom:

![Final BAR](images/64_final_bar.png)


## Where to go from here

You can download the completed version of MagicEight [here](TODO: URL).

This introductory series has given you a basic level of knowledge to get started with OS X apps - but there's so much more to learn!

Apple has some great documentation covering all aspects of OS X development - head on over to https://developer.apple.com/library/mac/navigation/

You should also stick around on raywenderlich.com - we've got some awesome OS X tutorials lined up for the coming months!

If you have any comments or questions feel free to reach out on twitter, or in the comments section below!

