SPEC NOTES

functions to modify

```
write_superblock
write_block_group_descriptor_table
write_block_bitmap
write_inode_bitmap
write_inode_table
write_root_dir_block
write_hello_world_file_block
```

Spec Notes:

Create a valid ext2 filesystem image that a kernel count mount, using:
- wrapped system calls
- provided ext2 structures

Root directory - inode 2

Create a 1 MiB ext2 file system with 1 KiB sized blocks and space for 128 inodes

4 Inodes: 
- root			- directory
- lost+found 	- directory
- hello-world	- regular file
- hello			- symlink

We are provided the inode for the lost+found along with its directory block

root and lost+found 
- owned by uid 0 and gid 0
- rwxr-xr-x

hello-world and hello 
- owned by uid 1000 and gid 1000
- rw-r--r--

hello-world
- 12 bytes long
- contains "Hello world\n"



Tips:
After you're done with the superblock and the first inode, things will go faster

Skeleton code creates a 1 MiB image file for you, initialized all to zero

When you assign {0} to a struct in C, it will zero initialize it

Zero initialize everything

Blocks start from 0
Inodes start from 1



Useful commands:
```
make 
./ext2-create				# runs executeable to create cs111-base.img
dumpe2fs cs111-base.img		# dumps the filesystems info to help debug
fsck.ext2 cs111-base.img	# check that your file system is correct
mkdir mnt					# create a directory to mount your filesystem to
sudo mount -o loop cs111-base.img mnt 	
							# mounts your filesystem, loop lets you use a file
sudo umount mnt 			# unmount the file system when you're done
rmdir mnt					# delete the dir used for mounting when you're done
python -m unittest 			# some base tests <3
```

LAB 4 DEMO NOTES
You are making your own ext4 file system
./ext2-create			# creates css-base.img

cs111-base.img

superblock: describes the format of the bits  

first 1024 bytes set aside for booting data. used to jump into actual
operating system.

https://www.nongnu.org/ext2-doc/ext2.html#:~:text=The%20Second%20Extended%20Filesystem%20uses,bitmaps%20to%20keep%20track%20of

http://www.science.smith.edu/~nhowe/262/oldlabs/ext2.html

https://www.youtube.com/watch?v=YRpUVGZ2uB4

`#define EXT2_SUPER_MAGIC 0xEF53`


You shouldn't have to change anything in main

Superblock describes the contents of the file

### Defines
```
BLOCK_SIZE
NUM_BLOCKS
NUM_INODES
```

first 10 inodes are reserved
1. bad inode
2. root directory