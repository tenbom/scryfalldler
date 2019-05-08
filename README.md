# Scryfalldler

A tool to quickly download Magic: The Gathering card images.   
* Download by...   
  * a list in a text file
  * all cards in a set
  * spoilers on the [official Wizards url](https://magic.wizards.com/en/products/warofthespark/cards)
* Of Size... 
  * 6 options from the [Scryfall site](http://scryfall.com) (20%, 65%, 90%, or 100% size, 100% size Art-Crop, 100% size Border-Crop)
  * 1 option from [Wizard's Gatherer](https://gatherer.wizards.com/Pages/Default.aspx) (35% size)

## Requirements
1. __PhP version 5.3 or higher, with curl extensions enabled__    
 * _Installing on Windows_   
   Download the latest PhP version from the [windows PhP website](https://windows.php.net/).    
   * Copy the file php.ini-development in C:\PHP7\ and rename it to php.ini   
   * Open C:\PHP7\php.ini in a text editor and make the fallowing changes   
   *   uncomment `; extension_dir = "ext"` to `extension_dir = "ext"`
   *   uncomment `;extension=curl` to `extension=curl`   
   
   Now we must add PhP to the Windows 10 system path enviornmental variables
   * Control Panel->System and Security->System->Advanced system settings->Enviornmental Variables
   * Under `System Variables` double click on `Path`
   * Select `New` and type `C:\PHP7`
   * In a command prompt type `php -v` to ensure it is working
 
2. __The Scryfalldler client__   
 * _Installing on Windows_   
    Download the Client from GitHub   
    * Press the green `Clone or Download` button in the upper right, fallowed by `Download Zip`   
    * Extract the archive to your desired location
  
## Usage
To run Scryfaddler, you must have a command prompt opened to the directory scryfaddler is in.

The base command that all scryfaddler processes are run off of is    
   `php scryfaddler`   
From there you can add arguments, declaring what cards and how to download them.
   
You can add _at most 1_ argument from this table.

| Arguments, cards to download   | Notes |
| ------------------------------| ----- |
| `-file <fileName.txt>` | Uses a file of name `<filename.txt>` as the list of cards to download. <br /> The text file must fallow the form outlined below in __Text File Format__.   <br /> By default downloads and compresses the images into a zip file, <br />  named `FILE`,  and places it in the same directory as scryfalldler|
| `-set <setAcronym>` | Downloads all cards from a set, `<setAcronym>` can be found in [this table](https://mtg.gamepedia.com/Template:List_of_Magic_sets)    <br /> By default downloads and compresses the images into a zip file, <br />  named `<setAcronym>`,  and places it in the same directory as scryfalldler|
| `-url <url location>` | Only for wizards official spoilers and recent releases.  <br /> `<url location>` should be of the form https://magic.wizards.com/en/products/dominaria/cards  <br /> By default downloads and compresses the images into a zip file, <br />  named  `WZR`, and places it in the same directory as scryfalldler|
| `-gatherer <set name>` | __Broken.__ Downloads all cards from a given set, <set name> should be of the form `"Future Sight"`  <br /> By default downloads and compresses the images into a zip file, <br />  named  `???`, and places it in the same directory as scryfalldler |
 
 You can add _any number_ of arguments from this table.

|        Arguments, additional   | Notes |
| ------------------------------| ----- |
| `-size <sizeKeyword>` | Declare the size of image to download, default is `large`. <br />  The valid values for `<sizeKeyword>` are: `small` for 20% size, <br /> `medium` for 65% size, `large` for 90% size, `png` for 100% size, <br /> `art_crop` for 100% size art only, `border_crop` for 100% size no-art, <br /> and `gatherer` for 35% size |
| `-folder <folder>` | Downloads the images into a zip file of default name `1` <br /> and then places it in a folder named `<folder>` within the scryfalldler directory|
| `-force <zipName>` | Forces the zip file name to be `<zipName>` |
| `-ext <imageExtension>` | Force the image extension to `<imageExtionsion>` <br /> Default is `jpg` |
| `-proxy <proxySite>` | Downloads the images through a proxy, <br /> `<proxySite>` should be of the form: http://proxy:port |
| `-no-check` | Do not download the latest version of scryfalldler from GitHub on this execution |

These are stand-alone arguments

|        Arguments, other | Notes |
| ------------------------------| ----- |
| ` `| No argument returns a list of all the possible arguments and a brief description |
| `-help`| Returns a list of all the possible arguments and a brief description |
| `-test`| Returns whether scryfalldler can successfully connect to Scyfall |
| `-version`| Displays the version of scryfalldler you are running |
| `-list`| From Scryfall, lists all available sets for download, their acronyms, and card count.  <br />  The output is fallowed by the option to enter a set acronym to download the entire set at the default `large` image size. |

#### Examples

`php scryfalldler`   
`php scryfalldler -file myDeck.txt`   
`php scryfalldler -file myDeck.txt -size png -folder decks/deck1/archive -force Deck1Archive`   
`php scryfalldler -set C17`   
`php scryfalldler -set C17 -size art_crop -force C17ArtCrop -ext png`   
`php scryfalldler -url https://magic.wizards.com/en/products/dominaria/cards`   
`php scryfalldler -url https://magic.wizards.com/en/products/dominaria/cards -folder cards/dominaria`   


### Text File Format
Text files used to declare the cards to download should have each line in the form...   
* `<amount> <card name>`   
   For Example   
   `1 Rhystic Study`  
   `2 Faithless Looting`   
   `1 Gitaxian Probe` 
 
There are optional parameters to add after the name, declaring which set to get the image from, and for cards with multiple images in a set (like lands) which collector number to use.
* `<amount> <card name>|<setAcronym>#<collectorNumber>`   
   For Example   
   `1 Island|GK2#132` 

Scryfalldler will ignore any entries of an already declared `<card name>`.  
* For Example     
    `1 Island|GK2#132`    
    `1 Island|GK2#131`   
    Scryfalldler will ignore the second entry for Island.   
  
## Changelog

* **1.4.1** (15/06/18):
  * Small fix for cards with foil versions causing bad exporting.
* **1.4** (27/04/18):
  * Added "-g" option for download from Wizards Gatherer without taking any info from Scryfall. Doing it searching by set name.
  * Added "-u" option for download from Wizards official set site card page. Empty cards comes from Gatherer. Also tokens, but "unnamed". Not works as good as Scryfall, needs some post-renaming, but usefull in early launched sets.
* **1.3.3** (02/04/18):
  * Configured error reporting for avoid showing in output high number of warnings and so on. Now only important errors will be shown.
* **1.3.2** (13/03/18):
  * Improved the new file list option. Now you could specify the set you want for the card edition with "card|set" (i.e: Opt|INV) (useful for lands also you can use collector number with #, i.e. Forest|bfz#273). Also, can read the XMage .dck format.
* **1.3.1** (12/03/18):
  * Fixed error saving cards with " in the name. Until now, these cards doesn't download.
  * Corrected version naming.
* **1.3** (12/03/18):
  * Added optional feature for download card images from a file list (deck file, for example).
* **1.2** (12/03/18):
  * Fixed error saving split cards (the cards with "//"). Until now, these cards doesn't download.
  * Fixed error saving cards with ":" and " * " in name. Replaced by blank, like XMage does. As strange chars in some systems, until now it caused that images with that doesn't download.
  * Added option (-x, -ext) for force the image extension for the cards.
* **1.1** (09/02/18):
  * Fixed a relevant bug with basic land naming.
* **1.0** (10/01/18):
  * Initial release.

## Help
Command `php scryfalldler -h` shows:
```yaml
Usage:
  php scryfalldler [arguments] 

Arguments: 
  All arguments also accept "--". 
    -h, -help		Shows help (this info). 
    -v, -version	Show version from your downloaded script.
    -l, -list		List avaiable sets for download with codes from Scryfall. 
    -s, -set		Launch download cards operation from the specified set from the site. 
    -z, -size		The size for downloaded images ("image_uris" from Scryfall API).
                        Special value "gatherer" downloads Wizards Gatherer image if avaiable. 
                         Default value: large. 
                         Possible values: small, normal, large, png, art_crop, border_crop, gatherer.  
    -u, -url		Download a set from the Wizards official site URL.
                        Usefull for new spoilers and for tokens pages. 
    -g, -gatherer	Download a set from Wizards Gatherer searched by name. 
    -f, -folder		Destination folder for the zip file (not tested with relative ones). 
    -r, -force		Force zip and inside folder name (for tokens download, for example).
    -d, -debug		With this, script only show messages but not download or create folder. 
    -t, -test		Test your connection with Scryfall. 
    -p, -proxy		Set proxy for connections (format http://proxy:port).
    -n, -no-check	By default, script check for updates each execution (requesting to GitHub).
                        A message will warn you on new versions. You can avoid it with this option.
    -x, -ext		Extension for card images. Instead .jpg (default value) that XMage uses,
                        you could force what will be used, for example "png" 
                         Default value: jpg.
    -i, -file		Optional file location, with cards-per-line for export a concrete group of
                        cards (a deck for example - allows XMage .dck format) like a set.
                        You could specify optionally the set for card edition (card|set) and collector
                        number (card|set#collnum) (for example, for lands). If not, last published
                        edition of the card will be downloaded. 

Site: 
  https://github.com/nachazo/scryfalldler 
```

<p align="center"><img src="https://i.imgur.com/I7QEYF6.gif" data-canonical-src="https://i.imgur.com/I7QEYF6.gif" width="500" /></p>
