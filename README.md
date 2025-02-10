# Castlequest GRIF

Port of Castlequest to GRIF (Game Runner for Interactive Fiction) by Scott Bakker

# Port of Castlequest (1980)

By Michael S. Holtzman and Mark Kershenblatt.


## Provenance

Port based on code from (https://github.com/Quuxplusone/Castlequest)

Info below copied from the above GitHub.

> The U.S. Copyright Office has a deposit related to this game:
[TXu000091366](https://cocatalog.loc.gov/cgi-bin/Pwebrecon.cgi?Search_Arg=TXu000091366&Search_Code=REGS&CNT=10&HIST=1)
>
> On 2021-03-02, Mark Kershenblatt received 78 pages of paper copies
from the USCTO. He scanned them in and sent the scans to Arthur O'Dwyer,
in the form of two PDFs. Arthur rotated and concatenated the PDFs
into the single 78-page PDF in this repository, `castlequest.pdf`.
>
> Arthur O'Dwyer manually transcribed the PDF into the plain text
file in this repository, `castlequest.ocr.txt`.


## GRIF Port

GRIF, or Game Runner for Interactive Fiction, is a program designed to run game data files. It uses the DAGS scripting engine and the GROD data dictionary for accessing text data, and loads and saves data files in GRIF format. The source is available in GitHub at [GRIF](https://github.com/BakkerGames/GRIF). Binary executables are available in the [Releases](https://github.com/BakkerGames/GRIF/releases) section there.

All of the text, values, and scripts necessary to play the game are stored in GRIF-formatted text files (one or more). GRIF handles input and output and the parsing of commands but leaves the rest up to the game data.

This port was done by hand from the FORTRAN source files in the [Castlequest](https://github.com/Quuxplusone/Castlequest) GitHub repository. It is a faithful representation of the original with some minor exceptions:

(1) GRIF ignores case in the typed commands. The original only allowed uppercase. Only the first four characters of any word are recognized as in the original.

(2) You are allowed to save and restore at any time and saving doesn't quit. Only one save file is supported.

(3) GRIF counts an entered command as a move when the command succeeds and changes some data. It is specifically handled by calling "@incmoves" at the appropriate time in the game scripts. So LOOK, INVENTORY, SCORE, and trying something which fails are not counted as moves. In the original some of these would be, while some data-changing commands would not. The original logic wasn't duplicated during the port. For this reason the amount of lamp light and moves before sunset had to be adjusted to give them approximately the same duration.

(4) The combination lock in the original does not function correctly, due to the LOCK value not being included in `COMMON /BLOCK2/ SAVAR` (probably an oversight). This means it always fails on the first attempt and returns to locked after leaving the room. This was fixed during the port.

(5) The logic for the castle being closed and having to enter a secret word to prove you are a baron was not included. Play whenever you want.

(6) The original subtracted 1/5 point per move over 250. This was left out. Take as long as you want to finish without worrying about your score, lamp permitting.

(7) It now takes only one move to leave the Mirror Maze after the Count is gone, instead of some random number of attempts.

(8) The original has some issues with tracking which rooms have been visited, so it showed long descriptions unnecessarily. This was fixed, including the Bedroom.

(9) The sword now glows properly in the presence of the wizard.

(10) I felt sad for the hunchback and gave them a more lasting effect in the game.

(11) Any typos and spacing issues from the original were left in. The fixes are in the "Mods\corrections.grif" file.

(12) For debugging and walkthroughs, "value.norandom" was added. If "true", the wolf and gnome never appear, the Mirror Maze is more stable, and the footsteps are never heard. Make sure it is "false" or undefined for the full Castlequest experience!

(13) For debugging, "value.debug" can be set to "true". This was a command in the original and needed a password. It is not directly changeable while playing in this port but can be turned on with a modification file (see below). It displays some extra messages, disables darkness, and does a few other things.


## Running Castlequest

To run, put "grif" or "grif.exe" and "Castlequest.grif" into the same directory, then run "grif Castlequest.grif". GRIF is available as binary executables for Windows and Linux or it can be compiled from the C# .NET 8 source. See [GRIF](https://github.com/BakkerGames/GRIF) for info and the current release with binaries.

You can save an output log of the game using the "--output &lt;filename&gt;" (or "-o") parameter. It will overwrite any existing file.

You can make a text document of commands to be entered and include it with the "--input &lt;filename&gt;" (or "-i") parameter. It will run all the commands listed, and then switch back to player input. This only really works with "value.norandom" set to "true". There are two such files in the "Walkthrough" directory.

You can make modifications to the game without changing the "Castlequest.grif" file by using the "--mod &lt;filename&gt;" or "--mod &lt;directory&gt;" (or "-m") command line parameter. It will load an additional GRIF file or directory of GRIF files after the base file is loaded. It will overwrite any duplicate keys with new values. You can have as many "--mod" parameters as you want. These are great for making modifications to the game. Look in the "Mods" directory for examples.


## Acknowledgements

I would like to thank Michael S. Holtzman and Mark Kershenblatt for writing the original Castlequest! I had played it in college back in the day, and still remember the enjoyment (and frustration).

Thanks also go to Arthur O'Dwyer for his part in transcribing the U.S. Copyright Office's document into readable and runnable code and blogging about it so others would know it existed again. This port would not have been possible without his work.

While writing this port, extensive use was made of a compiled version of the original FORTRAN code. Arthur O'Dwyer's directions worked perfectly and produced a fully functional game on Windows. I also made use of his walkthrough text (and made another HappyPath walkthrough of my own). I used the original game's output and compared to the GRIF output to make sure everything matched, as much as possible.