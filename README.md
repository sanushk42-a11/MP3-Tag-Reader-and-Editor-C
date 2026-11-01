# MP3 Tag Reader and Editor (C)

A command-line utility written in C to **view** and **edit** ID3v2 metadata in MP3 files. It parses the ID3v2.3 tag header and frames directly from the file, displays the stored information, and lets you modify individual fields from the terminal, with no external libraries.

## Features

- View ID3 tag details: version, tag size, title, artist, album, year, genre, comment
- Detects embedded album art (APIC frame)
- Edit individual fields: title, artist, album, year, genre, comment
- Correct handling of the synchsafe tag size (7 bits per byte)
- Safe editing: writes to a temporary file, then replaces the original
- Clear error messages for missing ID3 tags, invalid frame sizes, and bad options
- Built-in help screen

## Project Structure

```
MP3-Tag-Reader-and-Editor-C/
├── main.c          # Entry point, argument parsing
├── view.c / .h     # Reading and displaying tags
├── edit.c / .h     # Editing tags
├── help.c / .h     # Help screen
├── operations.h    # Shared enums (OperationType, Status)
└── README.md
```

## Build

```bash
gcc main.c view.c edit.c help.c -o a.out
```

## Usage

**View tags**
```bash
./a.out -v sample.mp3
```

**Edit tags**
```bash
./a.out -e -t "New Title"   sample.mp3
./a.out -e -a "New Artist"  sample.mp3
./a.out -e -A "New Album"   sample.mp3
./a.out -e -y "2026"        sample.mp3
./a.out -e -g "Rock"        sample.mp3
./a.out -e -c "My Comment"  sample.mp3
```

**Help**
```bash
./a.out -h
```

### Options

| Option | Description        | ID3 Frame |
|--------|--------------------|-----------|
| `-v`   | View MP3 tags      | n/a       |
| `-e`   | Edit MP3 tags      | n/a       |
| `-t`   | Edit Title         | TIT2      |
| `-a`   | Edit Artist        | TPE1      |
| `-A`   | Edit Album         | TALB      |
| `-y`   | Edit Year          | TYER      |
| `-g`   | Edit Genre         | TCON      |
| `-c`   | Edit Comment       | COMM      |
| `-h`   | Display help       | n/a       |

### Sample Output

```
----------------------------------------
          MP3 TAG READER
----------------------------------------
MP3 File : sample.mp3
ID3 Tag  : Found
Version  : 2.3.0
Tag Size : 12345 bytes
Title    : Song Title
Artist   : Artist Name
Album    : Album Name
Year     : 2013
Genre    : Pop
Comment  : Downloaded From Internet
----------------------------------------
```

## How It Works

An ID3v2 tag sits at the start of the MP3 file:

1. **Header (10 bytes):** `"ID3"`, version, flags, and a 4-byte synchsafe size
2. **Frames:** each has a 10-byte header (4-byte ID, 4-byte size, 2-byte flags) followed by its data
3. **Padding** (optional), then the MPEG audio data

The viewer walks through the frames and prints the ones it recognizes. The editor copies the file frame by frame, swaps in the new data for the requested frame, then copies the rest of the file unchanged.

## Limitations

- Supports **ID3v2.3** tags only (ID3v1 and ID3v2.4 are not handled)
- Only the first six text fields above can be edited, and the frame must **already exist** in the file
- The new value must **fit within the existing frame size**; otherwise the editor reports the maximum allowed length
- Text is written as ISO-8859-1 (encoding byte `0x00`)
- Editing creates a temporary `temp.mp3` in the current directory

## Possible Improvements

- Support ID3v2.4 and ID3v1
- Add missing frames when they don't exist
- Resize the tag so longer values can be written
- Add track number editing
- Extract album art and delete tags

## References

- [ID3v2.3.0 specification](http://id3.org/id3v2.3.0)
- [ID3 on Wikipedia](http://en.wikipedia.org/wiki/ID3)

## Author

**Sanush K Santhosh**

- GitHub: [sanushk42-a11](https://github.com/sanushk42-a11)
- LinkedIn: [Sanush K Santhosh](https://www.linkedin.com/in/sanush-k-santhosh-498432376)
- Project: [MP3-Tag-Reader-and-Editor-C](https://github.com/sanushk42-a11/MP3-Tag-Reader-and-Editor-C)
