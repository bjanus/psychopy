Builder concepts
--------------------

Routines and Flow
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Builder view of the PsychoPy application is designed to allow the rapid development of a wide range of experiments for experimental psychology and cognitive neuroscience experiments.

The Builder view comprises two main panels for viewing the experiment's :doc:`Routines </builder/routines>` (upper left) and another for viewing the :doc:`Flow </builder/flow>` (lower part of the window).

An experiment can have any number of `Routines`, describing the timing of stimuli, instructions and responses. These are portrayed in a simple track-based view, similar to that of video-editing software, which allows stimuli to come on go off repeatedly and to overlap with each other.

The way in which these `Routines` are combined and/or repeated is controlled by the :doc:`Flow </builder/flow>` panel. All experiments have exactly one :doc:`Flow </builder/flow>`. This takes the form of a standard flowchart allowing a sequence of routines to occur one after another, and for loops to be inserted around one or more of the `Routines`. The loop also controls variables that change between repetitions, such as stimulus attributes.

Example 1 - a reaction time experiment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
For a simple reaction time experiment there might be 3 :doc:`Routines </builder/routines>`, one that presents instructions and waits for a keypress, one that controls the trial timing, and one that thanks the participant at the end. These could then be combined in the :doc:`Flow </builder/flow>` so that the instructions come first, followed by `trial`, followed by the `thanks` :doc:`Routines </builder/routines>`, and a loop could be inserted so that the `Routine` repeated 4 times for each of 6 stimulus intensities.

Example 2 - an fMRI block design
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Many fMRI experiments present a sequence of stimuli in a `block`. For this there are multiple ways to create the experiment:
* We could create a single :doc:`Routine </builder/routines>` that contained a number of stimuli and presented them sequentially, followed by a long blank period to give the inter-epoch interval, and surround this single `Routine` by a loop to control the blocks.
* Alternatively we could create a pair of `Routines` to allow presentation of a) a single stimulus (for 1 sec) and b) a blank screen, for the prolonged period. With these `Routines` we could insert  pair of loops, one to repeat the stimulus `Routine` with different images, followed by the blank `Routine`, and another to surround this whole set and control the blocks.

Demos
~~~~~~~~
There are a couple of demos included with the package, that you can find in their own special menu. When you load these the first thing to do is make sure the experiment settings specify the same resolution as your monitor, otherwise the screen can appear off-centred and strangely scaled.

Stroop demo
==============
This runs a digital demonstration of the Stroop effect [1]_. The experiment presents a series of coloured words written in coloured 'inks'. Subjects have to report the colour of the letters for each word, but find it harder to do so when the letters are spelling out a different (incongruous) colour. Reaction times for the congruent trials (where letter colour matches the written word) are faster than for the incongruent trials.

From this demo you should note:
 * How to setup a trial list in a .csv file
 * How to record key presses and reaction times (using the `resp` Component in `trial` :doc:`Routine </builder/routines>`)
 * How to change a stimulus parameter on each repetition of the loop. The text and rgb values of the `word` Component are based on `thisTrial`, which represents a single iteration of the `trials` loop. They have been set to change every repeat (don't forget that step!)
 * How to present instructions: just have a long-lasting `TextStim` and then force end of the `Routine` when a key is pressed (but don't bother storing the key press).

.. [1] Stroop, J.R. (1935). "Studies of interference in serial verbal reactions". Journal of Experimental Psychology 18: 643-662.

Grating demo
===============
As at version 1.50.04 (I plan to update it to demo a mini psychophysics experiment), this is a very simple demo showing how to drift a Gabor stimulus in realtime. It shows a few things of note that differ to the Stroop demo:
 * The stimulus orientation is governed by `expInfo['ori']`, which is a python dictionary created in the `Experiement Settings` dialog
 * The phase of the stimulus is set to change every frame and its value is determined by the value of `trialClock.getTime()*2`. Every :doc:`Routine </builder/routines>` has a clock associated with it that gets reset at the beginning of the iteration through the `Routine`. There is also a `globalClock` that can be used in the same way. The phase of a `Patch` `Component` ranges 0-1 (and wraps to that range if beyond it). The result in this case is that the grating drifts at a rate of 2Hz.