# Introduction to the 2D animation features

## Overview

Wether it's a animating a logo, a character or a user interface, animation
is a key part of each developed game.

Godot has a powerful animation engine, that allows the developer to create
anything from a simple to a complex animation.

In this guide you will learn the basics of a very key part of Godot's 2D
animation system: the AnimationPlayer. You will get to know it's user
interface, create a simple animation with it, and also learn, how to call
functions in an animation.

## Animation components

Animations in Godot basically consist of the timeline, tracks and (key-)frames.

The timeline is the concerted animation based on tracks and frames.

A track is a reference to a property, that is modified during the animation.
An example would be the position value of the transform property of a sprite,
that is modified to match a path between two points A and B.

Keyframes are key points in the timeline, that specify a goal value for a
property of a track. The frames between key frames are not directly editable
and are generated by the animation engine, usually interpolating the property
values between two keyframes.

## The whereabouts

When doing animation in Godot you're dealing with three parts:

* One or more AnimationPlayer nodes (although one is usually enough)
* The animation tab
* Some nodes to animate

### AnimationPlayer nodes

The AnimationPlayer node type is the data container for your animations. One
AnimationPlayer node can hold multiple animations, that can automatically
transition to one another.

![The AnimationPlayer node](images/animationPlayerTree.png)

### Animation tab

When selecting an AnimationPlayer node, the animation tab (found on the
  lower panel per default) will open up. It mainly consits of three parts:

![The animation tab](images/animationTab.png)

* Animation controls (i.e. add, load, save, and delete animations)
* The tracks listing
* The timeline with keyframes, displayed as dots
* The track modifiers and keyframe editor (when enabled)
* The timeline and track controls, where you can zoom the timeline and edit
  tracks for example.

See the Animation tab reference below for details.

## Creating a simple animation

### Scene setup

For this simple tutorial, we'll going to create an AnimationPlayer node and
a sprite node as a children of the AnimationPlayer.

![Our scene setup](images/animationPlayerTree.png)

The sprite will hold a simple image texture and we will animate that sprite
to move between two points on the screen. As a starting point, move the
sprite to a left position on the screen.

---

*Note*

Adding animated nodes as child nodes to the AnimationPlayer is not required.
However, it is a nice way of distinguishing animated parts from non-animated
parts in the Scene Tree.

---

Select the AnimationPlayer node and click on "Add animation"
(![Add Animation](images/addAnimation.png)) in the animation
tab to add a new animation.

Enter a name for the animation in the dialog box.

### Adding a track

To add a new track for our sprite, select it and take a look in the toolbar:

![Convenience buttons](images/convenienceButtons.png)

These switches and buttons allow you to add keyframes for the location, rotation,
and scale of the selected node respectively.

Deselect rotation, because we are only interested in the location of our sprite
for this tutorial, and click on the key button.

As we don't have a track already set up for the transform/location property,
the animation tab will ask, wether it should set it up for us. Click on
"Create".

This will create a new track and our very first keyframe at the beginning of
the timeline:

![The sprite track](images/animationTrack.png)

As you can see, the track list doesn't hold a descriptive name for a track,
instead the list holds references.

The track is composed of a path to a node, followed by a colon,
followed by a reference to its property we would like to modify.

In our example, the path is `AnimationPlayer/Sprite` and the property is
`transform/pos`.

The path always starts at the parent node of the AnimationPlayer node (so
  paths always have to include the AnimationPlayer node itself).

---

*Note*

Don't worry, if you change the names of nodes you already have tracks for.
Godot will automatically update the paths in the tracks.

---

### The second keyframe

Now we need to set the destination, where our sprite should be headed and how
much time it will take to get there.

Let's say, we want it to take 2 seconds to go to the other point. Per default
the animation is set to last only 1 second, so change this in the timeline
controls on the bottom of the animation tab to 2.

Click on the header of the timeline near the 2 second mark and move the sprite
to the target destination on the right side of the screen.

Again, click the key button in the toolbar. This will create our second
keyframe - represented by a blue dot in the timeline.

### Running the animation

Click on the header of the timeline again, this time on the 0 second mark,
then click on the "Play from beginning"
(![Play from beginning](images/playFromBeginning.png)) button of the animation
controls.

Yay! Our animation runs:

![The animation](images/animation.gif)

### Back and forth

As you can see, the "loop" button is enabled by default and our animation
simply loops. Godot has an additionally nice feature here. Godot always
calculates the frames between to keyframes. In a loop, the first keyframe
is also the last keyframe, if no keyframe is specified at the end.

If you set the length of the animation to 4 seconds now, the animation will
be moving back and forth.

### Track settings

Each track has a settings panel at the end, where you can set the update rate
and the track interpolation.

The update rate of a track tells Godot when to update the values of the
properties. This can be:

* Continuous: Update the property on each frame
* Discrete: Only update the property on keyframes
* Trigger: Only update the property on keyframes or triggers

In normal animations, you will usually just use "Continuous", but be aware,
that you can have other forms of updating as well and the features for
coding complex animations here.

The interpolation tells Godot how to calculate the frame values between the
keyframes. These interpolation modes are supported:

* Nearest: Set the value of the nearest keyframe
* Linear: Set the value based on a linear function calculation between the two
  keyframes
* Cubic: Set the value based on a curved function calculation between the two
  keyframes

Cubic interpolation will lead to a more natural movement, where the animation
is slower at a keyframe and faster between keyframes. This is usually used
for character animation. Linear interpolation will create more of a robotic
movement.

## Keyframes for other properties

Godot doesn't restrict to just editing transform properties. Basically every
property can be used as a track and can set keyframes.

If you select your sprite while the animation tab is visible, you get a small
keyframe button for all properties of the sprite. Click on one button and
Godot will automatically add a track and keyframe to the current animation.

## Keyframe editing

For advanced use and detailed keyframe editing, the keyframe editor
(![Keyframe editor](images/keyframeEditor.png)) can be enabled.

This will add an editor pane on the right side of the track settings. When
you select a keyframe, you can directly edit its values in this editor:

![Keyframe editor editing a key](images/keyframeEditorKey.png)

Additionally, you can also edit the transition value for this keyframe:

![Keyframe editor editing a transition](images/keyframeEditorTransition.png)

This will tell Godot, how to change the values of the property when reaching
this keyframe.

You usually tweek your animations this way, when the movement doesn't
"look right".

## Advanced: Call Func tracks

Godot's animation engine doesn't stop here. If you're already comfortable with
Godot's scripting language GDScript and API, you know, that each node type
is a class and has a bunch of functions, that can be called.

For example, the [`SamplePlayer2D`](http://docs.godotengine.org/en/stable/classes/class_sampleplayer2d.html)
has a function to play a sample.

As we create animations, playing a sample at a specific keyframe would be great,
too. This is where "Call Func Tracks" come in handy. These tracks reference a
node again, this time without a reference to a property.

To let Godot play a sample when reaching a keyframe, follow this list:

* Add a SamplePlayer2D to the Scene Tree and add a sample library and a sample
to it.
* Click on "Add track" (![Add track](images/addTrack.png))
on the track controls of the animation tab.
* Select "Add Call Func Track" from the list of possible track types.
* Select the SamplePlayer2D node in the selection window. Godot will add the
  track with the reference to the node.
* Select the frame, where the sample should be played by using the timeline
  header
* Click on "Add keyframe" near the settings of our func track
  (![Add keyframe](images/addKeyframe.png)).
* Select the keyframe
* Enable the Keyframe Editor
* Enter "play" as the name of the function and set the argument counter to 1.
* Select "String" as the first argument type and use the Sample name as the
argument value.

Now, when reaching the keyframe, Godot will call the SamplePlayer2D node's
"play" function with the name of the sample.

## References

### Animation tab reference

![The animation tab refrence](images/animationTabReference.png)

The animation tab has the following parts:

* Animation controls
   * Play animation backwards from current position
   * Play animation backwards from the end of the animation
   * Stop animation
   * Play animation forwards from the beginning of the animation
   * Play animation forwards from the current position
   * Direct time selection
* Animation management:
   * Create a new animation
   * Load animation
   * Save animation
   * Duplicate animation
   * Rename animation
   * Delete animation
   * Animation selection
   * Automatically play selected animation
   * Edit animation blend times
   * Extended animation Tools
* Timeline zoom level control
* Timeline control
   * Length of animation
   * Steps of animation
   * Toggle loop animation
* Track controls
   * Add track
   * Move track up
   * Move track down
   * Delete track
   * Extended track tools
   * Toggle keyframe editor