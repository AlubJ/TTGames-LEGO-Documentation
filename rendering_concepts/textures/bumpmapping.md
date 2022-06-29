# Bumpmapping

A bumpmap, or bump map, is a texture used by a 3d model to simulate subtle divots or "bumps" on a UV-mapped surface without requiring additional 3d geometry. It contains information that is used by the lighting engine to determine how to light the surface as if it had height. <br/>
The most common type of bumpmap is a normal map, or normalmap, and the terms are often used interchangeably. Nu2 and nTT exclusively use normal maps, but it is important to understand other types of bumpmaps in order to understand how normal mapping works, as well as how to create a normal map from a base, diffuse, or albedo texture. 

## Types of bump maps
### Height maps
[<img align="right" src="https://raw.githubusercontent.com/AlubJ/TTGames-LEGO-Documentation/main/media/bumpdemo/nab2_bldg_brick06_height.png" width="256" alt="A height map, taken from the Battlefront II Classic official modding tools." title="A height map, taken from the Battlefront II Classic official modding tools." />](https://raw.githubusercontent.com/AlubJ/TTGames-LEGO-Documentation/main/media/bumpdemo/nab2_bldg_brick06_height.png)
The most basic form of bump map, a height map encodes the height of a surface as a greyscale image, with light areas being high, and dark areas being low. They are generally not used by 3D renderers due to their 1-dimensionality, but their simplicity makes them especially suitable for creation of other forms of bump maps by a texture artist, and it is often easy to create a height map from a diffuse or albedo texture (base textures can more difficult due to having ambient occlusion built in; see [below](#Generating Bumpmaps) for more information). <br/>
A major limitation of height maps is that they can't store the absolute depth of a surface. As such, tools that convert a height map to a normal map usually include a scale option to control the intensity of the bumpiness of the resulting texture. 

### DuDv maps
DuDv maps are a deprecated form of bump map, usually used by old DirectX 8 renderers. Encountering a DuDv map is rare, as normal maps quickly took their place, so they are more of a curiosity than anything important. DuDv maps are similar to normal maps (and normal maps can be easily converted to DuDv maps in Photoshop or GIMP), but they only use 2 color channels (red and green). 

### Normal maps
[<img align="right" src="https://raw.githubusercontent.com/AlubJ/TTGames-LEGO-Documentation/main/media/bumpdemo/nab2_bldg_brick06_directx.png" width="256" alt="A DirectX-style normal map, generated from the heightmap above." title="A DirectX-style normal map, generated from the heightmap above." />](https://raw.githubusercontent.com/AlubJ/TTGames-LEGO-Documentation/main/media/bumpdemo/nab2_bldg_brick06_directx.png)
Rather than storing the literal shape of a surface, normal maps encode bump information in tangential space. To avoid getting super-mathematical, this basically means that different colors determine how far a portion of a surface is from being flat. <br/>
A flat normal map has an RGB value of (128,128,255). The red value represents the X axis, the red value represents the Y axis, and the red value represents the Z axis. Any blue value below 128 is invalid, as it means the surface is facing backwards. 

## Normal map formats
Not all normal maps are created equal. Sometimes it is necessary to do some fiddling in an image editor to make a normal map compatible with a given game or 3D application. 

### DirectX vs OpenGL
[<img align="right" src="https://raw.githubusercontent.com/AlubJ/TTGames-LEGO-Documentation/main/media/bumpdemo/nab2_bldg_brick06_opengl.png" width="256" alt="An OpenGL-style normal map, generated from the heightmap above. Note the difference between this normal map and the DirectX normal map." title="An OpenGL-style normal map, generated from the heightmap above. Note the difference between this normal map and the DirectX normal map." />](https://raw.githubusercontent.com/AlubJ/TTGames-LEGO-Documentation/main/media/bumpdemo/nab2_bldg_brick06_opengl.png)
There are two major formats of normal map used by the game industry. They are referred to a "DirectX" and "OpenGL" normal maps due their usage by the respective rendering APIs. The only real difference is in how the green channel (Y-axis) is handled; the green color values are inverted on a DirectX normal map compared to an OpenGL normal map, and vice versa. <br/>
An average human can usually tell whether a normal map is in DirectX or OpenGL format by just looking at it. If the bumps look like bumps and the divots look like divots, then the normal map is in OpenGL format. By comparison, if the bumps look like divots and the divots look like bumps, then the normal map is in DirectX format. <br/>
Despite using on the DirectX API on Windows and the Xbox series of consoles, Nu2 and nTT actually usually use OpenGL normal maps, or variations thereof, except in Lego Star Wars II, which uses DirectX normal maps.

### "TCS bumpmaps"
[<img align="right" src="https://raw.githubusercontent.com/AlubJ/TTGames-LEGO-Documentation/main/media/bumpdemo/nab2_bldg_brick06_tcs.png" width="256" alt="A TCS-style normal map, converted from the OpenGL normal map above." title="A TCS-style normal map, converted from the OpenGL normal map above." />](https://raw.githubusercontent.com/AlubJ/TTGames-LEGO-Documentation/main/media/bumpdemo/nab2_bldg_brick06_tcs.png)
The Complete Saga, Lego Indy 1, and Lego Batman 1 use a slightly different channel layout compared to standard OpenGL normal maps. Unlike standard normal maps, TCS bumpmaps use 4 channels, with the Z axis being represented by the alpha channel, and the red channel being left pure white. 
While the increase in channel count means a larger file size and requires more VRAM, the alpha channel in a DXT3 or DXT5 texture is actually higher quality than the color channels! Whereas the color channels share 4 bits with a palette of two BGR565 colors and two colors interpolated from those for each 4x4 block, the alpha channel has its own 4 bits. DXT3 uses a true 4-bit alpha. DXT5 gives the alpha channel four 8-bit values and four values interpolated from those.
Note that despite the name, TCS uses regular OpenGL normal maps in addition to TCS bumpmaps. The game exclusively uses TCS bumpmaps for skeletal meshes (GHGs), but uses a mix of TCS and OpenGL (or maybe DirectX) normal maps for static meshes (GSCs), with OpenGL normal maps being especially common in original trilogy levels.

### LSWIII normal maps
Nobody knows how these work. They're a psychedelic mess. 

### LDCSV and similar titles
[<img align="right" src="https://raw.githubusercontent.com/AlubJ/TTGames-LEGO-Documentation/main/media/bumpdemo/nab2_bldg_brick06_ldcsv.png" width="256" alt="A LDCSV-style normal map, converted from the OpenGL normal map above." title="A LDCSV-style normal map, converted from the OpenGL normal map above." />](https://raw.githubusercontent.com/AlubJ/TTGames-LEGO-Documentation/main/media/bumpdemo/nab2_bldg_brick06_ldcsv.png)
LDCSV normal maps are basically identical to TCS bumpmaps, except the red channel is black instead of white.


## Creating bumpmaps

This section is a WIP. 

### Generating a normal map from a Base, Diffuse, or Albedo texture


### Generating a height map from a Base, Diffuse, or Albedo texture


### Generating a normal map from a height map


