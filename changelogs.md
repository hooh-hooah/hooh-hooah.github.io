# Modding Tool Changelog

## 0.7.0

### 2021-07-26

![](https://github.com/IL-Modding-Tool/Public-Resources/blob/master/edgy-breaking-changes.gif?raw=true)

#### :warning: **BREAKING CHANGES** :warning:

* ***Overall structure of the Modding Tool has changed!***

  * How to migrate?

    **You can't. Please download new version as new project.**

  * Why?

    To prevent things like this happen again, I've separated most of the editor codes into multiple Unity Packages. So we can update the codes separately later without screwing up everything again.

* ***Modding Tool .xml file's Extension has changed!***
  ![](https://github.com/IL-Modding-Tool/Public-Resources/blob/master/xml-to-sxml.png?raw=true)

  * Why?

    **Because Unity won't let me to add Scripted Importer for .xml files.** so I had no choice.

  * How to migrate?

    Fortunately, the process is easy.
    **Just change *.xml files to *.sxml file.**

#### General Changes

* Improved mod validation
  * **Now the modding tool will automatically validate sxml files.** Which means you don't have to constantly press check to see if the mod will be compiled or not. Neat, Isn't it?
* Changed `SideloaderMod` class as `SerializedObject`. This object will be automatically imported if the mod.xml file is sxml. (Related with the breaking changes.)
* Fixed most of the Package Issues by fixing everything as separate fork. Since the engine version is "Fixed", We really don't have to worry about the future supports. (Actually, I will take care of it mostly.)
* Fixed some Issues related with List Generation
  * Fixed that few male clothing list generation not working correctly.
  * Fixed that few female clothing list generation not working correctly.
* Removed various unused code.
* Now you can build the mod with the sxml files.
* Countless bug fixes.

## 0.6.0

### 2021-03-01

- Fixed Issues with Mod Packers
  - Added Mod XML Bundle Target Validation and Exception is now more helpful
  - Trimming main manifest data elements to prevent dumb auto formatting
  - Additional Fool-proof Asset Building Code
  - Fixed HPointList is not saving changed data sometimes
  - Fixed animation list generation issue
- Added More Preview Features
  - Heels Preview Components
  - Added Body Preset Types & Fixed Test Animations
- New Modding Features
  - Skinned Accessory Support
- New Assets
  - Added RMA Shader Set (Unreal Format), Modularized AIHS Shader Parameters
  - Reorganized base files, Added Male Clothing base
- Basic In-Game Modding Supports
  - Added helper method for the AI Furniture and Optimized the FindAssist
  - Added AI Main-game map support.
  - Reworked Heels workflow - Added Heels Preview and New Heels Example file
- Added Few Developer Features
  - Added ADVCommandParser/Dumper
  - Added Command Enum
- Fixed ItemComponent Animation Initialization
- Now Thumbnail Generator add overlay/underlay on existing image.

## 0.5.0

### 2020-09-27

-   Changed archive method: Removed `7z` dependency → changed to `SharpZipLib`.
-   New modding tool document is now available in http://hooh-hooah.github.io/
-   Added Studio Item Thumbnail Generator
-   Added and Integrated Image Compression Library with Item Thumbnail Generator
-   Reworked and Improved Mod Packer.
    -   All Mod Packing/Validation has moved to the `SideloaderMod` class.
    -   Legacy Mod XML is compatible. It is recommended to do little work to legacy mods.
    -   Improved Mod XML and Asset Validation. Now it can detect resolved AssetBundles when you make new Mod Packer Class
    -   Improved Automation and Quality of Life Changes.
    -   Added Material Editor Support and Automation
    -   Added Dependency Loader Automation
-   XML Helper has moved to the part of `SideloaderMod` class
-   Changed mod packer's archive option to `Stored`. The Loading Performance has been greatly improved compared to the last method.
-   Optimized the Modding Tool's GUI Performance.
    -   Now, AI/HS2 Inspectors Repaint when the content has changed.
    -   Changed all button events to delegate to prevent GUI Stack Failure
-   Added various ILLUSION shader preview
-   Added Map Info automatic initialization
-   Added Various Accessory Automatic initialization
-   Added Map Event Automatic Generation and Packing (No interaction required.)
-   Improved Clothing Testing Methods. Now it's not dependent to the scene. Now it's a just prefab
-   Added ADV Dump Function (For Advanced Developers)
-   Removed Legacy Mod Packer Code
-   Reduced Warning Error Logs
-   Added Developer Utility Extensions
-   Added Hanmen's new Shader Replica

## 0.4.2

### 2020-07-23

-   Added Dependency Loader Support
-   Added HS2 Map Support
-   Added HS2 Map Component Inspector
-   Changed ModPacker into modular structure.
-   Overall performance improvements.
-   Added Hanmen's ILLUSION Clothing Replica Shaders

## 0.4.1 - beta

### 2020-07-05

-   Reworked ColorableObjects shader. It's now can be used for Accessory and Studio Items.
-   Added Thumbnail Generator.
-   Reworked XML Helper, and it's less confusing now.
-   Added Accessory Component Custom Inspector.
-   Fixed ItemComponents are being weird.
-   Reworked Mod Files Validator.

## 0.4.0 - alpha

### 2020-07-02

-   Improved Asset Building w/ Validating Assets in .xml files.
-   Added SetName, SetNameSequence for Unity Macro.
-   Now Wrap Object with Parent respects target's parent.
-   Fixed Foldout Style Fuckery.
-   Updated Hair Preview - Now you can preview your hair and textures in Editor.
-   Added BaseBoneNavigator.

### 2020-06-28

-   Updated Engine's Version to HS2's Unity Engine Version (2018.4.11f)
    -   **Do not upgrade version with unity's feature. re-import everything, so your stuffs are not going to break.**
-   Added Preview Alpha
    -   Now you can preview your right without launching the game with binding helper.
    -   Binding helper will be combined with skin preview components.
    -   In the end it will be all-in-one preview system in one or two months.
-   Re-designed Component Inspector
    -   Now it's easier to manage your mod's component.
-   Reduced Human Error with more automated tasks
-   Moved Mod Component Initializing into Transform Component Inspector.
-   Added and refined the examples
-   Added Animation Example (Warning, it's really rough atm)
-   Added Bunch of Macros and UI Improvements
-   Initial Support for HS2 In-game Map Components - It works technically.
-   Added Anisotropic shader alpha.
-   Removed unused and big files from the project.

#### What's about to come?

-   **Complete Animation Workflow**
-   **Migrating mod configuration into ScriptableObject from mod.xml**
-   **Better UI**
-   **All-in-one Editor Preview System that almost identical to ILLUSION's Loading Code.**

## 0.3.0

### 2020-02-29

-   Changed Clothing Initializer's Class Target to SkinnedMeshRenderer from MeshRenderer
-   Added new targets for Clothing Targets
    -   TODO: Make it modular to make users can customize targets with regex support.
-   Added Automatic Dynamic Bone Initializer
    -   WARNING: It will remove existing dynamic bones of the object automatically.
    -   TODO: Make it only remove automatically added dynamic bones.
-   Resolved CSVBuilder's Same-named Asset Navigation Issue.
-   Added FK Bone List support for CSVBuilder.
-   Reformed the UI
-   Added Animation List Generator. It will fill out ItemComponent's Animation List based on Object's Animation Controllers.
-   Added ModPack Testing method.
-   Changed Temporary Folder location for zipping mod.
-   WIP: XML Inspector...
-   Merged Light Probe Intensity Adjustment into hooh's Tool Window.
-   Changed Material generation for hair mods.
-   Added XML Touch Tool for mod.xml Manipulation.
-   Changed few shader's name.
-   Added few shaders.
    -   Colorable Objects

## 0.2.1

### 2020-01-30

-   Fixed Hair Render Object setup fuckery
-   Now SB3U ScriptBuilder will include shader value adjustment.
-   Added Deploy Mode for ModPacker (good for just checking things and debugging)
-   Now you can add bone information for your items.
-   Now List Generator automatically put list items in mod.xml

## 0.2.0

### 2020-01-09

-   Moved Light Probe Intensity tool to hooh Modding Tool Window.
-   Now Foldout status will be saved after closing editor/updating code.

### 2020-01-08

**Hair mod.xml template has been changed! be aware.**

-   Changed FBXAssetProcessor: Added few name targets.
    -   "shoesmesh.fbx"
-   Added Foldout sections. Now you can fold stuffs
-   Moved Mod Scaffolding into separate class.
-   Combined error messages that you get when you try to execute modding tool function without selecting anything.
-   Moved Manifest Building into separate class.
-   Now hooh Modding Tool supports Heelz Mod. You can make heels mode with hooh Modding Tool
-   Removed Bandizip Dependency. Now you don't have to use bandizip to build mods. Good Riddance.
-   Moved Material Swapping into separate class.
-   Now texture swap is optional.
-   Now alerts you when there is no mod.xml in current Project Folder.
-   Made Probe Intensity public. It's going to be combined in same menu.
-   Added a lot of examples.

## 0.1.0

### 2020-01-07

-   Now you can build mod with ONE button.
-   Changed CSVBuilder Structure: Now it's using key-based delegates instead of category based delegates.
-   Changed FBXAssetProcessor: Added few name targets.
-   Enhanced Hair Component Initialization Tool.
    -   Now it initializes dynamic bones automatically.
    -   Now it initializes hair preview tools automatically.
-   Example: Added Skin Paint Example.
-   Added Open Folder Option when Mod Builder completes mod build.
    -   When you click Open Folder, it will open your zipmod destination folder.
-   Example: Updated hair example.
    -   Updated mod.xml comments and structure.
    -   Updated blender source files.
    -   Changed noise.png to 2x res noise texture.
-   Updated Shader Code.
    -   You can use Opaque Shader when you're making non-transparent clothes that will not tear. useful things like socks or small clothes.
    -   Now Glossiness and Metallic adjustments are working.
-   Added Changelogs.
