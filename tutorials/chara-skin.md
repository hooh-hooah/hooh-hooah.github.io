# Creating Custom Skin

## Related Document

This page only contains how to put hair models into the game with adjustable attributes.

If you need more basic information like setup folders or preparing hair assets, please visit the pages below before continuing to read this document:

-   [Getting Started with the hooh's Modding Tool](getting_started.md)
-   [Setting up Folder](tutorials/gearing-up.md)

## Step

### Making Texture

First, fill the whole layer with white and go to the channel and make an alpha texture.

Here all that matters. The Alpha channel is the most important thing to make a bodypaint/partial pant texture.

![image-20200214072029064](images\image-20200214072029064.png)

### Importing Texture

After making textures, Place all textures inside of the `tattoo` folder.

![image-20200214072443472](images\image-20200214072443472.png)

It's not over yet! The textures used for the character requires some flags to work in the game properly.

When you click the texture file, the inspector will change to "Import Option".

In the "Import Option" menu, set a few

-   **Alpha Source** → Input Texture Alpha
-   **Alpha is Transparency** → NO
-   **Streaming Mip Map** → NO
-   **Generate Mip Maps** → NO
-   **Wrap Mode** → Clamp

!> I recommend not to touch any compression options if you don't know what you're doing. Some compression option will make the texture lose its transparency! You can check [the Unity Engine's Document about texture compression](https://docs.unity3d.com/Manual/class-TextureImporterOverride.html) to see what's going on.

![image-20200214072527660](images\image-20200214072527660.png)

### Understanding Skin Shader

| texture | channel | description |
| ------- | ------- | ----------- |
| a       | b       | c           |

you can find detailed information in here

-   [ILLUSION Standard](technical/illusion-system.md)
-   [ILLUSION Shader](technical/illusion-shader.md)

### Previeing your skin Texture

### Setup Mod XML File

```xml
<packer>
	...
    <bundles>
    	<!-- referencing "tattoo" folder. path is relative to the folder  where mod.xml is present -->
        <folder from="tattoo" auto-path="textures" filter=".+\.(png|tga|tif|psd)"/>
	</bundles>
	<build name="example_bodypaint">
		<list type="spaint">
			<item kind="" possess="" name="My Custom Tattoo" tex-a="TextureName" tex-g="TextureName2" thumb="ThumbnailName"/>
		</list>
	</build>
</packer>
```

!> The GUID, bundle name, build name should be **unique**, and you can only refer files in Asset Bundles in the Mod XML File.

You can check the comment inside of the XML Code section above to see what to do.

For more detailed information, you can check those documents for reference.

-   [XML File Structure](technical/xml-file.md) for general Mod File Information
-   [Auto-Path Lists](technical/autopath-list.md) for `<folder auto-path>`
-   [XML List Types](technical/category-list.md) for `<list type>` and `<item>`

### Build Mod

![](imgs/mod_00.png)

Drag and drop your custom mod XML file into the mod builder's target window.

After setting the build target, check if the output path is where you desire to put your custom zipmod archive.

If everything is okay, validate your XML file if you didn't make any mistake inside the XML file.

Unless a mod packer cannot find an asset or has some issue while resolving the Asset Bundle's path, it says nothing.

Then you're good to go. Press the big green button and to build the mod.

It depends on your mod size, but it will play a nice sound to notify the packing is done after a few seconds or minutes.

### Testing your Skin Texture

### Trouble Shooting

!> If you can't find the issue in here then check [**Trouble Shooting**](tutorials/trouble-shooting.md) page.