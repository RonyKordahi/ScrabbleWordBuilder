# Scrable Word Builder - Hasbro Pulse Coding Challenge

The solution is presented in the form of a JavaScript file that can be run in a terminal.

IDE used: **Visual Studio Code**

Runtime environment used: **Node.js**

## Setup

1. Download and install [Visual Studio Code](https://code.visualstudio.com/).
    - Optional. See [alternative run method 3](#3-alternative-method-without-visual-studio-code).
2. Download and install [Node.js (LTS)](https://nodejs.org/en).
    - **Required.** 

## Running the File

### 1. Recommended Method

1. Open the repository in Visual Studio Code
2. Open a new terminal by selecting `Terminal -> New Terminal` in the menu at the top.
3. Run the file by executing this command `node ./scrabble.js`

### 2. Alternative Method (In Visual Studio Code)

> This method is intended for users who have a space character in their system file names.

1. Open the repository in Visual Studio Code
2. Open a new terminal by selecting `Terminal -> New Terminal` in the menu at the top.
3. In the new terminal type `node` followed by a space. 
    - Do not press `ENTER`.
4. Drag and drop the `scrabble.js` file into the terminal.
5. Run the command by pressing `ENTER`.

### 3. Alternative Method (Without Visual Studio Code)

1. In a terminal or command prompt, navigate to the root of this repository.
2. Run the file by executing this command `node ./scrabble.js`

## Assumptions and Design Decisions

### Dictionary

The list of words in the dictionary is from an [online source](https://github.com/benjamincrom/scrabble/blob/master/scrabble/dictionary.json). 

I set all the words to be in capital to match the coding challenge instructions' examples after importing the data and removing the linebreaks to fit them into an array to allow for easy randomization of the selected board word, and later to parse through it for potential solution words.

### Letter Data

The letter data is from an [online source](https://github.com/dariusk/corpora/blob/master/data/games/scrabble.json). 

I set all the letters to be in capital to simplify the execution logic. The object format for the letter data is great for holding all the information related to the data (i.e. points and tiles), and can still be used to randomize which letters are picked for the rack by using the `Object.keys()` method to convert it into an array, and then randomize which letter is pick from there.

### Logic

#### Settings Variables

I set a few constant variables in the top of the file to act as the game settings. Modifying these would change some of the parameters of the game.

`NO_WORD_PROB`: Since the word on the board is optional, I set a variable to contain the probability of it not being given. Original value of `10`.

`MIN_RACK_LETTERS`: The minimum letters that can be put in the rack. Original value of `1` as per the instructions.

`MAX_RACK_LETTERS`: The maximum letters that can be put in the rack. Original value of `7` as per the instructions.

#### Word Finding Logic

Once a board word was set (or not) and the rack was set, potential words needed to be found.

I came up with 2 rules to simplify the search and refine the results.

<u>**Rule 1**</u>: If the word on the board is included in the dictionary words and the rest of the letters are available in the rack, then that word is valid.

Example: if the board word is `HAT` and the dictionary word is `THAT` or `HATS`.

To achieve this, I checked if the board word is included inside the dictionary word. If it is, I would remove the board word from the dictionary word, take the remaining letters and check if they are all available in the rack.

<u>**Rule 2**</u>: If the dictionary word could be made using only one letter from the word on the board and the rest of the letters are available in the rack, then that word is valid.

Example: if the board word is `CHILL`, the rack contains `L, I, Y`, and the dictionary word is `LILY`, then that word is valid.

To achieve this, I checked if every letter of the word is available in the rack, and for any missing letters I would check the word on the board instead. If more than one letter is missing from the rack and found in the board, the dictionary word is marked as invalid and skipped. If there is only one missing letter and it is available in the word on the board, then the dictionary word is valid.