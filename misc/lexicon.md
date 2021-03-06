## [00] (Line Break)
 This code causes a line break in a text window.  The cursor will drop down to the start of the next line.  If the cursor is already at the bottom of the window, the text will simply be scrolled up and will start printing again at the start of the same line.  As commonly used in EB, a [00] will either be followed by a bullet symbol (type @ in most EB editors) or two spaces to keep everything nice and lined up.

## [01] (Start on Blank Line)
 This code behaves similarly to [00]. It will cause the text window's cursor to drop down to the next line. However, this code will have no effect if the cursor is already on a blank line. This code is useful for ensuring that text will be printed on a new line when you do not know what text will preceed it, such as after a menu or conditional jump.

## [02] (Stop Parsing)
 This code marks the end of a text block or text string.  Upon reading a [02], the engine will stop looking at this particular block and go on with whatever it was doing prior to reading it.  It's very important to put this at the end of all text blocks!  Otherwise, the engine will just keep surfing through ROM data until it hits one, or enough non-text code to make it glitch horribly.  Ever seen the infamous "Multi-Task code" that makes the engine keep reading seemingly unrelated text blocks endlessly (Game Genie EE64-54A1)?  Well, that's exactly what we're talking about here.  That code merely disables the effect of the [02] control code.

## [03] (Halt Parsing with Prompt - Variable)
 This code pauses text reading and waits until the player presses the A button.  It also displays the blinking triangle along the bottom of the window.  Note that its effect changes when in-battle:  When text speed is set to medium or fast, it will simply act as a short delay.  If you want to make sure it stops with the blinking triangle when in battle, you should use [14] (Halt Parsing with Prompt) (see that code's entry for more details).   Common usage of [03]'s in EarthBound is to place it before [00] (Line Breaks).  That way, the player won't miss anything that scrolls by too quickly.

## [04 XX XX] (Toggle On Event Flag)
 This code sets a value of 1 to event flag $XXXX.  The flag is then considered to be toggled "on."  Remember to reverse the flag number - to toggle on $0123, you would use [04 23 01].

## [05 XX XX] (Toggle Off Event Flag)
 This code sets a value of 0 to event flag $XXXX.  The flag is then considered to be toggled "off."  Remember to reverse the flag number - to toggle off $0123, you would use [05 23 01].

## [06 XX XX YY YY YY YY] (Event-Dependent Pointer)
 This code checks the status of event flag $XXXX.  If its value is set to 0x01 (Boolean True), it jumps to pointer $YYYYYY and continues reading from there.  If the flag is set to 0x00 (Boolean False), the code does nothing and the engine simply continues past it.

## [07 XX XX] (Return Event Flag)
 This code checks the status of event flag $XXXX, and fills the Working Memory data with that value.  This is useful if you need to use the value of an event flag in conjunction with the Boolean Pointer codes, [1B 02 XX XX XX 00] (Boolean-False Pointer) and [1B 03 XX XX XX XX] (Boolean-True Pointer) (see those codes' entries for more information).  Note: The code string [07 XX XX][1B 03 XX XX XX XX] is exactly the same as using a [06 XX XX YY YY YY YY] (Event-Dependent Pointer), except it overwrites whatever's currently in Working Memory.

## [08 XX XX XX XX] (Reference Pointer)
 This code references the data at $XXXXXX.  It will read all the text there until it reaches a [02] (Stop Parsing) code.  When that happens, it will continue parsing from the [08 XX XX XX XX] code that initally made the reference.  Contrast this against the [0A XX XX XX XX] (Jump Pointer) (see that code's entry for more information).

## [09 XX (YY YY YY YY)] (Multiple-Address Jump Pointer)
 This code behaves similarly to [0A XX XX XX XX] (Jump Pointer); however, it has the added benefit of being able to have multiple branching pointers within itself.  This one does take a bit of explanation.  The value of the first argument, $XX, determines how many pointers there will be in the code.  Codes that change lengths based upon the value of an argument are somewhat rare, but this one is fairly straightforward.  For example, if $XX were 0x01, the format of the code would be [09 XX YY YY YY YY].  However, if we changed $XX to 0x02, the format would change to [09 XX Y1 Y1 Y1 Y1 Y2 Y2 Y2 Y2].  If $XX is 0x03, the code becomes [09 XX Y1 Y1 Y1 Y1 Y2 Y2 Y2 Y2 Y3 Y3 Y3 Y3].  By this point you should get the idea.  The upper limit on this is 0xFF, or 255 pointers, though one would hope you'd never have to fill in that many pointer spaces.  [09 XX (YY YY YY YY)] starts by checking the Working Memory.  It then compares that value against $XX.  If the number in memory is either 0x00 or higher than $XX, the code will be ignored and the text will continue parsing past it.  Otherwise, the code will select a pointer based on the memory value.  If the value is 0x01, it will use the first pointer in the code's list; if the value's 0x02, it uses the second one, and so forth for the rest of them.  The code jumps to the data at the selected pointer and parses from that location as normal.  As with [0A XX XX XX XX], it will not return to the location of the original [09 XX (YY YY YY YY)] when it reaches a [02] (Stop Parsing) code; it will simply stop parsing.

## [0A XX XX XX XX] (Jump Pointer)
 This code jumps to the data at $XXXXXX and parses from that location as normal.  It will not return to the location of the original [0A XX XX XX XX] when it reaches a [02] (Stop Parsing) code, it will simply stop parsing.  Contrast this against the [08 XX XX XX XX] (Reference Pointer) (see that code's entry for more information).

## [0B XX] (Perform Boolean True Check - Equal to Working Memory)
 This code will compare the value in Working Memory to $XX.  It will then change the value in Working Memory based on the result; the value will be set to 0x00 (Boolean False) if it's not equal to $XX, or 0x01 (Boolean True) if it is equal.  This code lets you run the Boolean pointers, [1B 02 XX XX XX XX] (Boolean-False Pointer) and [1B 03 XX XX XX XX] (Boolean-True Pointer), based on the results of ANY code that stores data to Working Memory.  This allows for lots of flexibility in terms of conditional text based on whether you have a given item, who's in the player's party, or even what side the player is checking something on, just to cite a few examples.  Keep in mind, though, this code DOES delete what value is in the Working Memory in the process!  If you wanted a sequence of [0B XX] checks for various values, you'd want to use [1B 00] (Copy All Memory to Storage) before each [0B XX] code and [1B 01] (Copy All Storage to Memory) after any boolean pointers following them to make sure that the original value being checked is not lost (see those codes' entries for more information).

## [0C XX] (Perform Boolean False Check - Equal to Working Memory)
 This code will compare the value in Working Memory to $XX.  It will then change the value in Working Memory based on the result; the value will be set to 0x00 (Boolean False) if it is equal to $XX, or 0x01 (Boolean True) if it's not equal.  This code lets you run the Boolean pointers, [1B 02 XX XX XX XX] (Boolean-False Pointer) and [1B 03 XX XX XX XX] (Boolean-True Pointer), based on the results of ANY code that stores data to Working Memory.  This allows for lots of flexibility in terms of conditional text based on whether you have a given item, who's in the player's party, or even what side the player is checking something on, just to cite a few examples.  Keep in mind, though, this code DOES delete what value is in the Working Memory in the process!  If you wanted a sequence of [0C XX] checks for various values, you'd want to use [1B 00] (Copy All Memory to Storage) before each [0C XX] code and [1B 01] (Copy All Storage to Memory) after any boolean pointers following them to make sure that the original value being checked is not lost (see those codes' entries for more information).

## [0D XX] (Copy to Argumentative Memory)
 This code will copy the value stored in either Working or Secondary Memory into Argumentary Memory.  It will not affect the value stored in the register copied from.  The $XX value determines what memory to copy from, where a value 0x00 indicates Working Memory, and 0x01 indicates Secondary Memory.

## [0E XX] (Store to Secondary Memory)
 This code stores the value in $XX to Secondary Memory.  There are not many codes that manipulate Secondary memory. However, using [0E XX][0D 01] is the simplest way to load a constant value into Argumentary Memory.

## [0F] (Increment Secondary Memory)
 This code will increase the value stored in Secondary Memory by 1. In general, this code is used to create simple loops.

## [10 XX] (Delay Parsing)
 This code stops the text parsing briefly for dramatic purposes.  The duration of the pause is controlled by the value of XX.  The engine reads this value as units 1/60th of a second long.  Therefore, a one-second pause would be [10 3C], a two-second pause [10 78], and so forth.  The maximum value - 0xFF - is a 4.25 second pause.  These can be used any time in text when you want a pause of a specific length.  String them together to achieve longer pauses.

## ***** [11] (Create Selection Menu from Strings) *****

## [12] (Clear Text Line)
 This code erases all of the text on the current line and moves the cursor back to the beginning.  This is often used in text menus, number selectors, and other similar cases.  It's not commonly used in cases of generic conversational text.

## [13] (Halt Parsing without Prompt)
 The [13] code is highly similar to the codes [03] (Halt Parsing with Prompt - Variable), and [14] (Halt Parsing with Prompt).  Like those other two, it stops parsing text until the player presses the A button.  However, the main difference here is that it does not display the blinking triangle prompt.  This is commonly used when you want the player to have to hit A, but don't have a text window open at the time.  Also, it's standard procedure to place a [13] before a [02] (Stop Parsing).  That way, the player must confirm the end of the block and closing of the window, but without leading them to believe there's more text to come as a [03] might.

## [14] (Halt Parsing with Prompt)
 This code is virtually identical to [03] (Halt Parsing with Prompt - Variable).  It pauses the text, displays a blinking triangle prompt, and waits until the player presses the A button.  However, the main difference is that its function does not change when you're in battle.  This is the code you want to use if you're composing text to be displayed in battle.

## [15 XX] (Display Compressed Text/Bank 0)

## [16 XX] (Display Compressed Text/Bank 1)

## [17 XX] (Display Compressed Text/Bank 2)
 These three codes all work to make EB's text more efficient.  Each code links to one of three banks of text.  Each bank contains 256 (0x00 - 0xFF) different phrases, words, and character clusters.  As the engine parses the code, it will reference entry $XX from the proper bank, and substitute the text in that entry for the [15/16/17] code itself.  JHack's text editor automatically compresses its text, so you won't have to manually convert for these codes.  However, if you're entering your text in another program (say, a hex editor), you can also use CT-Boing Ultra to compress it.  Both programs are available from our downloads page.

## [18 00] (Close Current Window)
 This code closes the most recently-used text window.  Typically, only one window will be open at any given time, but if there are multiples open due to [18 01 XX] (Open Text Window) or another code, the one that was most recently opened (or most recently parsed in) will be the first to go.  If you want to close a specific window but aren't currently parsing in it, use a [18 03 XX] (Switch To Window) code (see the above codes' entries for more information).

## [18 01 XX] (Open Text Window)
 This code opens a text window and selects it as the focus window for parsing text.  The $XX argument determines the size and position of the window, which it reads from a database in the ROM.  Note that the engine will not support opening more than one of the same window.  If window type $XX is already open, this code will close it and then open it again, effectively clearing it like [18 06] (Clear Current Window).

## ***** [18 02] (UNKNOWN) *****

## [18 03 XX] (Switch To Window)
 This code selects the window type $XX as the focus for parsing text.  Any text data read by the engine following this code will be displayed in this window.  If a window is overlapping the one switched to, the engine will not bring it 'forward' so all of it is visible.  NOTE: Do not use this code if the window type $XX has not already been opened!  Attempting to switch to a closed window type will immediately crash the game.

## [18 04] (Close All Windows)
 This code, fairly straightforward, simply closes all opened windows.  This does not need to be used at the end of a text block, as the engine automatically performs that action.  However, in cases where animation, movement, or teleportation are used, one would be well advised to close the windows beforehand.  If you wish to halt parsing of the text while windows are closed, [13] (Halt Parsing without Prompt) should be used rather than [03] (Halt Parsing with Prompt - Variable) or [14] (Halt Parsing with Prompt).  The other two will display the triangle prompt icon floating in the top-left corner of the screen.

## [18 05 XX YY] (Force Text Alignment)
 This code forces text to appear in a specific location in a window.  $XX is rendered as the X-axis offset (horizontal), and is measured in pixels.  Text parsed after this code will start printing $XX pixels over from the left-hand side.  $YY is rendered as the Y-axis offset (vertical), and is measured in rows (16 pixels each).  Text parsed after this code will start printing $YY rows down from the top.  This code is extensively used in the status menu, among other locations.

## [18 06] (Clear Current Window)
 This code eliminates whatever text that's displayed in the current window.  The cursor will be returned to the top left corner of the window, as if it had just been opened.

## ***** [18 07 XX XX XX XX YY] (UNKNOWN) *****

## ***** [18 08 XX XX XX] (UNKNOWN) *****

## ***** [18 09 XX XX XX] (UNKNOWN) *****

## [18 0A] (Show Wallet Window)
 This code opens a small window in the center-left of the screen, and automatically displays the amount of money on hand.  This is used in various places; notably in the code for stores and telephones.

## ***** [18 0D XX XX] (UNKNOWN) *****

## [19 02] (Load String to Memory)

## ***** [19 04] (UNKNOWN) *****

## [19 05 XX YY YY] (Inflict Status Change)

## [19 10 XX] (Return Character Number)

## [19 11 XX] (Return One Letter from a Character's Name)
 This code looks at the value in Secondary Memory, takes the letter in that position in character $XX's name, and puts its value into Working Memory. The corresponding values can be found in the Standard Argument Listing. Knowing this value generally isn't necessary, as this code is almost always followed by [0D 00][1C 03 00] to print the letter in question.

## ***** [19 14] (UNKNOWN) *****

## [19 16 XX YY] (Return Byte YY of Character's Status)

## ***** [19 18 XX] (UNKNOWN) *****

## [19 19 00 00] (Pass Item Number to Working Memory)

## ***** 19 1A XX,"UNKNOWN"; *****

## ***** 19 1B XX,"UNKNOWN"; *****

## ***** 19 1C XX YY,"UNKNOWN"; *****

## ***** 19 1D XX YY,"UNKNOWN"; *****

## ***** 19 1E,"UNKNOWN"; *****

## ***** 19 1F,"UNKNOWN"; *****

## ***** 19 20,"UNKNOWN"; *****

## ***** 19 21 XX,"UNKNOWN"; *****

## [19 22 XX YY ZZ ZZ] (Return Direction from Character to Object?)

## [19 23 XX XX YY YY] (Return Direction from NPC to Object?)

## [19 24 XX XX YY YY] (Return Direction from Sprite to Object?)

## ***** 19 25 XX,"UNKNOWN"; *****

## **** 19 26 XX,"UNKNOWN"; *****

## **** 19 27 XX,"UNKNOWN"; *****

## **** 19 28 XX,"UNKNOWN"; *****

## ***** 1A 00,"UNKNOWN INCOMPLETE"; *****

## [1A 01 WW WW WW WW XX XX XX XX YY YY YY YY ZZ ZZ ZZ ZZ] (Current Party Member Selection Menu)

## **** 1A 04,"UNKNOWN INCOMPLETE"; *****

## [1A 05 XX YY]: Display the inventory of character YY in window XX.

## [1A 06 XX] (Display Shop Menu)

## ***** 1A 07,"Related to Escargo Express stored goods window"; *****

## ***** 1A 08,"UNKNOWN INCOMPLETE"; *****

## ***** 1A 09,"UNKNOWN INCOMPLETE"; *****

## [1A 0A] (Open Phone Menu - Dummy?)

## ***** 1A 0B,"UNKNOWN"; *****

## [1B 00] (Copy Active Memory to Storage)
 This code copies the values in all Active Memory to their respective Storage Memory registers.  This will overwrite whatever is in Working Memory Storage, Argumentative Memory Storage, and Secondary Memory Storage.  The values will remain in the Active Memory registers.  This is the most commonly used way to store memory data before using a boolean check or another code that will fill an Active Memory register with its results.

## [1B 01] (Copy Storage to Active Memory)
 This code copies the values in all Storage Memory to their respective Active Memory registers.  This will overwrite whatever is in Active Working Memory, Active Argumentative Memory, and Active Secondary Memory.  The values will remain in the Storage Memory registers.  This code is often used following a boolean check or other code that manipulates the value in an Active Memory register, when the initial value has been saved using a [1B 00] (Copy All Memory to Storage) code.

## [1B 02 XX XX XX XX] (Boolean-False Pointer)
 This code checks the value stored in Working Memory.  If its value is set to 0x00 (Boolean False), it jumps to pointer $XXXXXX and continues reading from there.  If the flag is set to 0x01 (Boolean True), the code does nothing and the engine simply continues past it.  Any other value stored in Working Memory will be treated as 0x01 and the code will be summarily ignored.  As a jump pointer, it will not return to the location of the original [1B 02 XX XX XX XX] when it reaches a [02] (Stop Parsing) code, it will simply stop parsing.

## [1B 03 XX XX XX XX] (Boolean-True Pointer)
 This code checks the value stored in Working Memory.  If the flag is set to 0x00 (Boolean False), the code does nothing and the engine simply continues past it.  If its value is set to 0x01 (Boolean True), it jumps to pointer $XXXXXX and continues reading from there.  Any other value stored in Working Memory will be treated as 0x01 and the the engine will successfully jump to the pointer.  As a jump pointer, it will not return to the location of the original [1B 02 XX XX XX XX] when it reaches a [02] (Stop Parsing) code, it will simply stop parsing.

## [1B 04] (Swap Working and Argumentative Memory)
 This code exchanges the values in the Working and Argumentative Memory registers.  Whatever is in Working Memory will be placed in Argumentative Memory, and whatever's in Argumentative Memory will be placed in Working Memory.  This is useful when you've used a code to generate some value or another, and want to use it in a code that accepts values from Argumentative Memory.


## [1B 05] (Copy Active Memory to WRAM)

## [1B 06] (Copy WRAM to Active Memory)


## [1C 00 XX] (Text Color Effects)

## [1C 01 XX] (Display Statistics)

## [1C 02 XX] (Display Character Name)
 This code prints the names of the characters that can join your party in the current window. Where the value of $XX signifies the character to use.  [1C 02 XX] uses the standard Argument Listing for playable characters and NPCs.  Check the Standard Argument Listing for more details on usage.

## [1C 03 XX] (Display Text Character)
 This code prints out text character $XX. In general, this code will always be used with 00 as the argument, loading a previously previously determined value from Argumentary Memroy. See [19 11 XX]. If you should ever want to use this code with a constant argument, refer to the standard argument listing for values.

## [1C 04] (Open HP/PP Windows)
 This code opens the HP/PP status windows at the bottom of the screen.  It's often used in cases where either stat is being changed or in the item shop windows.  The windows will close when the text block they were opened in stops parsing.

## [1C 05 XX] (Display Item Name)
 This code prints the name of an item in the current window.  The value of $XX is the item number that should be displayed.  However, if the value used is 0x00, the engine will instead use the value stored in Argumentary Memory in its place.  As there can be over 200 items in EarthBound, it would be inefficient to display all possible arguments here.  However, conveniently enough, the Item Editor will display the correct number value next to the item name itself, which should provide as suitable a reference means as any hacker needs.

## [1C 06 XX] (Display Teleport Destination Name)
 This code prints the name of a destination from the Teleport psychic ability in the current window.  The $XX value can be any one of the entries, with values ranging from 0x01 to 0x10.  If the value used is 0x00, the engine will instead use the value stored in Argumentary Memory in its place.  Any other value will result in a null response.  These names can be seen and edited in the Teleport Destination Editor in JHack.

## [1C 07 XX] (Display Text Strings Horizontally)

## [1C 08 XX] (Display Text Graphics)
 This code prints special images, typically seen in battle text, in the current window.  There are only two known arguments.  An $XX value of 0x01 will display the "SMAAAASH!" graphic, and a value of 0x02 will display the "YOU WON!" graphic.  Any other value will result in a null response.  [1C 08 XX] does not load values from the Argumentary Memory in the event that the $XX value is 0x00.

## ***** 1C 09,"UNKNOWN"; *****

## [1C 0A XX XX XX XX] (Display Numeric Value)
 This code prints the value of $XXXXXXXX as a decimal number in the current window.  If the value of $XXXXXXXX is 0x00000000, the engine will instead read the value stored in Argumentary Memory, and display that value as a decimal number.

## [1C 0B XX XX XX XX] (Display Numeric Value as Money)
 This code prints the value of $XXXXXXXX as a monetary value in the current window.  It is formatted like the money values in EarthBound's store windows; the number is aligned to the right side of the window, with a "$" character preceeding it and the ".00" character following it.  If the value of $XXXXXXXX is 0x00000000, the engine will instead read the value stored in Argumentary Memory, and display that value as a decimal number.

## [1C 0C XX] (Display Text Strings Vertically)


## [1C 0D] (Display Action User Name) ----\__ What memory do these read from?  We should know that.

## [1C 0E] (Display Action Target Name) --/


## ***** 1C 0F,"UNKNOWN"; *****

## ***** 1C 11 XX,"UNKNOWN"; *****


## [1C 12 XX] (Display PSI Name)

## [1C 13 XX YY] (Display Battle Animation)

## ***** 1C 14 XX,"UNKNOWN"; *****

## ***** 1C 15 XX,"UNKNOWN"; *****

## [1D 00 XX YY] (Give Item to Character and Return Recipient)
 This code will add an item to a character's inventory.  The value in $XX stands for the character number, and the $YY value stands for the item number to be added.  Nothing will occur if the character does not have any empty slots in his or her inventory.  However, if any character recieves the item successfully, the value of Working Memory will also be changed to the number of the character whom recieved the item. This and $XX use the Standard Argument Listing for playable characters and NPCs, and the value of $YY is the item number that will be added. Check the Standard Argument Listing for more details on usage.

## [1D 01 XX YY] (Take Item from Character)
 This code will remove an item from a character's inventory.  The value in $XX stands for the character number, and the $YY value stands for the item number to be removed.  $XX uses the standard Argument Listing for playable characters and NPCs, and the value of $YY is the item number that will be added.  Nothing will occur if the character does not have the item in his or her inventory.  Check the Standard Argument Listing for more details on usage.

## [1D 02 XX] (Perform Boolean False Check - Inventory Capacity)
 This code will check the inventory of the character whose number is equal to $XX.  It will then change the value in Working Memory based on the result; Working Memory will be set to 0x00 (Boolean False) if empty item slots exist in the character's inventory, or 0x01 (Boolean True) if no space remains in the inventory.  $XX uses the standard Argument Listing for playable characters and NPCs.  Check the Standard Argument Listing for more details on usage.

## [1D 03 XX] (Perform Boolean True Check - Inventory Vacancy)
 This code will check the inventory of the character whose number is equal to $XX.  It will then change the value in Working Memory based on the result; Working Memory will be set to 0x00 (Boolean False) if no space remains in the character's inventory, or 0x01 (Boolean True) if empty item slots exist in the inventory.  $XX uses the standard Argument Listing for playable characters and NPCs.  Check the Standard Argument Listing for more details on usage.

## [1D 04 XX YY] (Perform Boolean True Check - Absence of Item)
 This code will check the inventory of the character whose number is equal to $XX to see if they are carrying the item whose number is stored in $YY.  It will then change the value in Working Memory based on the result; Working Memory will be set to 0x00 (Boolean False) if character $XX does not have the item, or 0x01 (Boolean True) if that character has at least one of the item.  $XX uses the standard Argument Listing for playable characters and NPCs.  Check the Standard Argument Listing for more details on usage.

## [1D 05 XX YY] (Perform Boolean True Check - Presence of Item)
 This code will check the inventory of the character whose number is equal to $XX to see if they are carrying the item whose number is stored in $YY.  It will then change the value in Working Memory based on the result; Working Memory will be set to 0x00 (Boolean False) if character $XX has at least one of the item, or 0x01 (Boolean True) if that character does not have the item.  $XX uses the standard Argument Listing for playable characters and NPCs.  Check the Standard Argument Listing for more details on usage.

## [1D 06 XX XX XX XX] (Increase ATM Balance)
 This code will increase the balance stored in the ATM by the value of $XXXXXXXX.  If the value of $XXXXXXXX is 0x00000000, the engine will instead read the value stored in Argumentary Memory and increase the ATM balance accordingly.  The ATM balance can not exceed a value of 9,999,999 (0x98967F).

## [1D 07 XX XX XX XX] (Decrease ATM Balance)
 This code will decrease the balance stored in the ATM by the value of $XXXXXXXX.  If the value of $XXXXXXXX is 0x00000000, the engine will instead read the value stored in Argumentary Memory and decrease the ATM balance accordingly.  The ATM balance can not be reduced below 0.  That'd just be silly.

## [1D 08 XX XX] (Increase Wallet Balance)
 This code will increase the balance stored in the Wallet by the value of $XXXX.  If the value of $XXXX is 0x0000, the engine will instead read the value stored in Argumentary Memory and increase the Wallet balance accordingly.  The Wallet balance can not exceed a value of 99,999 (0x01869F).

## [1D 09 XX XX] (Decrease Wallet Balance)
 This code will decrease the balance stored in the Wallet by the value of $XXXX.  If the value of $XXXX is 0x0000, the engine will instead read the value stored in Argumentary Memory and decrease the Wallet balance accordingly.  The Wallet balance can not be reduced below 0.  That'd just be silly.

## [1D 0A XX] (Return Price of Item)
 This code will change the value stored in Working Memory to match the price of the item whose number is stored in $XX.  To use this returned information you will likely have to transfer it to Argumentary Memory, using [1B 04] (Swap Working and Argumentative Memory) or [0D XX] (Copy to Argumentative Memory).  See those codes' entries for more information.

## [1D 0B XX] (Return Selling Price ofItem)
 This code will change the value stored in Working Memory to match one half of the price of the item whose number is stored in $XX.  To use this returned information you will likely have to transfer it to Argumentary Memory, using [1B 04] (Swap Working and Argumentative Memory) or [0D XX] (Copy to Argumentative Memory).  See those codes' entries for more information.

## ***** 1D 0C XX XX,"UNKNOWN"; *****

## [1D 0D XX YY YY] (Check for Status Ailment)

## [1D 0E XX YY] (Give Item to Character, Return Recipient and Number of Items)
 This code will add an item to a character's inventory.  The value in $XX stands for the character number, and the $YY value stands for the item number to be added.  Nothing will occur if the character does not have any empty slots in his or her inventory.  However, if any character receives the item successfully, the value of Argumentative Memory will also be changed to the number of items they have in their inventory (including the new one). Additionally, the value of Working memory will be changed to the number of the character whom recieved the item  $XX uses the standard Argument Listing for playable characters and NPCs, and the value of $YY is the item number that will be added.  Check the Standard Argument Listing for more details on usage.


## ***** 1D 0F XX XX,"UNKNOWN"; *****

## ***** 1D 10 XX XX,"UNKNOWN"; *****

## ***** 1D 11 XX XX,"UNKNOWN"; *****

## ***** 1D 12 XX XX,"UNKNOWN"; *****

## ***** 1D 13 XX XX,"UNKNOWN"; *****

## [1D 14 XX XX XX XX] (Check for Cash on Hand)
True if X<$

## ***** 1D 15 XX XX,"UNKNOWN"; *****

## [1D 17 XX XX XX XX] (Check for Cash in ATM)

## ***** 1D 18 XX,"UNKNOWN"; *****

## [1D 19 XX] (Boolean Check for Number of Party Members)  NOTE: Teddy doesn't count!

## [1D 20] (Check for User Targeting Self)

## [1D 21 XX] (Generate Random Number)

## [1D 22] (Check for Exit Mouse Compatibility)

## ***** 1D 23 XX,"UNKNOWN"; *****

## ***** 1D 24 XX,"UNKNOWN"; ***** (Return Cash Earned Since Last Call?)

## [1E 00 XX YY] (Recover HP by Percent)
 This code will increase the Current HP Stat of character number $XX by $YY percent of that character's Maximum HP Stat value.  If this code is used while the HP/PP status windows are open, changes in the stat will be shown as rolling values.  The Current HP Stat may not be increased beyond the Maximum HP Stat value.  $XX uses the standard Argument Listing for playable characters and NPCs.  Check the Standard Argument Listing for more details on usage.

## [1E 01 XX YY] (Deplete HP by Percent)
 This code will decrease the Current HP Stat of character number $XX by $YY percent of that character's Maximum HP Stat value.  If this code is used while the HP/PP status windows are open, changes in the stat will be shown as rolling values.  The Current HP Stat may not be decreased below zero.  If a character's HP is reduced below zero, their status will change to "Unconscious" when the text block ends.  $XX uses the standard Argument Listing for playable characters and NPCs.  Check the Standard Argument Listing for more details on usage.

## [1E 02 XX YY] (Recover HP by Amount)
 This code will increase the Current HP Stat of character number $XX by $YY points.  If this code is used while the HP/PP status windows are open, changes in the stat will be shown as rolling values.  The Current HP Stat may not be increased beyond the Maximum HP Stat value.  $XX uses the standard Argument Listing for playable characters and NPCs.  Check the Standard Argument Listing for more details on usage.

## [1E 03 XX YY] (Deplete HP by Amount)
 This code will decrease the Current HP Stat of character number $XX by $YY points.  If this code is used while the HP/PP status windows are open, changes in the stat will be shown as rolling values.  The Current HP Stat may not be decreased below zero.  If a character's HP is reduced below zero, their status will change to "Unconscious" when the text block ends.  $XX uses the standard Argument Listing for playable characters and NPCs.  Check the Standard Argument Listing for more details on usage.

## [1E 04 XX YY] (Restore PP by Percent)
 This code will increase the Current PP Stat of character number $XX by $YY percent of that character's Maximum PP Stat value.  If this code is used while the HP/PP status windows are open, changes in the stat will be shown as rolling values.  The Current PP Stat may not be increased beyond the Maximum PP Stat value.  $XX uses the standard Argument Listing for playable characters and NPCs.  Check the Standard Argument Listing for more details on usage.

## [1E 05 XX YY] (Consume PP by Percent)
 This code will decrease the Current PP Stat of character number $XX by $YY percent of that character's Maximum PP Stat value.  If this code is used while the HP/PP status windows are open, changes in the stat will be shown as rolling values.  The Current PP Stat may not be decreased below zero.  $XX uses the standard Argument Listing for playable characters and NPCs.  Check the Standard Argument Listing for more details on usage.

## [1E 06 XX YY] (Restore PP by Amount)
 This code will increase the Current PP Stat of character number $XX by $YY points.  If this code is used while the HP/PP status windows are open, changes in the stat will be shown as rolling values.  The Current PP Stat may not be increased beyond the Maximum PP Stat value.  $XX uses the standard Argument Listing for playable characters and NPCs.  Check the Standard Argument Listing for more details on usage.

## [1E 07 XX YY] (Consume PP by Amount)
 This code will decrease the Current PP Stat of character number $XX by $YY points.  If this code is used while the HP/PP status windows are open, changes in the stat will be shown as rolling values.  The Current PP Stat may not be decreased below zero.  $XX uses the standard Argument Listing for playable characters and NPCs.  Check the Standard Argument Listing for more details on usage.

## [1E 08 XX YY] (Change Character Level Stat)
 This code will increase the Level Stat of character number $XX to level number $YY.  Related statistic boosts and PSI gains are automatically factored but are not displayed in text form.  If this code is used while the HP/PP status windows are open, changes in either stat will be shown as rolling values.  The Experience Stat may exceed a value of 0x63 (99), however graphical glitching may occur if the HP and PP stats are raised beyond their normal display limits.  $XX uses the standard Argument Listing for playable characters and NPCs.  Check the Standard Argument Listing for more details on usage.

## [1E 09 XX YY YY YY] (Boost Experience Stat)
 This code will increase the Experience Stat of character number $XX by $YYYYYY points.  If this code causes the Experience Stat to pass a level-up threshold, the appropriate level-up text will be displayed in the current window and the level-up background music will play (the music will override map music until it is forcibly reset by a code such as [1F 03] (Restore Default Music)).  The Experience Stat can not exceed a value of 9,999,999 (0x98967F).  $XX uses the standard Argument Listing for playable characters and NPCs.  Check the Standard Argument Listing for more details on usage.

## [1E 0A XX YY] (Boost IQ Stat)
 This code will increase the IQ Stat of character number $XX by $YY points.  The IQ Stat is only one byte in length, so if this code causes it to increase beyond 0xFF, only the low byte will be used.  $XX uses the standard Argument Listing for playable characters and NPCs.  Check the Standard Argument Listing for more details on usage.

## [1E 0B XX YY] (Boost Guts Stat)
 This code will increase the Guts Stat of character number $XX by $YY points.  The Guts Stat is only one byte in length, so if this code causes it to increase beyond 0xFF, only the low byte will be used.  $XX uses the standard Argument Listing for playable characters and NPCs.  Check the Standard Argument Listing for more details on usage.

## [1E 0C XX YY] (Boost Speed Stat)
 This code will increase the Speed Stat of character number $XX by $YY points.  The Speed Stat is only one byte in length, so if this code causes it to increase beyond 0xFF, only the low byte will be used.  $XX uses the standard Argument Listing for playable characters and NPCs.  Check the Standard Argument Listing for more details on usage.

## [1E 0D XX YY] (Boost Vitality Stat)
 This code will increase the Vitality Stat of character number $XX by $YY points.  The Vitality Stat is only one byte in length, so if this code causes it to increase beyond 0xFF, only the low byte will be used.  $XX uses the standard Argument Listing for playable characters and NPCs.  Check the Standard Argument Listing for more details on usage.

## [1E 0E XX YY] (Boost Luck Stat)
 This code will increase the Luck Stat of character number $XX by $YY points.  The Luck Stat is only one byte in length, so if this code causes it to increase beyond 0xFF, only the low byte will be used.  $XX uses the standard Argument Listing for playable characters and NPCs.  Check the Standard Argument Listing for more details on usage.


## [1F 00 XX YY] (Play Music Track)

## ***** 1F 01 XX,"UNKNOWN"; ***** (Stop Music)

## [1F 02 XX] (Play Sound Effect)

## [1F 03] (Restore Default Music)

## [1F 04 XX] (Toggle Text Printing Sound)

## [1F 05] (Disallow Sector Boundaries to Change Music)

## [1F 06] (Allow Sector Boundaries to Change Music)

## [1F 07 XX] (Apply Music Effect)

## [1F 11 XX] (Add Party Member)

## [1F 12 XX] (Remove Party Member)

## [1F 13 XX YY] (Change Direction of Character)

## ***** 1F 14 XX,"UNKNOWN"; *****

## [1F 15 XX XX YY YY ZZ] (Generate Active Sprite)

## [1F 16 XX XX YY] (Change Direction of TPT Entry)

## [1F 17 XX XX YY YY ZZ] (Generate Active TPT Entry)

## ***** 1F 18 XX XX XX XX XX XX XX,"UNKNOWN"; *****

## ***** 1F 19 XX XX XX XX XX XX XX,"UNKNOWN"; *****

## [1F 1A XX XX YY] (Generate Floating Sprite near TPT Entry)

## [1F 1B XX XX] (Delete Floating Sprite - TPT)

## [1F 1C XX YY] (Generate Floating Sprite near Character)

## [1F 1D XX] (Delete Floating Sprite - Character)

## [1F 1E XX XX YY] (Delete TPT Entry from Screen)

## [1F 1F XX XX YY] (Delete Generated Sprite from Screen)

## [1F 20 XX YY] (Trigger PSI-style Teleport)

## [1F 21 XX] (Teleport to Preset Coordinates)

## [1F 23 XX XX] (Trigger Battle Scene)

## [1F 30] (Set Normal Font)

## [1F 31] (Set Mr. Saturn Font)

## [1F 41 XX] (Trigger Special Event)

## [1F 50] (Disable Controller Input)

## [1F 51] (Enable Controller Input)

## [1F 52 XX] (Generate Number Selector)

## ***** 1F 60 XX,"UNKNOWN"; *****

## [1F 61] (Movement Code Trigger)

## ***** 1F 62 XX,"UNKNOWN"; *****

## [1F 63 XX XX XX XX] (Screen-Reload Pointer)

## [1F 64] (Purge All NPCs)

## [1F 65] (Purge First NPC)

## [1F 66 XX YY ZZ ZZ ZZ ZZ] (Activate Map Hotspot)

## [1F 67 XX] (Deactivate Map Hotspot)

## [1F 68] (Store Coordinates to Memory)

## [1F 69] (Teleport to Stored Coordinates)

## [1F 71 XX YY] (Realize PSI Power)

## [1F 81 XX YY] (Check If Character Can Use Item)

## [1F 83 XX YY] (Equip character XX with his or her YYth item)

## ***** 1F 90,"UNKNOWN"; *****

## [1F A0] (Change Direction of Current TPT Entry to Up)

## [1F A1] (Change Direction of Current TPT Entry to Down)

## ***** 1F A2,"UNKNOWN"; *****

## [1F B0] (Save the Game)

## ***** 1F C0 XX YY YY YY YY,"Multiple-entry pointer table (Reference address)"; *****

## [1F D0 XX] (Attempt to Fix Items)
 This code looks at each broken item in Jeff's inventory and determines if Jeff's IQ is sufficient to fix it. If the code comes across a fixable broken item, it removes the broken item from Jeff's inventory and replaces it with the fixed item. The number of the broken item is put into Argumentary Memory and the number of the new, fixed item in put into Working Memory. There is a XX% chance that this code will work, otherwise it will do nothing.

## [1F D1] (Return Direction of Nearby Magic Truffle)

## [1F D2 XX] (Summon Traveling Photographer)

## [1F D3 XX] (Trigger Timed Event)

## [1F E1 XX YY ZZ] (Change Map Pallet)

## [1F E4 XX XX YY] (Change Direction of Generated Sprite)

## [1F E5 XX] (Lock Player Movement)

## [1F E6 XX XX] (Delay Appearance of TPT Entry)

## ***** 1F E7 XX XX,"UNKNOWN"; *****

## ***** 1F E8 XX,"UNKNOWN"; *****

## ***** 1F E9 XX XX,"UNKNOWN"; *****

## ***** 1F EA XX XX,"UNKNOWN"; *****

## [1F EB XX YY] (Render Character Invisible)

## [1F EC XX YY] (Render Character Visible)

## ***** 1F ED,"UNKNOWN"; *****

## ***** 1F EE XX XX,"UNKNOWN"; *****

## ***** 1F EF XX XX,"UNKNOWN"; *****

## [1F F0] (Activate Bicycle)

## [1F F1 XX XX YY YY] (Give TPT Entry a New Movement Pattern)

## [1F F2 XX XX YY YY] (Give Sprite a New Movement Pattern)

## [1F F3 XX XX YY] (Generate Floating Sprite Near Generated Sprite)

## [1F F4 XX XX] (Delete Floating Sprite - Generated Sprite)