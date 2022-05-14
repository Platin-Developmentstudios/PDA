# PDA
## Simple archive format

### TODO
- [x] Regular files
- [x] Directories
- [x] Symlinks
- [x] File modes
- [x] Create image
- [x] List files in image
- [x] Extract files from image

### Dependencies to build
* clang++
* make
* Linux or WSL

### Building and usage
* Compile the code with ```make```. Executable will be available in bin/ folder
* Create PDA archive:
```
bin/pda create myarchive.pda myfile.txt mydir mysymlink.pdf
```
* Extract PDA archive:
```
bin/pda extract myarchive.pda myoutdir/
```
* List files in PDA archive:
```
bin/pda list myarchive.pda
```

### Specifications
PDA archive consists of chainloaded file headers and contents. First file header starts at offset 0x00. To get contents of that file, add ```sizeof(fileheader)``` (0x112 or 274 bytes) to header offset. To get next file header, add another ```file->size``` bytes
```
1st file header -> 1st file contents -> 2nd file header -> 2nd file contents
```
File header structure:
```c
#define PATH_LENGTH 128
#define PDA_SIGNATURE "PDA"

struct fileheader
{
    char signature[5]; // Should be PDA_SIGNATURE
    char name[PATH_LENGTH]; // File name with full path
    char link[PATH_LENGTH]; // Contains relative path to symlinked file
    uint64_t size; // Size of file contents in bytes
    uint8_t type; // File type
    uint32_t mode; // File modes
} __attribute__((packed));
```
File types:
```c
enum filetypes
{
    PDA_REGULAR = 0,
    PDA_DIRECTORY = 1,
    PDA_SYMLINK = 2
};
```


