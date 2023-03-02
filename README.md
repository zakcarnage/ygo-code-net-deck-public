# ygo-code-net-deck
Code for the YGO Code Net Deck project, used to store replays and assemble competitive decklists
Watch the introductory video here:
https://youtu.be/abKKT3v34AE

# WARNING - USE AT YOUR OWN PERIL
This project features code which is 
A) Not optimised;
B) Only tested on one device.
While the majority of the libraries used are fairly generic, I recommend only using the relevant notebook(s) and script(s) if you are 
deeply familiar with using Python on your own device.
In particular, the code makes lazy use of parallel running processes which can be brutal if your system is not prepared for it.
As such, I cannot recommend you use this code. But, if you want to ignore that and try it out, be my guest.

# Introduction
This project uses free to access match replays from duelingbook to create a training set from starting hands.
After some filtering according to the user's design, the data is used to train a simple model with the winning flag set as the target.
The project uses a generic genetic algorithm approach to deck building.
This practice is widely noted as common for card games with deck building aspects.
However, I have no references to note as I was not aware when putting this together.

This project was intended as a way to harness useful, publicly available information from the www.duelingbook.com platform.
Without the work of those running duelingbook, many players would not have access to free to play competitive Yu-Gi-Oh! TCG matches.
While I do not personally endorse the use of duelingbook given the availability of the real life TCG, and Master Duel for those esports inclined,
I believe that the duelingbook team put in an enormous amount of work to help community members engage in free to play competitive TCG matches.
If you are interested in their work, please do visit the website.

### Stages
This project is broken into the following stages:

1. API capture 
2. Card selection
3. Replay scraping
4. Model Training
5. Deck optimising

# Set Up
First you will need to clone this repo to your machine and establish the following folder structure:
your-folder
|
|___ygo-code-net-deck
|    |
|    |___Complete Robot
|    |___Construct Base Deck
|    |    |_YGO Card Database
|    |___Model_Folder
|
|
|__project_data_folder
    |___Replays
    |    |_log.txt <- this file must contain a number, I recommend starting from 46000000
    |___Compressed_Replays
    |___Hands


# API Capture
(Previously database scraping)
This stage uses requests to save a copy of the ygoprodeck data locally.
It is a great service that these people offer, and I highly recommend that you visit their website before runnin any code:
https://ygoprodeck.com/api-guide/

# Card Selection
This stage opens up an interactive window using tkinter.
Here you can input the names of cards, search through the database, then add them to your deck and save it with a custom name.
Sometimes you'll want to include engine requirements in your deck. If you specify these, you can ensure that at least one copy will feature in your deck.
Search for a card name.
Click the card name you want, and make sure it changes in the heading.
Click add to deck.
To remove a card from the deck, double-click its name.
To make a card in your deck an engine requirement, click on the name, then click the engine requirement button.
Type in a name for your deck and use the save button to save the cards into a csv which will be a starting point for the model.

# Replay Scraping
THIS DOES NOT WORK ANYMORE
I am looking into ways to deal with it, but right now you can't scrape db replays the way you used to be able to.
If you have any suggestions (which aren't webdriving as I'm already onto that one) then let me know.

# Model Training
With the captured data, you can now train a model!
As a completely rough and ready prototype, this stage uses a random forest classifier from sklearn to predict game outcomes based on starting hands.
You can read more about random forest classifiers here:
https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html

The approach is deeply simplistic, but results were very promising, so I've kept it straightforward for ease of explanation.

# Deck Optimising
This stage generates random decks based on the originally selected cards.
The minimum is set as 1 for engine requirements and 0 otherwise.
The maximum is based on the latest ban list information as retrieved from the ygoprodeck api.
One thousand hands are randomly generated for the deck.
The classifier from the Model Training stage is used to find a 'probability' of winning with each hand.
This is used to estimate the average win-rate for the deck.
The most promising decks are 'bred' together, taking the counts of each card as the 'genes' (with a limit of min 40 cards and max 60 cards).
The offspring are trialled, and the process is repeated.
The most successful deck is displayed and the top X performing decks are saved down.
Enjoy the new list!

# Notes
There are MANY shortcomings in this notebook, many of which I have already addressed in some updated code.
Of note:
- hand generation is slow;
- replay scraping does not work (new db auth);
- there is frequent use of parallel running code, rather than proper optimisation.

While I am very excited about what was achieved in this project, there is a lot of work left to do.
I'm excited to see where we go from here.

Please consider subscribing to my YT channel if you are interested in seeing more Yu-Gi-Oh! Ccoding projects in the future.