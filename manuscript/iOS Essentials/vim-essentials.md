## Vim Essentials
- h - Move Leftwards
- l - Move Rightwards
- j - Move Downwards
- k - Move Upwards
- w - Move to next word(Special characters also counted)
- W - Move to next word(Based on space separation)
- b - Move back to previous word(Special characters also counted)
- B - Move back to previous word(Based on space separation)
- ^ - Move to the first non blank character in the line
- $ - Move to the end of the line
- 0 - Move to the start of the line, includes blank character also
- } - Move to the next paragraph
- { - Move to the previous paragraph
- f<character> - Moves to the first word in that line moving forward that starts with that character
- F<character> - Moves to the first word in that line moving backward that starts with that character
- t<character> - Same as the f<character> but puts that cursor before the character
- T<character> - Same as F<character> but puts the cursor after the character
- o - Insert a new line below and enter into the insert mode
- <n><vim-command> - This performs the vim command n number of times
- <n>gg / <n>G / :<n> - Will go n lines below the beginning of the file, i.e will go to line number n
