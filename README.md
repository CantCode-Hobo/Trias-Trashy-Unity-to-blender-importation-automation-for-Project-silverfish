# Trias-Trashy-Unity-to-blender-importation-automation-for-Project-silverfish
Some code to help automate the importation areas from the game project silverfish. (THIS CODE DOES NOT IMPORT STUFF FROM UNREAL ENGINE TO BLENDER, it uses another importer to get the job done) 
I do not know how to upload stuff to github so forgive if this looks bad. Or dont I cant force you to do anything.

Also fair warning I wrote most of this amonth apart from itself so it may be slightly outdated. Should still work. 
=========================================================================================================================
WORKING FOR "Project Silverfish Alpha Demo 0.14.9"
Tria's bad Little Guide
Things you may need:
Windows 10 (The code that this guide has worked on my computer, which is windows 10, Im not sure it will work for other os's but I hope it does)
Blender 3.6.7 (7-9)(addon says 3.4 but this works)			https://download.blender.org/release/Blender3.6/
Umodel tools (This is a optional Blender addon)								https://github.com/skarndev/umodel_tools/releases/tag/v1.0.4
FModel														https://fmodel.app/download
Project SilverFish											https://store.steampowered.com/app/2941710/Project_Silverfish/ or https://siris-pendrake.itch.io/project-silverfish
The Blender addon io_import_scene_unreal_psa_psk_280.py		https://github.com/Befzz/blender3d_import_psk_psa/blob/master/addons/io_import_scene_unreal_psa_psk_280.py
(some sort of file unpacker ie 7zip) 
A "mapping file". You can get one from the discord. Needed for Fmodel. I dont know what it does but fmodel needs it to work with the unreal files.
------- ------- ------- ------- ------- ------- ------- 
I DONT KNOW WHY BUT io_import_scene_unreal_psa_psk_280 HAS TO BE REMOVED AND REINSTALLED EVERY TIME YOU CLOSE AND OPEN BLENDER, IF YOU DONT DO THIS NO MESH'S WILL BE IMPORTED.
------- ------- ------- ------- ------- ------- ------- 
Here are some Blender scripts that will automate stuff for ya.
FoliageFinder.txt			    (Finds all the trees and bush's that are not in scene and imports them.)
FindsMissingMesh.txt  		(this will find missing items like loot nodes and boxs and write them into a txt file. (will not get same stuff as FoliageFinder))
ImportsMissingMesh 			  (will read the txt file and import all files inside of it. This will import unnecessary things that you normaly dont see. I have tried to limit these with the list_ignored)
matscript.txt				      (This just removes copied materials) From https://blender.stackexchange.com/questions/75790/how-to-merge-around-300-duplicate-materials
MaterialFinder.txt			  (This imports most textures and will attempt to asign them to its proper mesh, it will not asign the nodes all that well and will stack all image nodes on top of each other.)
Clean consle.txt			    (Cleans your consle, use this if trying to debug and need a clean error log.)		 Frome https://blender.stackexchange.com/questions/190273/can-you-clear-the-system-console-without-restarting-blender
GUICreator??? NOt TODO
WeightmaptoGround? TOO DO (this will create the ground as it is in game)

-Open Fmodel put Project SilverFish file path in directory. Chose "GAME_UE5_4" for UE version.
-Then open settings up and add in the "mapping file path" (you may have to select "local mapping file" then you can add in the path)
-Click on silverfish-windows.Utoc (the file with a few GB)
-Rightclick on SilverFish and select "Save Folder's Packeages Models" 			(this will give the models as .psk files or pskx. These next three steps may take a moment or 30 have patience) 
-Rightclick on SilverFish and select "Save Folder's Packeages Properties"		(this will give json files, the scripts need these to find materials and models for importation)
-If you plan on using one of the maps find the map you want and export as json 	(you will have to add in the filepath manually in my code)

========IF YOU USE umodel_tools USE THIS OPTIONAL PART OF THE GUIDE========
-In blender, go to addons and find: "umodel_tools.zip" install It. (if you wish to have most of the mesh's imported without my code)
-Enabled it. Click the little arrow to the left to open up the info about the addon. 
-Under "Game profiles" click the + And name in "Project SilverFish" 
-In UModel export Directory select the file were Fmodel exported the files. Should looks something like this "xxx\FModel\Output\Exports\"
-In Asset Directory Select a file were you want your imported files to be put. (this doesnt matter all that much, just put it out of the way.)
-Import your scene, Click F3 and type "Make local" then you want to go to object, relations, make local, all.
======== This umodel_tools is optional, its just a extra step that may help you.========

-Now go to addons and import the addon: ""io_import_scene_unreal_psa_psk_280.py"" Its used to import the psk files and by the automation scripts. (If you have it installed already remove it and reinstall it)

===FoliageFinder=====================================================================
-Go to scripting tab, put in "FoliageFinder.json" put in the file path to the map in the file_path and the file path to the mesh's in the mesh_dir.
-This is my map file path 												"file_path = r"D:\Users\Admin\Downloads\Character_Default\CultBargain\Scene\txt\StoneMaze_P.txt""
-This is my file path that leads to the meshes that will be imported. 	"mesh_dir = r"D:\Users\Admin\Downloads\Character_Default\FModel\Output\Exports\Game\Assets\StaticMeshes\Mesh_Environment""
-Hit play in the scripting area and the Foliage should be imported and placed in the right location, rotaion and scale.
-IF it is leaving meshes at spawn: make sure there is no mesh at spawn in a 1,1,1 area. The code imports everything to spawn the moves it, so haveing a mesh in spawn could cause issues. (Ive done what I can to correct this)
-If the mesh's are not being imported, remove the "io_import_scene_unreal_psa_psk_280" script completely and reinstall it into blender, try again. If that does not work, close and reopen blender and redo the guide, if it still doenst work. Pray, I dont know how to write code, so it will be between you and god from this point.

===FindsMissingMesh=====================================================================
-Now assuming the plants were imported and placed nicely, you will need to use the "FindsMissingMesh" code now. 
-You will need to add in file paths again into this code. there will be 3 file paths you need to add. 
-The first is the path to the map you are trying to import to blender. Here is what my path looks like 	:	"file_path = r"D:/Users/Admin/Downloads/Character_Default/CultBargain/Scene/txt/StoneMaze_P.txt""
-Second is were it finds the files it will need to add to the scene. 								 	:	"base_path = "D:/Users/Admin/Downloads/Character_Default/FModel/Output/Exports/SilverFish/Content/""
-Third is the file were it will write all the missing meshes and the locations they need to be in.		:	"log_file_path = r"D:/Users/Admin/Downloads/Character_Default/Code/wip/Tandem/Tandem.txt""
-Hit play in the scripting area and the missing info from the scene should be put into the Tandem.txt file. 
-If there is any issues I have set up a large amount of flags that will give and hide info for you. I honestly dont remeber much about this code, but it should work, it works for me.

-I may have set up a bunch of files that hold the missing files in it already, if you followed the optional "umodel_tools" part these will be repetitive. 

===ImportsMissingMesh=====================================================================
-Now that you have a full tandem.txt put the ImportsMissingMesh code into blender. 

-There will be three file paths again, its important that they end like my examples.
-First is the link to the txt file. 								: txt_file_path = r"D:\Users\Admin\Downloads\Character_Default\Code\wip\Tandem\Tandem.txt"
-Second links to the pskx files, these hold what we want to import. 	: base_path_Pskx = r"D:/Users/Admin/Downloads/Character_Default/FModel/Output/Exports/Game/Assets"
-Third links to the json files, these link to the pskx files.		: base_path_Json = r"D:/Users/Admin/Downloads/Character_Default/FModel/Output/Exports/SilverFish/Content"
-Once you have all the paths assigned you have 2 options for importing. When you import by default every mesh imported will be assigned to a cube, the cube is hidden, but its there for ease of placement.
-If you dont want the cube's then set "Remove_cube_parent" to "T" this will remove the cubes but keep the location of the meshes. 
-Its done like this so if you want to move a item and all the stuff that goes with it, it will be easier. 
-Make sure to select the "scene Collection" Or have the Collection_Auto_Select set to "T"

-If there is any files you dont want imported then add them to the "List_Ignored" this list is applied inside when reading the txt file and inside the json's.
-I tried to set up a bunch of # info things. This is so that if you want to resue the code for somthing other then what I set up you will have a easier time. 
-Once you have all the paths set up, and all the flags set to your choice, if you want to see the files as they import make sure to have the system consle open. 
-Hit play in the blender script area, and if your importing a decent amount of stuff blender will freeze for a min or two. You can watch as its doing this in the consle window.
-Dont worry if the consle window is frozen at start, its just getting set up and ready to load the stuff into the consle log. 
-If you see this in the log, dont worry its due to the "io_import_scene_unreal_psa_psk_280" 
"
-----------------------------------------------
---------EXECUTING PSK PYTHON IMPORTER---------
-----------------------------------------------
Unknown chunk:  ACTRHEAD
"
=====THERE MAY BE WEIRDLY PLACED MESH'S AROUND 0,0,0. CHECK THEIR CUBES, IF THE CUBES PLACEMENT IS THE DEFAULT 0,0,0 THEN SOMTHING IS WRONG. If not, then idk. Im testing on Ruinsfeilds_p right now and there is things not in game at 0,0,0 but they do show up in in the files. For this youll have to decide what you wish to keep. Im not sure if this is stuff left over in the files that was disabled or if it has a second placement it should be moved to after. In Ruinsfeilds_p there are 3 of a set boat, but the files ref 6 of them. the other 3 are what will be imported close to the 0,0,0. This leads me to believe that the mesh in that location is leftovers. I will not be adding paramiters to skip such mesh's in case that there is a proper mesh in there. =======================
-At this point you can use the "matscript" code that will take all the copys of the same materials and turn it into one material.
####Set up tryplainar, be simple, have it set to just take color and set scalar to 7.5. and rotation to be 90. OR SET UP A TRIPLANAR THAT EFFECTS WHICH EACH FACE DEPENDING ON WHICH WAY ITS IS FACING.
===MaterialFinder=====================================================================
-Now get the MaterialFinder code into blender.
-There will be a directory you need to add to this code.
-Here is what mine looks like "search_directory = r"D:\Users\Admin\Downloads\Character_Default\FModel\Output\Exports\SilverFish""
-You want to have "Exports\SilverFish" at the end, this is so the code can search from there on and find the material json's and seach those for the images to import.
-This will import all the textures into the materials and attempt to asign them properly. It will not work well at assigning the nodes.
-The textures will be over top of each other so you will have to drag them off of eachother.
-You will also need to manually asign the textures to the right nodes, I may try to have the code do that, but prob not.

Once all this is done there you go, you have most of a Project SilverFish level in blender, this may work for other games, but its all dependant on how they set their files up.
I do plan on setting up a ground script that will get the proper ground imported, but not in this build.
This code does not import animations, just meshes and armitures, and materials. 
I also plan on makeing a script to add the gui into blender. (the hud)

IF AT ANY POINT YOU WISH TO CLEAR THE LOG USE "Clean consle.txt" SCRIPT. 
