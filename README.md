<b> Note: This repository has been made private in compliance with the Object Oriented Design class rules. For access to the code, please send an email to josyula.r@northeastern.edu. </b>

README
Created by Rida Jawed and Rohit Josyula.

Design Thinking
Our design for Assignment 4-6 consisted of a Model, View, Controller Design, and has a Command
Design Pattern as explained below.

Model:
Our Model is called the ImageProcessorModel and it represents an inventory of all the images that
have been loaded into the model or created by the user. It utilizes a Map class to store the images
and label them. The objects in this Map are Image.class objects. The model also had methods called
addImage that allows the user to add an Image object, getImage that allows the user to retrieve a
particularImage object.

The Image class contains the height, width, and maxValue of the Image, as well as a 2D Array of all
the Pixels that the Image is comprised of. The Image class has getter methods that get the width,
height, maxValue, pixels list, pixel at a given spot, and makes sure that they are deep copies. In
addition we have a toString() and copy() methods that represents the Image as a String, and makes
a deep copy of it respectively.

The pixels are an object of the Pixel.class and they store the position(x,y) of the Pixel as well as
the RGB composition of that particular Pixel. The Pixel class also has getters that provide the x
and y coordinates, and the RGB values that are all ints. Additionally, we have the toString() and
copy methods that represent the Image as a String, and makes a deep copy of it respectively.

Controller:
The controller class is a method that runs the the MVC project. It contains the model and the view
as fields. Additionally, we have a field called knownCommands() that consists of all the commands
that are possible for the user to use and transform an image. The start() method in Controller
begins the program and runs until there is no more inputs to read. The method gets the next input
from the user and creates the correct Command Object to use to transform the image, also giving
it the right inputs (See Command Design Pattern below). It gets the correct image from the model,
gives it to the command, the command provides a new transformed image, and the controller adds it to
the model.

Command Design Pattern:
For the knownCommands field in the, we have a command design pattern that consists of all the
command classes to use. The interface ImageCommandTransforms contains a method called execute that
executes the command on the given inputs which is the old image name and the new image name. This
pattern consists of brighten, greyscale by component, flip horizontal, flip vertical, load, and
save commands. However, in the future, if anyone wants to be able to support a new command, they can
just create a new class that extends the abstract command class that implements the
ImageCommandTransform interface and implement the execute method and its respective helper method
for that particular transformation.

View:
The view is fairly simple for this project because it just ensures proper communication with the
user in the console output. It contains a field called the appendable which is an Appendable.class
object and is used to output messages to. It also has a public method called renderMessage() that
allows us to output a message to the appendable which by default is System.Out. This view is used in
the Controller to output error messages when the user puts in incorrect commands, provides wrong or
not enough arguments, or provides faulty files.

New Design Changes (11/10/22):
Additional support for the sepia, greyscale, blur, and sharpen commands were implemented for this
program. Greyscale and sepia were very similar in terms of functionality since they are both color
transformations, so an abstract class for color transformations was created that extends the general
abstract command class and accounts for the similar logic between the greyscale and sepia commands.
Likewise, the sharpen and blur commands were very similar, so an abstract class for filter commands
was created and accounts for the similar logic between these two classes. This means in the future,
anyone who wants to implement a new filter or color transformation command can simply extend the
corresponding abstract class and provide the appropriate matrix values to perform the operation.

In terms of changes to the design, one of the main fixes was the problem of leaking concrete classes
to users instead of using their respective interfaces. The appropriate adjustments were made
throughout the code to resolve it. Another fix made was to throw the appropriate exception if null
was passed to one of the arguments in the constructor. The javadoc was updated accordingly as well.
The program is also able to support a script containing a list of commands to run, and this change
was made by adjusting the Main class accordingly. The last design change made was to use function
objects when it comes to loading and saving different file types. Initially, the load and save
methods in the ImageUtils class were designed such that it would only load and save PPM files.
However, the program can support different file types for images, including PPM, JPG, JPEG, BMP, and
PNG. The full details of this are discussed in the next paragraph.

The load and save methods in ImageUtils were very long, so the functionality was delegated to
function objects based on the type of operation done, and whether the file is a PPM. The reason for
this is because the logic for loading and saving a PPM file was different than that for the other
file formats, as they utilized a BufferedImage. In regards to the save method, two new classes were
created to save the Visual object to the respective format, and the implementation for the convert
method they had to do varied. In regards to the load method, the file being loaded into the model
was parsed very similarly irrespective of its type, so an abstract class was created to account for
this. The appropriate implementations for dealing with a PPM vs a BufferedImage was done in their
respective classes. 


New Design Changes (11/22/22):
Additional support for applying the transformations to the images via a GUI were implemented for
this program. To do this, the Swing framework was utilized to create this view, with all the
front-end logic based off it. A Panels interface was created so that any implementations would have
to accept a Visual to process and display, and another method to render any error messages to the
screen. A Histogram class was created so that all the different RGB component histograms of the
loaded image can be displayed on the panel. Finally, as part of the back-end logic, a new controller
implementing the IFeatures interface was made so that it can interact with the view such that when a
button is pressed in the view, the controller is notified by it of what to do. One additional method
was added to the ImageUtils class, which helped with converting from a Visual object to a
BufferedImage. The purpose of this was to help in rendering the Visual to the screen.

GUIView:
    PhotoEditorView:
    We created an extension of JFrame which was called the PhotoEditorView and was our main frame
    for the GUI interface. In this frame, we decided to have all the transformation commands as
    buttons as well as an internal panel of the class Editor Panel described below.
    Editor Panel:
    Editor Panel is class that represents the main chunk of the GUI interface and it extends a
    JPanel It contains the potential error messages that may show up throughout the execution of the
    program, the image being worked on, and the red, green, blue, and intensity, component
    histograms of the image.

Using this GUIView implementation, the user can graphically interact with our photo editor and load
and save files from/to a specific directory.
Features:
    We implemented a new interface called IFeatures which represents all the features supported by
    the GUIView. This interface has methods for all the commands possible (flips, component
    visualization, sepia, etc) which take in the name of the image being worked on and executes the
    transformation. These methods add the new edited image to the model and also send it to the view
    to show the new updated image. So far, there is one implementation of IFeatures, called
    FeaturesImpl that has fields for a ImageProcessorModel and a GUIView.

Histograms:
    To display the color component histograms of the images, we decided to make a Histogram class
    that implements an interface called Graphs. Objects of the Histogram class store the values of
    the different bars in a bar chart and allow the user to increment bars by 1 and also get the
    maximum value.

    The Editor Panel has 4 fields of type Graphs which are Histograms that represent the color
    distribution of the image. We used these fields in the Editor panel to update the color
    distribution every time a transformation is made and a new image is being displayed in place of
    the last image. We override the paintComponent method of Editor Panel and drew out these
    Histograms using rectangles.

Invariants:
Pixel:
    1. Position coordinates are a positive integer
    2. RGB values are between 0 and 255
Image:
    1. The width and height are positive integers
    2. The max value is the maximum of the RGB values amongst all the pixels
Controller:
    1. The model is a non-null object
    2. The view is a non-null object
Commands:
    1. The model is a non-null object
View:
    1. The appendable is a non-null object

Transformation Samples:
We used an image from https://illustoon.com and compressed to apply all transformations on it, and
the transformed outputs can be found in the res directory.



