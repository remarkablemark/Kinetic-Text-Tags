# Kinetic-Text-Tags
 Text Tags that add various effects to Ren'py text!

Ever felt the text in your visual novel is looking a bit too static?
Ever want to spice it up with a little flair?
Or feeling a line needs a little more OOMPH!! to get the job done?


![Example Wave](https://github.com/SoDaRa/Kinetic-Text-Tags/blob/main/game/example_gifs/ExampleWaves.gif)
![Example Scared](https://github.com/SoDaRa/Kinetic-Text-Tags/blob/main/game/example_gifs/ExampleScared.gif)
![Example Chaos](https://github.com/SoDaRa/Kinetic-Text-Tags/blob/main/game/example_gifs/ExampleChaos.gif)
![Example Swap](https://github.com/SoDaRa/Kinetic-Text-Tags/blob/main/game/example_gifs/ExampleSwap.gif)
![Example Move](https://github.com/SoDaRa/Kinetic-Text-Tags/blob/main/game/example_gifs/ExampleMove.gif)

Then look no further! Through the power of Custom Text Tags, you can add any translation, rotation, projection, or styling you want on the fly!!
You can even change the text and stack multiple effects together.
You can even have your text react to the mouse a little if you'd like!
Just add some Kinetic Text Tags to your Ren'py project and you'll be flinging text around the screen like a wizard in no time!

To use what I've made or to learn how to create your own, simply include **[kinetic_text_tags.rpy](https://github.com/SoDaRa/Kinetic-Text-Tags/blob/main/game/kinetic_text_tags.rpy)** in your project and read over the code and comments for how you can get started and to learn how to create your own.
Due to some limited functionality with button styling, I have included a patch to renpy that should help enable that functionality. However it is experimental. Details on it will be included below.

## How it works
    This modification works by taking the letters within a string of text sent to the tag, and replacing them with custom displayables that is then inserted back into the Text with that string. Our displayable then handles the rendering and placement of the text and can move it around freely and even change its content and styling!
    
## Base Functionality Notes
- You can nest your text tags within each other as well. Allowing you to combine effects together. Though if you are nesting several at once, it is recommended you create what I am calling an Omega tag that will nest them together more directly and save on memory. It may also be more efficent for you to have one class that applies the effects of multiple classes in itself. I leave the decision up to you. Just do be aware of memory concerns.
- The online documentation never mentions it, but renpy does support matrix math being applied to renders. An example of the RotateText class using this has been included in a block comment, along with notes of some other matrix functions within renpy if you'd like to play around with them.
- Many more effects may be possible than the ones included. You could easily include ones such as text that zooms when first displayed, or text that seems to blow away in the wind after a certain amount of time. I implore users to experiment with this and see what effects they can come up with!
    
## Base Limitations
- Because our parent text doesn't consider us a text element, it will not automatically apply its text styling to our custom displayable. So any style choices will need to be supplied within our text tag. That way we can apply those changes to the child text (our single letter) by including the style rules within its string.
- Following from the previous, currently, I only have implemented support for certain tags. The tags handled in the DispTextStyle class are {b}, {u}, {s}, {i}, {font}, {color}, {size}, {=custom_style}, {plain}, {outlinecolor} and {alpha} (though alpha is unused in renpy) as well as some of my custom tags. The {p}, {w}, {nw} and {fast} should be ignored by DispTextStyle entirely and allowed to pass through. As should the {clear}, {#} and {k}. The {space}, {vspace}, {image}, {a}, {horiz}, {vert}, and ruby tags ({art}, {rt}, {rb}, etc) are all likely to be workable, but how they should be implemented is likely to differ depending on how you want to use them.
- Because of the first limitation, by default, if your text tag is applied to a textbutton, it will not update properly when hovered or selected. A section below will cover a workaround I have for this.
- The history screen will not by default apply any text tags. This can be used to have what is shown on the say screen be different on the history screen if you'd like. However, if you'd like to have some of your effects carry through, then you can either append the gui.history_allow_tags variable or change the line '''$ what = renpy.filter_text_tags(h.what, allow=[gui.history_allow_tags, "your_tag_here"])'''  in the history screen section of screens.rpy
- It should be noted that because we are using the Text class within Renpy, it may be a bit bloated. You could likely save a lot on resources by defining a Letter class that has much of the same functionality as the Text class, but only has to worry about a single letter. If you are finding yourself having performance concerns, I would advise doing this.  I may also provide one here in the future for those interested. I'll update on here if/when it has been added.

# Advanced (Style integration)
    In order to allow people to use these text tags to work with buttons, I have found a workaround to the style inheritance problem. In order to implement it, go to where you have your Renpy version installed on your computer (this should be a folder called renpy-(version number)-sdk). Once there go  renpy-(version number)-sdk>renpy>text and replace the text.py with the **[text.py](https://github.com/SoDaRa/Kinetic-Text-Tags/blob/main/text.py)** included. This will tell the Text class to send its style to our wrapper class as well as any prefix additions. Just be sure to implement 'set_style()' and 'set_style_prefix()' to every class you'd want this to apply to. Examples you can copy are available a the bottom of the BounceText class in the form of a block comment, just so people not using it do not need to worry about it.
    This will allow for any style prefixes like hover_, insensitive_ and idle_ to pass through our parent Text so we can apply it to our Wrapper's child. This also means you won't have to declare a style for your text at the beginning of every tag!
    However, I cannot say that I have rigorously tested this change. While I have added checks that should prevent this from being applied to anything but our classes, I cannot say I have tested every scenario you might encounter. So **use this at your own caution**. If you'd like to remove these changes, you can find my additions to the Text class bookended by long sequences of hashtags within the Text class. Specifically it modifies the update() function at line 1706-1713 (near the end of the function) and set_style_prefix() at line 1801-1813. Deleting these lines should return Renpy back to normal. **This addition is also only for renpy 7.3.5.** When renpy 7.4 is released, I will try to provide an update.
    And if anyone finds a better work around than this, feel free to let me know and I'll be happy to add them in here. I only went for this method since it seemed the easiest to implement. Other ideas I considered were having a Wrapper class that attempt to guess when the mouse was over the button based on getting the bounds of a Text supplied with the given string. Or have one that was supplied with the bounds of the button so it could check for the mouse in that region. However, both of these seemed rather cumbersome and more difficult to manage that most people would want. I will include some notes on the 

For a more detailed account of how this was made or (other details I may have forgotten to include), you can read more about it in my post on the Lemma Soft Forums https://lemmasoft.renai.us/forums/viewtopic.php?f=51&t=60527
