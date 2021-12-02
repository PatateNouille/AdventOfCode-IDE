# AdventOfCode-IDE

A custom-made IDE for the Advent Of Code
https://adventofcode.com



# How to use

The IDE is adapted to a 3-step workflow that seems to be usable for every advent of code puzzle :

1. Parse the raw text given by the website as the puzzle's input into more managable and formated chunks
2. Parse the formated input into actual data objects, giving easy access to all necessary info to solve the puzzle
3. Actually solve the puzzle using the previously computed data

Each step is represented by a column (an additional column serves as the console).
Each column has a custom log at the top, a button to execute this step, the IO variables used (more on that later) and the actual code executed for this step

The IO variables are two standard variables used by each step, indicated with the IN / OUT above the code window.
For example, executing the first step auto sets the ``text`` variable with the content of the top left log, and prints the content of ``formated`` in the next log.
The IO variables are also usable in the next steps.

The console column has some special buttons :
- Clear console - simply clears the column, no undo !
- Save board - saves the board as a string in the clipboard, alternatively uses an alert to print the sharable string
- Load board - loads a board from the clipboard, simply copy a string generated with the save button before clicking it. **No undo !**



## 1 - Raw Input

The puzzle input is supposed to be pasted into the leftmost log (which also is the only one not in read-only).
Its content is simply copied into the IO var ``text``. It will probably have \r\n for each line break if you are using windows!

In general, the formatting is as simple as ``formated = text.split('\r\n');``. Its purpose really only is to make the input text more easily usable as an array of easily parasable strings.

## 2 - Formated Input

Once the input text has been broken up into formated chunks, it can be used to populate the IO var ``data`` with convinient objects for the puzzle at hand.
Usually either an array or a map is used to store these.

## 3 - Data Objects

Here is the bulk of the puzzle, the actual algorithm that should result in the puzzle's answer.
Using the previously made data objects should make the usage of the puzzle's input easy and convinient.

The IO var ``output`` (usually a number) should be set to the answer by the end of the 3rd step's code.
It is also automatically printed in the custom console (not the browser's one).



# Additional Methods

``Log(msg)``

Prints a message in the custom console. ``msg`` can be a ``number``, ``string``, ``array`` or ``object``.
Arrays and objects are formated using ``JSON.stringify``.



# Examples

07/12/2020 - Part two :
```json
{"rawCode":"formated = text.split('.\\r\\n');","inputCode":"data = {};\r\n\r\nlet container = /([a-z]+ [a-z]+) bags contain /g;\r\nlet subBag = /(\\d+) ([a-z]+ [a-z]+) bags?,?/g;\r\n\r\nlet str, result;\r\n\r\nlet bagCol, info, subBagCount;\r\n\r\nfor (let i = 0; i < formated.length; ++i)\r\n{\r\n    str = formated[i];\r\n\r\n    result = container.exec(str);\r\n    container.lastIndex = 0;\r\n\r\n    bagCol = result[1];\r\n\r\n    info = {\r\n        subBagCount: 0,\r\n        subBags: []\r\n    };\r\n\r\n    while ((result = subBag.exec(str)) !== null)\r\n    {\r\n        subBagCount = Number(result[1]);\r\n        info.subBagCount += subBagCount;\r\n\r\n        while (subBagCount-- > 0)\r\n        {\r\n            info.subBags.push(result[2]);\r\n        }\r\n    }\r\n    subBag.lastIndex = 0;\r\n\r\n    data[bagCol] = info;\r\n}","dataCode":"let queue = [];\r\n\r\nqueue.push(\"shiny gold\");\r\n\r\nlet curColor, info;\r\n\r\nwhile (queue.length > 0)\r\n{\r\n    curColor = queue.shift();\r\n\r\n    info = data[curColor];\r\n\r\n    output += info.subBagCount;\r\n\r\n    queue = queue.concat(info.subBags);\r\n}"}
```
