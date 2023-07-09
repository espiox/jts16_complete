# How to combine openFPGA cores

Before doing this, you should be familiar with the folder layout of the Analogue Pocket SD card and where core files are placed. You don't need much technical knowledge to do this, but cores have to be set up precisely or they won't boot. Make sure you back up your files first!


## Combine Core Files
First, decide which core will be your "base." Then, place the .rbf_r files from the other cores into that core's folder in Cores, making sure each one has a unique filename.

Open up the core.json file and add the new .rbf_r files to the "cores" section, giving each one a unique name and ID number.

		"cores": [
			{
				"name": "jts16",
				"id": 0,
				"filename": "jts16.rbf_r"
			},
			{
				"name": "jts16b",
				"id": 1,
				"filename": "jts16b.rbf_r"
			}
   		]


## Platform ID
To differentiate from original cores, I tend to give mine a unique platform ID/shortname - usually adding "_c" to the end of the original. You'll need to change that in the core.json file:

   			"platform_ids": [
				"jts16_c"
			],
			"shortname": "jts16_c",

You'll also need to update the platform ID/shortname in all places it appears, including folder names in Assets, Cores, and Presets:

- Assets\jts16_c\jotego.jts16_c
- Cores\jotego.jts16_c
- Presets\jotego.jts16_c
- Presets\Input\jts16_c\jotego.jts16_c
- Presets\Interact\jts16_c\jotego.jts16_c

You should also make sure you have a platform info file and image with the new ID/shortname:

- Platforms\jts16_c.json
- Platforms\\_images\jts16_c.bin


## Instance Files
Next, open up each instance (game) json file for the your secondary core(s) in Assets\jts16b\jotego.jts16b (for example) and change this section:

		"variant_select": {
			"id": 0,
			"select": false
		},

To this, making sure that the ID number matches the core it needs:

  		"core_select": {
			"id": 1,
			"select": true
		},

(You don't need to do this for games for your "base" core - if a core isn't specified, it will default to whichever core is id "0".)

Once you've done this for every game, move all the files over to the correct Assets folder for your multi-core. You also need to combine each core's Presets folders, making sure that the folders under Input and Interact match the layout of your folders in Assets.


## Notes
- This only works if the audio.json, data.json, and video.json files are the same for each core being combined. There's currently no way that I'm aware of to specify these differently for each .rbf_r file.
- If input.json and interact.json are different per-core, you'll need to set up files per-game in the Presets folder, matching the filename of the game in Assets (check the Raizing multi-core for an example of this).
