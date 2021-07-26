
### Initializing the Component

![](imgs/070_mod_02.png)

When you've done putting your mesh to the scene, Go to the `hooh Tool` Window.

If you cannot find the `hooh Tool` window, Press `ALT` key and check the upper menu.

After opening the `hooh Tool` Window, Navigate to the `Initialize Modding Components > Common > Clothing` Button.

Then the Modding Tool will automatically find the references in your model and initialize everything to make your model work in the game.

### Validating Component

![](imgs/com_00.png)

Just in case when you didn't set things as the Document, you can manually review the component to check if it's going to work correctly in the game.

-   **Visible Renderers**

    First, if every `Skinned Mesh Renderer` is included in "Visible Renderers", you're good to go.

-   **Texture Render Objects**

    Second, There is "Texture X Render Object" below. To explain what they're for, you need to know that ILLUSION renders a new clothing texture when you change the game's color or clothing pattern.

    For that purpose, you can assign a maximum of 3 sets of colormask and diffuse textures in the Mod XML File.

    Each "Texture Render Object" group represents for each set of diffuse and colormask in the Mod XML File.

    Mostly, A single texture for the clothing so check that every renderer listed in Texture 1 Render Object.

-   **Options**

    You can assign toggleable optional meshes for the clothing but remember that you can't toggle optional state in-game.

-   **Cloth Object Assignment**

    This section is for assigning the half-off state of the clothing. If you're making any clothing mod for Top/Bottom, Inner Top/Bottom, or Pantyhose Section, You need to assign the full state of the clothing in this section. Otherwise, It will not work properly.

    Ensure that the component is referencing Top/Bottom Clothing's Full and Half-off state meshes in the section.

-   **Cloth Colors**

    Well, as the title says, this is the color information of your clothing. If you enable each color option, you can color your outfit in the game.

    Unfortunately, you can't adjust a few sliders in the game if you're using the standard shader. To use all the game options, you must use `Clothing Shader Replica` for your clothing material.

    You can find all the included shader information inside the [Shader Information](technical/shaders.md) Document.

    You can find all the information about the included shader inside the [Shader Information](technical/shaders.md) Document.
