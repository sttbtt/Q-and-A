# Q&A

A student that completes this project shows that they can:

- understand and explain what Auto Layout is, and the problems it solves
- implement common layouts using constraints in Interface Builder
- implement common layouts using UIStackView

## Introduction

Q&A allows a user to ask a question, and also answer them. This project is meant to help you solidify Auto Layout skills.

Please watch the screen recording in order to know what the finished application should look like.

## Screen Recording

![](https://user-images.githubusercontent.com/16965587/43413591-b8bd7ff8-93ed-11e8-8830-7e7c2f0e5d75.gif)

## Instructions

Please fork and clone this repository. Create a new Xcode project inside of the cloned repository. Note that these instructions will be intentionally vague in parts of the project you should have decent experience with.

### Part 1 - Question and QuestionController

First will be setting up the single model object and its controller for this project.

#### Question

The model object should be a struct called `Question`. Create a new Swift file for it and give it the following properties:

- A `question` string that represents the actual question the asker has 
- An `asker` string that will store the name of the person asking the question
- An optional `answer` string that represents the answer to the question. This should be optional as there will not be an answer for a question as soon as it is created.
- An optional `answerer` string that is the name of the person answering the question

Create an initializer for this struct that gives default values of `nil` to the `answer` and `answerer` parameters.

Be conscious of which properties should be constants or variables.

#### QuestionController

Create a new Swift file called "QuestionController.swift" and in it, make a class called `QuestionController`. 

Add the following to the `QuestionController`:

- An array of `Question` objects called `questions` that will be used as the data source for the application.
- A "Create" function that will initialize a `Question` object and add it to the `questions` array.
- An "Update" function that takes in a `Question` (that you want to update), `answer` string, and an `answerer` string to add to the question.
- A "Delete" function that takes in a `Question` to be deleted, and removes it from the `questions` array.

### Part 2 - Storyboards and Constraints

This application has three view controllers:

- One will show a list of `Question`s that have been asked
- One will allow a user to ask a question
- One will allow a user to answer a question.

**All of these view controllers should be laid out and constrained yourself.** Do not use of the options in the "Resolve Auto Layout Issues" button (the triangle TIE Fighter) such as "Add Missing Constraints" or "Reset to Suggested Constraints".

#### QuestionsTableViewController Scene

The first view controller the user will see in the application is the list of `Question`s.

1. Drag out a `UITableViewController` scene.
2. Embed it in a navigation controller, and set the navigation controller as the initial view controller.
3. Give the table view contoller the title "Q&A".
4. Add a bar button item, and give it the title "Ask Question"

The table view's cell is custom. It should look like this:

![](https://user-images.githubusercontent.com/16965587/43414311-a7b2f5c4-93ef-11e8-9ca5-72a9ec97aaaf.png)

Using stack views, make your cell look similar to the screenshot above.

Hints:

1. The cell has five labels, not three.
2. The text style of the labels that say "Question", "Asked By", and the one on the last line should be set to medium.
3. Put the labels that are next to each other horizontally in their own stack view. Then you may put those stack views in a "super" stack view that holds those two stack views and the label on the bottom
4. Using the pin menu, constrain the "super" stack view to the cell at about 8 points in each direction.

Create Cocoa Touch Subclasses for both the table view controller scene, and the custom cell. Be sure to set their class in the Identity Inspector.

Add outlets to the custom cell.

#### AskQuestionViewController Scene

In this view controller, you are not allowed to use stack views. This view controller will allow the user to ask a question. It will be shown when the user taps on the "Ask Question" bar button item in the table view controller.

1. Add a `UIViewController` scene to the storyboard.
2. Add a text field to the scene that will allow the user to enter their name. Constrain it to be 16 points from the top of the safe area, and 0 points away from the leading and trailing margins.
3. Add a text view that is 8 points under the bottom of the text field, and 0 points away from the leading and trailing margins. Also, constrain it to be one third of the height of the view controller's view.
4. Add a navigation item, then a bar button item. The bar button item's title should be "Submit Question".
5. Create a Cocoa Touch Subclass for this view controller. Set the view controller scene's class in the Identity Inspector.
6. Add outlets from the text field and text view, and an action from the "Submit Question" bar button item. Leave the action blank for now.
7. Back on the table view controller scene, create a show segue from the "Ask Question" bar button item to the `AskQuestionViewController` scene that you just finished setting up. Give the segue an identifier.

#### AnswerViewController Scene

In this view controller, you must use a stack view to help you lay out the scene. This view controller will be shown when tapping on a question cell to allow the user to answer a question, and view the answer to a previously answered question.

1. Add a `UIViewController` scene to the storyboard.
2. Add two labels to the scene. One will display the question to be answered, while the other will show the name of the person who asked the question.
3. Add a text field allowing the user to enter the name of the person answering the question
4. Add a text view allowing the user to answer the question.
5. Add all four UI elements to a stack view. Constrain the stack view to the top of the view controller. Make set its height to be half of the view controller's view's height.
6. Add a navigation item, then a bar button item. The bar button item's title should be "Submit Answer".
7. Back on the table view controller scene, create a show segue from the custom cell to the `AnswerViewController` scene that you just finished setting up. Give the segue an identifier.

### View and View Controller implementation

#### QuestionTableViewCell

1. If you haven't already, make outlets from the labels in the storyboard to the custom cell class.
2. Add a variable `question` whose type is an optional `Question`.
3. Create a private method `updateViews()`, that takes the values in the `question` variable and sets the text of the labels accordingly. Make sure you unwrap the `question`.
4. Add a `didSet` property observer to the `question` variable, and call `updateViews()` inside of it.

#### QuestionsTableViewController


1. Clean up this file. You will only need the required `UITableViewDataSource` methods, the `commit editingStyle` and `prepareForSegue` methods.
2. Add a constant called `questionController` and set its value to a new instance of `QuestionController`.
3. Using this `questionController`, implement the `numberOfRowInSection` function.
4. Implement the `cellForRowAt` method. Remember that you are using a custom cell. This function should pass an instance of `Question` to the cell.
5. Implement the `commit editingStyle` method to allow for deleting question cells. (remember to delete a `Question` before you delete the cell)
6. Add the `viewWillAppear` method. Reload the table view in it so that when a new question or answer gets added, the table view will reflect these changes.


#### AskQuestionViewController

1. Add a `questionController: QuestionController?` variable.
2. In the action of the bar button item:
    - Get the text from each text field, and make sure they aren't `nil` or an empty string (`""`). 
    - Call the model controller's `createQuestion` method
    - Have the navigation controller pop to the previous view controller.

#### AnswerViewController

1. Add a `questionController: QuestionController?`, and a `question: Question?` variable.
2. In the action of the bar button item:
    - Get the text from the text field and text view, and make sure they aren't `nil` or an empty string (`""`). 
    - Call the model controller's "Update" method to add an answer and answerer to the question that you pass in.
    - Have the navigation controller pop to the previous view controller.
3. Create an `updateViews()` method. As this view controller is only accessible upon tapping a question cell, grab the values from the `question` variable and place them in the corresponding labels and text field or text view. Be aware that tapping a cell that doesn't have an answer and answerer should still display the question and the name of the person who asked it. If it does have an answer and answerer, those values should also be placed in the text field and text view accordinly.

#### PrepareForSegue

Finally, go back to the `QuestionsTableViewController` class. Now that the destination view controllers that this view controller segues to are implemented, fill out the `prepareForSegue` method. 

1. If the segue is going to the `AskQuestionViewController`, you need to pass the `questionController`.
2. If the segue is going to the `AnswerQuestionViewController`, you need to pass the `questionController`, and also the `question` that the user tapped on and wishes to view and/or add an answer to.

### Go Further

- Re-constrain the custom cell without using stack views at all.
- Modify the custom cell so that it will resize automatically to show the answer (if there is one) in its entirety. 
