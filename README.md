# WIP Club Penguin: Game Day! Server Emulator

## Just documenting networking information for now, but I hope to make a server that integrates with something like [WaddleForever](https://github.com/nhaar/Waddle-Forever)

### I haven't actually played Game Day! all the way through so if anyone wants to help document stuff, please do!

When connecting online in Club Penguin: Game Day!, it first connects to the DWC server (`nas.nintendowifi.net`). You can use a [server emulator](https://github.com/barronwaffles/dwc_network_server_emulator) to get past this, but you will need to patch it to remove SSL. You can do this with [Wiimmfi ISO Patcher](https://wiimmfi.de/patcher/iso) and `wit cp . "Disney Club Penguin - Game Day!.iso" --update --psel=data --http -vv`.

After that, you can type in a username and password and press "Ok", and it makes a POST request to `https://console.clubpenguin.com/submit.php`.

The POST request looks like this when logging in:
```
POST /submit.php HTTP/1.1
Host: console.clubpenguin.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 723

HTML Form URL Encoded: application/x-www-form-urlencoded
    Form item: "ProductID" = "2"
    Form item: "SessionID" = "0017abe72155"
    Form item: "SVLToken" = "NDS3Ypw3oP8NfYvqD2rao1yjIJueGKTd5VWnLb5LUTwTLQOD79DJRdjDi7gn0CKcC1vPLDY1CKB7dO3eaOJ"
    Form item: "Mode" = "live"
    Form item: "RequestID" = "10"
    Form item: "Username" = "AzureDiamond"
    Form item: "Password" = "hunter2"
    Form item: "ItemData" = "0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0"
    Form item: "ProgressionData" = "0_0_0_0"
    Form item: "HiScoreData" = "0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0"
    Form item: "AchievementData" = "0_0_0_0_0_0_0_0_0_0_0_0_0_0"
```
And this when uploading coins:
```
POST /submit.php HTTP/1.1
Host: console.clubpenguin.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 193

HTML Form URL Encoded: application/x-www-form-urlencoded
    Form item: "ProductID" = "2"
    Form item: "SessionID" = "0017abe72155"
    Form item: "SVLToken" = "NDSeErAW90KvoylZBcP6stOK4NXhWUGF3aP65Sp6tHv8u3ZLfe7V1vUj2aOe8E7pixYMl8CHrv0udy3m6Fd"
    Form item: "Mode" = "live"
    Form item: "RequestID" = "1"
    Form item: "Username" = "AzureDiamond"
    Form item: "Password" = "hunter2"
    Form item: "Amount" = "50"
```

`ProductID`: Seems to always be 2

`SessionID`: The MAC address of the Wii

`SVLToken`: The `servicetoken` returned by the `NasServer`

`Mode`: Seems to always be `"live"`

`RequestID`: Is `10` when logging in, and `1` when uploading coins.

`Username`: The players username, in plaintext

`Password`: The password, in plaintext

`ItemData`: The items got. A `1` indicates the item has been obtained, and a `0` indicates it has not. Joined with underscores in-between, it is always 38 numbers:

	00 = Red Mohawk
	01 = Blue Ball Cap
	02 = Green Propeller Cap
	03 = Gold Viking Helmet

	04 = Red Face Paint
	05 = Blue Face Paint
	06 = Green Face Paint
	07 = Yellow Face Paint

	08 = Gold Medal
	09 = Silver Whistle

	10 = Red Hockey Jersey
	11 = Blue Hockey Jersey
	12 = Green Hockey Jersey
	13 = Yellow Hockey Jersey

	14 = Red Cheerleader Outfit
	15 = Blue Cheerleader Outfit
	16 = Green Cheerleader Outfit
	17 = Yellow Cheerleader Outfit

	18 = Referee Jersey

	19 = White Pompoms

	20 = Red Sneakers
	21 = Blue Sneakers
	22 = Green Sneakers
	23 = Yellow Sneakers

	24 = Color blue item
	25 = Color green item
	26 = Color pink item
	27 = Color black item
	28 = Color yellow item
	29 = Color purple item
	30 = Color brown item
	31 = Color peach item
	32 = Color red item
	33 = Color orange item
	34 = Color dark green item
	35 = Color light blue item
	36 = Color lime item
	37 = Color aqua item

`ProgressionData`: Not sure yet

`HiScoreData`: The players high score in each game, as an int, joined by underscores. It is always 69 numbers:

	00 = Feed A Puffle (Free for All) (Easy)
	01 = Feed A Puffle (Free for All) (Medium)
	02 = Feed A Puffle (Free for All) (Hard)
	03 = Feed A Puffle (2 vs. 2) (Easy)
	04 = Feed A Puffle (2 vs. 2) (Medium)
	05 = Feed A Puffle (2 vs. 2) (Hard)
	06 = Puffle Paddle (Free for All) (Easy)
	07 = Puffle Paddle (Free for All) (Medium)
	08 = Puffle Paddle (Free for All) (Hard)
	09 = Puffle Paddle (2 vs. 2) (Easy)
	10 = Puffle Paddle (2 vs. 2) (Medium)
	11 = Puffle Paddle (2 vs. 2) (Hard)
	12 = Unknown
	13 = Unknown
	14 = Unknown
	15 = Unknown
	16 = Unknown
	17 = Unknown
	18 = Unknown
	19 = Unknown
	20 = Unknown
	21 = Unknown
	22 = Unknown
	23 = Unknown
	24 = Unknown
	25 = Unknown
	26 = Unknown
	27 = Unknown
	28 = Unknown
	29 = Unknown
	30 = Bean Balance (Free for All) (Easy)
	31 = Bean Balance (Free for All) (Medium)
	32 = Bean Balance (Free for All) (Hard)
	33 = Bean Balance (2 vs. 2) (Easy)
	34 = Bean Balance (2 vs. 2) (Medium)
	35 = Bean Balance (2 vs. 2) (Hard)
	36 = Rollin' Riot (Free for All) (Easy)
	37 = Rollin' Riot (Free for All) (Medium)
	38 = Rollin' Riot (Free for All) (Hard)
	39 = Rollin' Riot (2 vs. 2) (Easy)
	40 = Rollin' Riot (2 vs. 2) (Medium)
	41 = Rollin' Riot (2 vs. 2) (Hard)
	42 = Unknown
	43 = Unknown
	44 = Unknown
	45 = Unknown
	46 = Unknown
	47 = Unknown
	48 = Unknown
	49 = Unknown
	50 = Unknown
	51 = Unknown
	52 = Unknown
	53 = Unknown
	54 = Goal! (Free for All) (Easy)
	55 = Goal! (Free for All) (Medium)
	56 = Goal! (Free for All) (Hard)
	57 = Goal! (2 vs. 2) (Easy)
	58 = Goal! (2 vs. 2) (Medium)
	59 = Goal! (2 vs. 2) (Hard)
	60 = Unknown
	61 = Unknown
	62 = Unknown
	63 = Unknown
	64 = Unknown
	65 = Unknown
	66 = Unknown
	67 = Unknown
	68 = Unknown

`AchievementData`: Presumably related to stamps.

`Amount`: The amount of coins to transfer. Seems similar to the EPF server uploading coins function, which also used `Amount`. A recreation server for that is available at https://github.com/rocketprogrammer/epf-server

You can patch out the SSL requirement by first extracting the disk using Dolphin (Properties -> Filesystem -> Extract Entire Disc...), then patching `DATA/files/penguin_maker.rso` with [`nossl_patch_arm9.c`](https://github.com/barronwaffles/dwc_network_server_emulator/blob/master/tools/nossl_patch_arm9.c), then running `DATA/sys/main.dol`.

It expects 59 ints back, joined by underscores. Examples response:

```
HTTP/1.0 200 OK
Server: Nintendo Wii (http)
Date: Tue, 18 Nov 2025 06:45:11 GMT
Content-type: text/plain
NODE: wifiappe1
Content-Length: 119

Line-based text data: text/plain (1 lines)
    1_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_0_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1_1
```

The values are as follows:

0 = Body color:

	1 = Blue
	2 = Green
	3 = Pink
	4 = Black
	5 = Yellow
	6 = Purple
	7 = Brown
	8 = Peach
	9 = Red
	10 = Orange
	11 = Dark green
	12 = Light blue
	13 = Lime
	14 = Aqua

	Auto gives the color item too

Values 01 - 38 are items:

	01 = Red Mohawk
	02 = Blue Ball Cap
	03 = Green Propeller Cap
	04 = Gold Viking Helmet

	05 = Red Face Paint
	06 = Blue Face Paint
	07 = Green Face Paint
	08 = Yellow Face Paint

	09 = Gold Medal
	10 = Silver Whistle

	11 = Red Hockey Jersey
	12 = Blue Hockey Jersey
	13 = Green Hockey Jersey
	14 = Yellow Hockey Jersey

	15 = Red Cheerleader Outfit
	16 = Blue Cheerleader Outfit
	17 = Green Cheerleader Outfit
	18 = Yellow Cheerleader Outfit

	19 = Referee Jersey

	20 = White Pompoms

	21 = Red Sneakers
	22 = Blue Sneakers
	23 = Green Sneakers
	24 = Yellow Sneakers

	25 = Color blue item
	26 = Color green item
	27 = Color pink item
	28 = Color black item
	29 = Color yellow item
	30 = Color purple item
	31 = Color brown item
	32 = Color peach item
	33 = Color red item
	34 = Color orange item
	35 = Color dark green item
	36 = Color light blue item
	37 = Color lime item
	38 = Color aqua item

39 - 58 = Unused?
- Setting these all to 1, then resyncing doesn't change the submit post request, but setting them all to one does produce a different save file saved onto the Wiimote then one that's synced with all `0`'s, but I'm not sure if thats related.
