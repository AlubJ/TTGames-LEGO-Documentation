# Texture Compression

## Uncompressed textures
### RGB888 ("24-bit")
RGB888 is used for flat uncompressed textures with full color depth and no transparency or translucency. It's high file size makes it unsuitable for large textures, but its high compatibility potentially desirable when porting particularly detailed low-resolution textures with single-color gradients. 

### RGBA8888 ("32-bit")
RGBA8888 is like RGB888, but it adds an alpha channel for translucency. This makes it suitable for low-res decals that may look too blocky with RGBA4444 or DXT5, but BC7 should be favored if possible due to file size. 

### IA88 ("Grayscale with alpha")
IA88 has no color data, instead having just a greyscale channel and an alpha channel. This makes it suitable for grayscale images with high gradient detail and translucency. Probably not supported by Nu2. 

### A8 ("Alpha-only")
Like IA88, except only one color (nominally black). Usually used as an alpha mask or for monochrome particles and decals such as scorch marks. Probably not supported by Nu2. 

## Reduced color depth
### RGB565
RGB565 is similar to RGB888, but with less color accuracy. If BC7 is not possible, and translucency is not required, then RGB565 may be desired for textures with noticeable gradients, as the loss in color depth isn't as noticeable. Nonetheless, some engine versions don't support 565, such as the version used in Lego Batman 1. 

### RGB5551
RGB5551 is similar to RGB565, except it reduces the color accuracy of the green channel by 1 bit in favor of a 1-bit alpha channel, allowing pixels to be either fully opaque or fully transparent. This makes it suitable for low-res ladders or window frames. Our eyes aren't as sensitive to green as much as red, so the color difference between RGB565 and RGBA5551 is negligible. 

### RGBA4444
Unlike RGB565 and RGBA5551, RBGA4444 spreads the 16 bits evenly across all four color and alpha channels. Unfortunately, this means it actually can look significantly worse than DXT5, so if the gradients are noticeable and BC7 isn't possible, then it'll be necessary to use RGBA8888 or IA88 instead. Alternatively, it may be possible to use an RGB565 texture with a separate texture containing an A8 alpha mask. 

## Paletted/Indexed
### P8 ("8-bit, 256-color")
P8 textures store a color lookup table (a "color palette") with up to 256 RGB888 values, and each pixel uses one of these values. <br/>
The DSS format *does not support* 256-color textures, and trying to use an 8-bit PNG en lieu of a DDS *will* result in a crash. Nonetheless, this does not mean you'll never encounter such textures. The PS2 versions of Lego Star Wars I+II made liberal usage of such textures, and these were often higher-quality than their DXT-compressed counterparts on PC, Xbox, and GameCube. Early 3D games from the late 90s, such as Dark Forces II or Half-Life, often only supported indexed textures, and will look worse when converted to DXTn (eyes Half-Life: Source). When dealing with 256-color textures, it is therefore a matter of balancing quality with filesize. An RGBA8888 will contain all the detail from a formerly indexed texture, but will also be quadruple the file size. An RGB565 or RGBA5551 won't be as faithful to the original color values, but will only be double the file size and may be sufficient even for textures with transparency, as most indexed image formats only used a binary alpha, with the notable exception of some formats on the PS2 and PNG8+8. 

## CPU-decoded compression
### PNG
Although PNG and other common image compression formats may provide smaller file sizes than DDS offers, using them will actually increase user VRAM usage, due to not being able to be hardware-decoded by the GPU. Additionally, PNG doesn't support pre-calculated mipmaps, so since Nu2 doesn't calculate on-the-fly mipmaps, distant textures and high-res UI elements will be significantly more aliased than if they used an uncompressed or DXT-compressed format. Use sparingly. 

### JPG
Just don't.

## S3 Texture Compression (S3TC/DXTn/BCn)
### BC1 ("DXT1")
DXT1 is the most basic form of S3TC, and is half the file size of an 8-bit texture of the same resolution. It is the most supported, having been introduced in 1998 for the Savage3D 3D-accelerated GPU and subsequently included in DirectX 6 and OpenGL 1.3 and supported by the Dreamcast, GameCube, Xbox, PSP, PS3, and 3DS (as well as all subsequent API versions and game consoles). <br/>
DXT1 works by dividing the image into 4x4 blocks (hence BC = Block Compression), and having a dedicated color palette for each block. The palette consists of 2 discrete RGB565 colors and two interpolated colors. The interpolated colors are calculated relative to the discrete colors, and as such are shades of the discrete colors. To optimize for this, avoid having more than 2 major colors in one 4x4 block, only relying upon the interpolated colors for gradients and antialiasing. <br/>
DXT1 also supports a binary alpha channel, which takes the place of one of the two interpolated colors for blocks with transparency. The transparent pixels are completely black, and DXT5 should usually be used instead if transparency is desired. <br/>
DXT1 is recommended for as the go-to format for most opaque textures larger than 256x256. If using for "pixel art", integer upscaling by a multiple of 4 will result in no blocky artefacting, although they would still have a reduced color depth due to the discrete colors using RGB565. 

### BC2 ("DXT3")
DXT3 takes the opaque texture compression method used by DXT1 and adds a discrete 4-bit alpha channel for translucency. The newer DXT5 is preferable over DXT3. Nonetheless, the PC versions of Lego Star Wars I+II use DXT3 for *all* their textures, regardless of whether translucency is used. This is wasteful, because DXT3 compression results in a slightly larger file size due to the additional 4 alpha bits. 

### BC3 ("DXT5")
Unlike DXT3's 4-bit alpha, DXT5 uses a similar method for its alpha as the standard S3TC uses for color compression, having two discrete alpha values. Unlike with the color compression, however, the alpha values use a full 8 bits, and there are either 4 or 6 interpolated alpha values (if there are 4 interpolated values, then the last two values are fully opaque and fully transparent). <br/>
DXT5 is recommended for most textures that have translucency, and is almost always significantly more accurate at representing alpha compared to DXT3, while having the same file size. Lego Star Wars: The Complete Saga and later titles almost exclusively use DXT5 for any textures that have transparency or translucency. 

### BC7 ("DX10")
Despite what the FourCC implies, BC7 actually requires DirectX 11 or higher; the FourCC is shared with BC4 and BC5, which were introduced in DirectX 10. Regardless, it isn't supported by DirectX 9. When it can be used, however, it far surpasses DXT1 and DXT5 in terms of quality, with an opaque BC7 texture being comparable in file size to a DXT5 texture of the same resolution. Compared to the older methods, BC7 doubles the color accuracy, with each 4x4 block having 4 discrete RGB888 or RGBA8888 colors and 4 interpolated colors. This makes it highly suitable for colorful high-detail textures and sprites, and is used for the character icon sheet in The Skywalker Saga. 

### DXT2, DXT4, BC4, BC5, and BC6
These formats are scarcely used, and you might never encounter them. 
DXT2 and DXT4 are allegedly inferior versions of DXT1 and DXT5, respectively. I couldn't tell you how they differ, because nobody uses them. <br/>
BC4 and BC5 were introduced in DirectX 10. BC4 is greyscale only, with 8-bit precision and encoded like DXT5's alpha channel, being used by Lego Batman 3 for its cubemaps. BC5 is effectively two BC4 textures combined, having two channels with 8-bit precision and each with their own block palette and apparently having been designed for a non-standard type of normal map.<br/>
BC6, also known as BC6H, was introduced in DirectX 11 and is designed for HDR textures, being like BC1 but with the palette encoded as RGB161616f (16-bit float per channel), and no transparency.