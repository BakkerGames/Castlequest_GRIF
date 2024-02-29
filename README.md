# Castlequest GRIF

Port of Castlequest to GRIF (Game Runner for Interactive Fiction)

# Port of Castlequest (1980)

By Michael S. Holtzman and Mark Kershenblatt.


## Provenance

Port based on code from (https://github.com/Quuxplusone/Castlequest)

Info below copied from above GitHub.

The U.S. Copyright Office has a deposit related to this game:
[TXu000091366](https://cocatalog.loc.gov/cgi-bin/Pwebrecon.cgi?Search_Arg=TXu000091366&Search_Code=REGS&CNT=10&HIST=1)

On 2021-03-02, Mark Kershenblatt received 78 pages of paper copies
from the USCTO. He scanned them in and sent the scans to Arthur O'Dwyer,
in the form of two PDFs. Arthur rotated and concatenated the PDFs
into the single 78-page PDF in this repository, `castlequest.pdf`.

Arthur O'Dwyer manually transcribed the PDF into the plain text
file in this repository, `castlequest.ocr.txt`. (If you find any
places where the transcription differs from the original PDF,
Arthur will pay a "bug bounty" of $5 per error! Open a pull request
on this repository or send him an email.)


## GRIF Conversion

GRIF, or Game Runner for Interactive Fiction, is an application designed to run game data files. It contains the DAGS scripting engine and the GROD data dictionary for holding and accessing text data. It also uses GRIFDataIO to load and save data files. All are available as source in GitHub. GROD and DAGS are available as separate NuGet packages.

All of the text, values, and scripts necessary to play the game are stored in JSON-formatted text files (one or more). GRIF handles the input and output and the parsing of entered commands but leaves the rest up to the game data.

This conversion was done by hand from the FORTRAN source files in the above GitHub repository. It is a faithful representation of the original with a few minor exceptions:

(1) GRIF ignores case on typed commands. The original only allowed uppercase.

(2) GRIF counts an entered command as a move when the command succeeds and changes some data. It is specifically handled by calling "@incmoves" at the appropriate time in the game scripts. So LOOK, INVENTORY, SCORE, and trying something which fails are not counted as moves. In the original some of these would be, while some data-changing commands would not. The original logic wasn't duplicated during the conversion. For this reason the amount of lamp light and moves before sunset had to be adjusted to give them approximately the same duration.

(3) The combination lock in the original does not function correctly, due to the LOCK value not being included in `COMMON /BLOCK2/ SAVAR` (probably an oversight). This means it always fails on the first attempt and returns to locked after leaving the room. This was fixed during conversion.

(4) The logic for the castle being closed and having to enter a secret word to prove you are a baron was not included. Play whenever you want.

(5) It now takes only one move to leave the Mirror Maze after the Count is gone, instead of some random number of attempts.

(6) The original has some issues with tracking which rooms have been visited, so it showed the long description unnecessarily. This was fixed, including the Bedroom.

(7) The original subtracted 1/5 point per move over 250. The new move counting logic makes this harder and less necessary, so it was left out.

(8) I felt sad for the hunchback and gave them a more lasting effect in the game.

(9) The sword doesn't glow properly in the presence of the wizard. This bug was left in but can be easily fixed. The fix is in the "corrections.grif" file.

(10) Several typos and spacing issues were left in. The fixes are in the "corrections.grif" file.

(11) I haven't extensively checked all of the failures and errors which are possible, so some of those may not function as the original.

(12) For debugging and walkthroughs, "value.norandom" was added. If "true", the wolf and gnome never appear, the Mirror Maze is more stable, and the footsteps are never heard. Make sure it is "false" for the full Castlequest experience!

(13) Also for debugging, "value.debug" can be set to "true". This was in the original and needed a password. It is not directly changeable while playing in this version but can be turned on with a modification file (see below). There are some extra messages which will be shown, and saving the game doesn't quit.


## Running Castlequest

To run, put "GRIF.exe" and "Castlequest.grif" into the same directory, then run "GRIF Castlequest" or even just "GRIF". GRIF.exe runs in Windows 64-bit and needs .NET 8 or above installed. The source can probably be compiled to run in other environments.

You can save a log of the game using the "-o/--output <filename>" parameter. It will overwrite any existing file.

You can make a text documents of commands to be entered and include it with the "-i/--input <filename>" parameter. It will run all the commands listed, and then switch back to player input. This only really works with "value.norandom" set to "true". There are two files in the "Walkthrough" directory with such lists of commands.

You can make modifications to the game without changing the "Castlequest.grif" file by using the "-m/--mod <filename/directory>" command line parameter. It will load an additional GRIF file or directory of files after the base file is loaded and overwrite any duplicate keys with new values. You can have as many "-m/--mod" parameters as you want. These are great for making tiny modifications to the game such as "value.norandom" or "value.debug", or the "corrections.grif" file. Or localization to another language...

There is a mod called "room_maps.grif" in the Mods folder. It adds an ASCII map of the room's exits at the beginning of the "@look" command. It handles changes to the rooms when new exits become available.

Finally, there is a full map of the game in "Documents/maps.txt". Best to leave that until after playing, though.


## Acknowledgements

While writing this conversion, extensive use was made of a compiled version of the original FORTRAN code. Arthur O'Dwyer's directions worked perfectly (after some GFORTRAN issues on my part) and produced a fully functional game. I also made use of his walkthrough text (and made another walkthrough of my own). I used the original game's output and compared to the GRIF output to make sure everything matched, as much as possible.

I would like to thank Michael S. Holtzman and Mark Kershenblatt for writing the original Castlequest! I had played it in college back in the day, and still remember the enjoyment (and frustration).

Thanks also go to Arthur O'Dwyer for his part in transcribing the U.S. Copyright Office's document into readable and runnable code and blogging about it so others would know it existed again. This conversion would not have been possible without his work.