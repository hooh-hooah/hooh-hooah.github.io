In 0.7.0 version of the Modding Tool, I introduced the new way to build the mod.

You still can build the mod like before, but I recommend you to also try the new method.

1. **Using `hooh Tool Window`**
![](imgs/mod_00.png)

Drag and drop your custom mod XML file into the mod builder's target window.

After setting the build target, check if the output path is where you desire to put your custom zipmod archive.

If everything is okay, validate your XML file if you didn't make any mistake inside the XML file.

Unless a mod packer cannot find an asset or has some issue while resolving the Asset Bundle's path, it says nothing.

Then you're good to go. Press the big green button and to build the mod.

2. **Using new SXML Window.**

![](imgs/070_mod_01.png)

Just click the mod.sxml file, then you will see the menu inside the Inspector window.

Everything is pretty straight forward. Just click `Build Mod`.

The `Build Mod` turn into red color when something went wrong. Then scroll down the inspector window and find the `Issues` section.

You'll be able to find what's wrong with the file.
