# Non-Power of Two Textures
<figure align="right" src="https://cdn.discordapp.com/attachments/717427407690137680/936414494467428393/Lando_General_UVs_1.png" alt="Lando_General_GenericJedi's texture, with UVs visible" width="256" title="The GenericJedi version of Better UVs Lando_General V3 uses a non-power of two vertical resolution to make room for leg printing UVs" />

In the world of texture design, it is customary to constrain the vertical and horizontal resolutions of a texture to a power of two. The reason behind this is twofold: 
1. Computers, being binary machines, like numbers to be cleanly divisible by 2
2. The earliest 3D accelerated GPUs only accepted power of two textures, and modern GPUs that aren't beholden by this restriction require more resources to process such textures compared to power-of-two textures.

The powers of two are as follows:<br/>
`0✻, 2✻, 4, 8, 16, 32, 64, 128, 256, 512, 1024, 2048, 4096, 8192✻✻, 16384✻✻, 32768✻✻`
> ✻ DXTn block compression requires the resolutions of each axis to be divisible by 4; if you have a 2x2 texture for some reason, just make it 4x4, and a resolution of 0 is pointless and going to result in errors. <br/>
> ✻✻ DirectX 9 doesn't support textures resolutions with a total pixel count higher than that of a 4096x4096 texture; this doesn't mean that you can't have an 8K texture, but rather that the total number of pixels can't exceed 16777216. Therefore, a resolution of 
65536x256 is perfectly legitimate, though perhaps a tad impractical. 

Textures that do not conform to these resolutions are considered Non-Power of Two (NPoT) textures. 
Although usually preferably avoided, situations sometimes arise where they make things easier. When such is the case, the guidelines should be followed to avoid unnecessary performance drops: 
* Keep in mind that DXT compression requires resolutions to be cleanly divisible of 4. Resolutions cleanly divide by 16 are preferable. 
* Modern GPUs should have no issues with NPoT textures at resolutions lower than 512x512, especially if they are cleanly divisive by 16. 
* For NPoT textures larger than 512, try to keep the resolution to one that can cleanly divided into few power-of two tiles as possible, without any NPoT tiles. For example, instead of 960x720, use 1024x768. 1024x768 requires 3 tiles of 1024x256, whereas 960x720 requires a whopping 675 tiles of a much smaller 64x16. 
