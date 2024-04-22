# INDEX.HTML EXPLANATION

* **Basic Structure:**
  * It follows the standard HTML structure with `DOCTYPE`, `<html>`, `<head>`, `<body>`, and closing tags.
* **Head Section:**
  * It sets the character encoding (`UTF-8`), viewport for responsiveness (`width=device-width, initial-scale=1.0`), and the page title ("Tic Tac Toe").
  * It includes a link to an external stylesheet (`style.css`) likely containing the visual styles for the game.
* **Body Section:**
  * **Game Board:**
    * A `div` with class `board` and ID `board` likely represents the main game board container.
    * Inside the `board` container, nine `div` elements with class `cell` and attribute `data-cell` are nested. These elements likely represent the individual squares on the Tic Tac Toe board.
  * **Winning Message:**
    * A `div` with class `winning-message` and ID `winningMessage` likely holds information displayed when a winner is determined.
    * Inside the `winning-message` container, a `div` with attribute `data-winning-message-text` might be used to display the winning message itself (e.g., "X Wins!").
    * A button with ID `resetButton` is likely for restarting the game.
* **Script:**
  * A reference to an external JavaScript file (`script.js`) is included. This file probably contains the game logic for handling turns, checking win conditions, and updating the game state based on user interaction.

# SCRIPT.JS EXPLANATION

**Constants:**

* `X_CLASS`: Represents the CSS class name for the "X" symbol (`'x'`).
* `CIRCLE_CLASS`: Represents the CSS class name for the "O" symbol (`'circle'`).
* `WINNING_COMBINATIONS`: An array containing lists of indexes representing winning positions (rows, columns, diagonals).

**DOM Element References:**

* `cellElements`: This selects all elements with the `data-cell` attribute, likely representing the individual squares on the board.
* `board`: This references the element with the ID `board`, likely the main game board container.
* `winningMessageElement`: This references the element with the ID `winningMessage`, which displays the winning message.
* `restartButton`: This references the button element with the ID `restartButton` for restarting the game.
* `winningMessageTextElement`: This selects the element with the `data-winning-message-text` attribute within the winning message container, likely used to display the specific winning message text.

**Functions:**

* `startGame()`: This function initializes the game state.
    * It sets the initial turn to "X" (not circleTurn).
    * It iterates through all `cellElements`:
        * Removes any existing "X" or "O" classes from the cells.
        * Removes any previously attached click event listeners.
        * Attaches a new click event listener to each cell using the `handleClick` function, ensuring it only fires once (`{ once: true }`).
    * It calls `setBoardHoverClass` to update the board's hover class based on the current turn.
    * It hides the winning message element.

* `restartButton.addEventListener('click', startGame)`: This adds a click event listener to the restart button. Clicking the button restarts the game by calling `startGame()`.

* `handleClick(e)`: This function handles a click event on a cell.
    * It retrieves the clicked cell element from the event object.
    * It determines the current class to be placed based on the turn (X_CLASS for X's turn, CIRCLE_CLASS for O's turn).
    * It calls `placeMark` to add the appropriate class to the clicked cell.
    * It checks for a win using `checkWin` with the current class. If a win is detected, it calls `endGame(false)` to display the winner.
    * If it's not a win, it checks for a draw using `isDraw`. If it's a draw, it calls `endGame(true)` to display a draw message.
    * If it's neither a win nor a draw, it swaps turns using `swapTurns` and updates the board's hover class using `setBoardHoverClass`.

* `endGame(draw)`: This function handles ending the game.
    * If `draw` is true, it sets the winning message text to "Draw!".
    * Otherwise, it displays the winning message based on the current turn ("X's Wins!" or "O's Wins!").
    * It shows the winning message element.

* `isDraw()`: This function checks for a draw condition. It uses the spread operator (`...`) to convert `cellElements` to an array and then uses `every` to check if all cells are marked with either "X" or "O" class.

* `placeMark(cell, currentClass)`: This function simply adds the provided `currentClass` (X_CLASS or CIRCLE_CLASS) to the cell element's class list.

* `swapTurns()`: This function switches the turn between "X" and "O" by negating the `circleTurn` variable.

* `setBoardHoverClass()`: This function updates the board's class based on the current turn. It removes any existing hover classes (X_CLASS or CIRCLE_CLASS) and adds the appropriate class based on the current turn (X_CLASS for X's turn, CIRCLE_CLASS for O's turn). This likely provides a visual indication of whose turn it is.

* `checkWin(currentClass)`: This function checks for a winning condition using the provided `currentClass`.
    * It iterates through each combination in `WINNING_COMBINATIONS`.
    * For each combination (represented by a list of indexes), it uses `every` to check if all cells in that combination have the `currentClass`.
    * If all cells in a combination have the same class, it means there's a win and the function returns `true`.
    * If no winning combination is found, the function returns `false`.

# STYLE.CSS EXPLANATION

* **Global Styles:**
  * It sets `box-sizing: border-box` for all elements, ensuring padding and border are included in the element's width and height.
  * It defines root variables:
      * `--cell-size`: This sets the base size of each cell (100px).
      * `--mark-size`: This calculates the size of the "X" and "O" marks based on the cell size (90% of the cell size).

* **Body:**
  * It removes any margin from the body element.

* **Game Board:**
  * The `.board` class styles the game board container.
      * It sets the width and height to 100% viewport dimensions (full screen).
      * It uses flexbox for layout with centering for both horizontal and vertical axes.
      * It defines a grid layout with three columns of automatic width.

* **Individual Cells:**
  * The `.cell` class styles the individual squares on the board.
      * It sets the width and height based on the `--cell-size` variable.
      * It adds a black border.
      * It uses flexbox for centering content within the cell.
      * It positions elements relatively within the cell.
      * It sets the cursor to "pointer" on hover.
  * The code then removes borders for specific cells to create a seamless grid-like appearance:
      * Top borders are removed for the first three cells in each row.
      * Left borders are removed for cells starting at the first cell of each column (`nth-child(3n + 1)`) .
      * Right borders are removed for cells ending at the last cell of each column (`nth-child(3n + 3)`) .
      * Bottom borders are removed for the last three cells.
  * Styles are applied to cells marked with "X" or "O" to prevent further interaction:
      * Cursor is set to "not-allowed" on these cells.

* **X and O Mark Styles:**
  * These styles define the appearance of the "X" and "O" marks.
      * Black background color is used for both "X" and the outer part of "O".
      * A light grey hover effect is applied to empty cells based on the current turn ("X" or "O").
  * The code creates the "X" symbol using two pseudo-elements (`::before` and `::after`) positioned absolutely within the cell.
      * They are rotated at 45 degrees in opposite directions to create the X shape.
  * The code creates the "O" symbol using a single pseudo-element with a border-radius set to 50% for a circle shape.
      * A white inner circle is created within the black border.

* **Winning Message:**
  * The `.winning-message` class styles the container for the winning message.
      * It's initially hidden (`display: none`).
      * It uses fixed positioning to cover the entire viewport when displayed.
      * It sets background color with transparency (black with 90% opacity).
      * It centers content horizontally and vertically using flexbox.
      * It sets white text color and a large font size (5rem).
  * Styles are defined for the button within the winning message:
      * It has a large font size (3rem).
      * It has a white background color with a black border.
      * It includes hover styles to invert colors (black background with white text and border).
  * The `.winning-message.show` class is used to display the winning message element by changing its display property to `flex`.
