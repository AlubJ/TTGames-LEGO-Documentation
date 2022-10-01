Collection File
============
The collection file is used to add loaded characters to the character grid and shop. The file is just a simple list of characters.

## Structure
The collection file is a list where the character and all arguments are on one single line.
```
collect "some_character" buy_in_shop 2000
```

The line must start with `collect` followed by a character name. Any extra parameters are optional. 

Comments are indicated with a single `//`, `;` or `#`.

## Parameters
`buy_in_shop [price]` - Adds the character to the shop with the given price.

`no_progress` - Adds the character to the character grid from the start of the game.

`do_not_show` - Removes the character from the character grid.

`cheat_code "[cheat code]"` - Adds the character to the shop and makes it buyable after redeeming a cheat code. Cheat codes have to be 6 digits long.

`area_complete "[level filename]"` - Adds the character to the shop and makes it buyable after beating a specific level.

`all_episodes_complete` - Adds the character to the shop and makes it buyable after beating all episodes.

`story` - Locks the character behind level completion.

`100_percent` - Locks the character behind 100% completion.
