---
layout: default
title: MS Word
parent: Application Tips
nav_order: 6
---
# MS Word

## Equation numbering and cross-referencing

1. For the equation numbers themselves, create a right tab stop on the line where the equation is. Use a center tab stop for the equation.

2. Where the equation number will be, create parentheses (). Then, with the curser inside the parentheses, select Quick Parts (in Text section of insert ribbon) > Field...

3. Select the SEQ field name, then in the text box for field codes write " EQ" after the "SEQ" that should already be there, so that it says "SEQ EQ"
  - you can use any code you want after SEQ, as long as you're consistent.
  - this causes the numbers to automatically increment for each successive equation.
  - if you add a new equation before other ones, you'll have to manually update the fields. You can select all the text where fields need to be updated, right click, and select "update field"

4. If you want to cross reference that figure number, first highlight the number you created from the above steps (not including the parentheses), then click bookmark (in the insert ribbon). Choose a descriptive name for your bookmark, like "eq_bvalue". Then, place the text curser where you want the cross reference in the text, and press the cross-reference button, and then select your bookmark.

