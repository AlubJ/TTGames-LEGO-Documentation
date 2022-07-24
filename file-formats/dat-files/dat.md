DAT Files
============
DAT files are used as an archive to store all files used in the game. They have evolved frequently over the years and include many different variants.

# How to extract

We have a lot of tools at our disposal for this:

- [QuickBMS](https://aluigi.altervista.org/quickbms.htm) by Luigi Auriemma. You will also need the most recent script: [ttgames_skywalker_saga_v2.bms](https://cdn.discordapp.com/attachments/792810237098590248/961418302150828142/ttgames_skywalker_saga_v2.bms) (Please note that this is the slowest method of extracting but is able to extract nearly all DAT archives in existence)
- [DATOneArchiver](https://github.com/IsaMorphic/DATOneArchiver) by Isabelle Santin (Able to extract Lego Star Wars: The Video Game and Lego Star Wars II: The Original Trilogy archives)
- [DATManager](https://github.com/connorh315/DATManager) by Connor (Able to extract most archives)

# How to make your own

We don't have a lot of tools for this: 

- [DATOneArchiver](https://github.com/IsaMorphic/DATOneArchiver) by Isabelle Santin (Able to build Lego Star Wars: The Video Game and Lego Star Wars II: The Original Trilogy archives)
- [DATPacker](https://github.com/connorh315/DATPacker) by Connor (Able to build Dimensions archives)

# Generic structure
The main structure of the DAT file is:

## Start of file

- First four bytes of a dat file is a LE-UINT for the header offset, that has been obfuscated by: subtracting 0x100, bit shifting to the right 8 times and then XORing with 0xFFFFFFFF
- The next four bytes of a dat file is a LE-UINT for the header size, so that the game can buffer the whole header into memory.
- There is then 0x100 bytes of padding (usually zeroes), before the main file "blob" starts.

## File blob

This contains all the file data, stored back-to-back to be as compact as possible. Each file can either be compressed by using one of twelve compression methods, or be uncompressed.

Compressed files will be split up into chunks, and each chunk will have a four-byte character code at the very start of the chunk, to signify the compression method, such as "DFLT", "LZ2K" or "OODL". Following this four-byte code is a LE-UINT declaring the compressed size of the chunk, and a LE-UINT declaring the decompressed size of the chunk. The rest of the chunk is the compressed file data, with a length already declared by the first UINT (compressed size).

Decompressed files are not split into chunks, and are the whole file from start to finish.

All relevant data (such as offsets, total file size etc.) are contained in the DAT header...

## DAT Header

Only a generic breakdown is given here, as the DAT header varies from game-to-game and generation. More technical breakdowns will be provided in separate documentation.

Note that endianness is variable here, newer archives will most likely be big-endian and older archives most likely little-endian.

### Header start

The start of the header will contain:

- File count as a UINT
- Archive ID as a INT (I assume to stop you from trying to use Skywalker Saga DATs on the Hobbit, for example). Currently a negative number from -1 -> -13 (where -13 is TSS)

It then transitions to declaring the following info for the name blob and name table:

- Segment count for file names as a UINT
- Name blob length as a UINT

### Name blob

After the start of the header, is usually the name blob, this contains all of the names that are used to construct full paths for files.

For example, if we were to have five files:

- \chars\minifig\voldemort\voldemort.cd
- \chars\minifig\voldemort\voldemort.dno
- \chars\minifig\harry\harry_potter.cd
- \chars\minifig\harry\harry_potter.dno
- \chars\collection.txt

Then the DAT file will try to "compress" the filenames like so:

It will tear apart the files at the backward-slashes, and store the separate segments like so:

chars..minifig..voldemort..voldemort.cd..voldemort.dno..harry..harry_potter.cd..harry_potter.dno..collection.txt

This is an efficient system at storing multiple paths that share parent directories. Needless to say though a system must be in place for building these paths again...

### Name resolution table

This will contain information for every segment that needs to be declared, such as the parent, the offset of the segment in the name blob, and also the file id, if the segment is the file

This heavily varies between version of the DAT file, so it deserves its own explanation per-DAT file.

### File info table

Again, this varies heavily, but will usually contain for each file:

- File offset location in the DAT file as a UINT
- The compression method used as a BYTE
- The compressed size as a UINT
- The decompressed size as a UINT

The file offset and the compression method may be ANDed together for certain variants.

### FNV path list

The first file path that is declared doesn't belong to the first file info entry, this is because (for further obfuscation) the file paths are hashed using the FNV algorithm, and are then ordered by their hashes.

Once the hashes have been sorted, the first hash in the list becomes the first file in file info, the second hash becomes the second file in file info... etc. etc.

This list then contains the computed hashes as either UINTs (in 32-bit archives) or ULONGs (in 64-bit archives)

### Compression blob

This just contains a bunch of BYTEs representing the compression method that every file uses, and I think that this is the best example of how DAT files have become a mess as they have evolved as this is already declared in other parts of the header. Furthermore, some DAT files will also write compressed size blobs at the end.

## Final notes

Unfortunately, with the evolution of DAT archives over the past 20 years, it is too much of a mess to generalise to DAT files into one documentation file. However, if you decide to take a deeper review into DAT archives, you'll realise there's a lot of history, and a lot of reasons why they change the archives over the years. From the frequent hash collisions on 32-bit archives to the change in compression methods used throughout the years - They're actually pretty interesting.

