# Born-Digital Camera Original Video Formats
This repository provides introductory scripts for packaging, transferring, and verifying born-digital camera original video formats. All scripts are written in bash. The scripts generally assume the best-case scenario that you are transferring files directly off of a flash memory card (like an SD, SxS, or a CF card), but they can be adapted for use with files coming from any source or configuration.

## Overview
The following scripts can be followed as a basic workflow, but it's possible that not all steps are necessary for your archive, and different tools can be swapped in that are more or less intensive (using md5 in Terminal rather than creating an md5 manifest using hashdeep is a faster method of checking source files against a destination after a transfer, for example). Overall, the steps are as follows:
- Create checksums for files at source
- Copy files from the root directory at source to destination
- Verify checksums at destination
- Package into a tarball to maintain directory structure during storage

#### Create checksums for source files
**Create md5 manifest using hashdeep**
`$ hashdeep -re path/to/source01_location > path/to/known_hashes.txt`
- `-r` recursive mode
- `-e` show progress as manifest is generated

#### Transfer files off flash memory card
**Rsync to storage volume**
`$ rsync -va --progress /path/to/source/files/or/directory /path/to/destination/directory`
- `-v` verbose, gives you more information about transfer
- `-a` archive mode: recursive, preserves permissions
- `--progress` shows progress during transfer

#### Verify checksums for transferred files
**Verify md5 manifest using hashdeep**
`$ hashdeep -vvreak path/to/known_hashes.txt path/to/sourcelocation02 > path/to/hashdeep_validation.txt`
- `-vv` extra verbose
- `-r` recursive mode
- `-e` compute estimated time remaining for each file
- `-ak` audit mode to validate files against known hashes

#### Package files to preserve hierarchy
**Compress directory structure into tarball**
`$ tar -czvf name-of-archive.tar.gz /path/to/directory-or-file`
- `-c` create an archive
- `-z` compress archive with gzip
- `-v` verbose

**When extracting, use tar command again**
` $ tar -xzvf name-of-archive.tar.gz`
- `-x` extract (vs. create)

