---
layout: post
title:  "C cheat sheet"
date:   2016-04-06 15:50:03 -0400
categories: c programming language
---


# Reading from STDIN

{% highlight c %}
# include <stdlib.h>

void main(){
    int c; //We read into integer because there might be values that don't fit into character value

    while( (c = getchar()) != EOF){ //EOF can be simulated by CTRL + D on Linux
        printf("%c\n",c);
    }
}

{% endhighlight %}


# Pointers

Pointers are variables in C that contain address of some variable.

{% highlight c %}
char *p;
p=&c; //we are pointing at address of c (c is a char variable) and our pointer has a size of char
{% endhighlight %}

* '&' opertor only applies to variables and array elements (objects in memory). It cannot be applied to registers, expressions, and constants.
* '*'is the indirection or dereferencing operator. It operates on a pointer variable, and returns an l-value (it is a value that has an address that can be accessed through some operator) equivalent to the value at the pointer address.

# Strings
{% highlight c %}
strcmp(str1,str2); //return 0 if equal
strcpy(destination, source); //copies string from source into destination
atoi(s); //converts char into integer, returns 0 if char cannot be converted into integer
{% endhighlight %}

# Arrays

{% highlight c %}
int nums[100];
{% endhighlight %}

# Prototypes

{% highlight c %}
return_value function_name(type param); //we put them at the beginning of our pre so that the order of our functions does not matter
{% endhighlight %}

**Libraries**

{% highlight c %}
#include <unistd.h>
int chdir(const char *path); //changes directory of the process
/*Upon successful completion, 0 shall be returned. Otherwise, -1 shall be returned, the current working directory shall remain unchanged, and errno shall be set to indicate the error.*/ 
	
char* getcwd( char* buffer, size_t size );
/* buffer - NULL, or a pointer to a buffer where the function can store the directory name. size - The size of the buffer, in bytes. */

#include <sys/wait.h>
pid_t wait(int *stat_loc);

/* The wait() function shall suspend execution of the calling thread until status information
for one of the terminated child processes of the calling process is available, 
or until delivery of a signal whose action 
is either to execute a signal-catching function or to terminate the process.*/ 

pid_t waitpid(pid_t pid, int *stat_loc, int options); 
#include <stdlib.h>
exit(0); //success and -1 faillure
{% endhighlight %}

**Errors**
error: variably modified ... at file scope.

The const qualifier really means "read-only"; an object so qualified is a run-time object which cannot (normally) be assigned to. The value of a const-qualified object is therefore *not* a constant expression in the full sense of the term. (C is unlike C++ in this regard.) When you need a true compile-time constant, use a preprocessor #define (or perhaps an enum).

**Pre processor directives**

We can define macros in C which are used to replace constants and constant expressions. They cannot be changed by C code.
When C code is compiled, compiler replaces macros through the code by their value.

{% highlight c %} 
#define COMMANDSIZE 100 // NO FKN SEMICOLON AFTER #define CNAME VALUE
#define NAME "Obama"
#define AGE (100/2) //we can store expressions
{% endhighlight %} 

# Posix Shared Memory

Unix processes don't share address spaces, so they communicate through "mail boxes" or "messages".

**Posix Shared Memory Functions**

* shm_open - it opens a shared memory 
* mmap - it maps shared memoru into process adress space (we can reference shared memory block as a variable in our program)
* mumnap
* shm_unlink

**shm_open & shm_unlink**

* **shm_open()** creates and opens a new, or opens an existing, POSIX shared memory object
 * A POSIX shared memory object is in effect a **handle** which can be used by unrelated processes to mmap(2) the same region of shared memory


* **shm_unlink()** function performs the converse operation, removing an object previously created by shm_open()

{% highlight c %} 
#include  <gtsys/mman.hh>
#include <sys/stat.h>
/* For mode constants */
#include <fcntl.h>
/* For O_* constants */
int shm_open(const char *name, int oflag, mode_t mode);
//name is a null-terminated string that defines name of shared obj
//int oflag: bit mask O_RDONLY or O_RDWR and others
//mode_t mode: unix file mode (ie. 0666)

int shm_unlink(const char *name);
{% endhighlight %}

**mmap**


> void *mmap(void *addr, size_t length, int prot, int flags,int fd, off_t offset);

* void *addr: where to map the memory. `NULL`allows the OS to choose
* allocates length` bytes starting at `offset` bytes in file `fd`
* prot controls whether we  can read, write, etc.
* flags argument  determines whether updates are visible to other processes



Example:
{% highlight c %} 
#include <sys/mman.h>
void *mmap(void *addr, size_t length, int prot, int flags, int fd, off_t offset);
int munmap(void *addr, size_t length);
 {% endhighlight %}

**Semaphores**

Example
{% highlight c %}
#include 
sem_t binary;
....
sem_init(&binary,1,1); //last argument is the value of semaphore
...
sem_wait(&binary);
...
sem_post(&binary);
{% endhighlight %}

***


# Bit Manipulation 
If you need to build a bit map in C you need to use bit manipulation operators that C provides. First, we need to have array of bytes over which we are going to do our bit manipulations.

Bellow are some of the C data types: 

| Data type|     Size      | 
|----------|:-------------:|
| *char*   |  1 byte       |
| *int*    |  2 - 4 bytes  |
| *long*   |  4 bytes      |

[More C data types](http://www.tutorialspoint.com/cprogramming/c_data_types.htm)

We can see that **char data type** has a nice size of 1 byte (8 bits). Lets use it.

{% highlight c %}
unsigned char bitmap[200];//we make and array of 200 bytes
//unsigned means that our char cannot be negative
{% endhighlight %}

* How can char be unsigned? [Answer](http://stackoverflow.com/questions/4337217/difference-between-signed-unsigned-char)
  * > It is true that char is mostly intended to be used to represent characters. But characters in C are represented by their integer "codes".




### Now lets talk about bitwise operators in C:



 A = 0011 1100
 B = 0000 1101

A&B = 0000 1000

A|B = 0011 1101

A^B = 0011 0001 //**XOR**  1^0 -> 1; 1^1 -> 0

~A  = 1100 0011

They are very similar to [set](https://en.wikipedia.org/wiki/Set_%28mathematics%29) and [logic](https://en.wikipedia.org/wiki/Logical_connective) operators.

C also has the following operators '<<' and '>>'. 



*For example:*


> 1 << 2 is kind of the same as 0000 0001 << 2 = 0000 0100 = 1 x 2^2 + 0 x 2^1 + 0 x 2^0=4 


> 4 >> 1 = 2



*Example of simple bitmap implementation:*

{% highlight c %}
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

unsigned char bitmap[10];//each char is 1 byte


void bitmapReserve(int byte_num, int index){ //bit index starts at 0
	int bit_index = 7 - index; //I like thinking about bit map as an array starting on the left 
	bitmap[byte_num] = bitmap[byte_num]|(1 << bit_index);
}


void bitmapFree(int byte_num, int index){
	int bit_index = 7 - index;
	bitmap[byte_num] = (bitmap[byte_num]^(1 << bit_index));//Why cant we use ~(bitmap[byte_num]&(1 << bit_index)?

}

void write_out_bitmap(){
	FILE *fp;
	fp = fopen("test", "w+");
	int i=0;
	for(i;i<10;i++){
		fwrite(&bitmap[i], 1, sizeof(bitmap[i]), fp); //we want to write a binary out
	}
	fclose(fp);
}

int main(){
	bitmapReserve(1,0);
	bitmapFree(1,0);	
	write_out_bitmap();
	exit(0);	
}
{% endhighlight %}



{% highlight shell %}
$ gcc bitmap.c -o bitmap; ./bitmap; xxd -b test
00000000: 00000000 00000000 00000000 00000000 00000000 00000000  ......
00000006: 00000000 00000000 00000000 00000000                    ....
{% endhighlight %}

Resources:


* [fwrite()](http://www.tutorialspoint.com/c_standard_library/c_function_fwrite.htm)



* [xxd](https://spin.atomicobject.com/2013/09/09/5-unix-commands/)




* [Bit manipulations in C](http://www.tutorialspoint.com/ansi_c/c_bits_manipulation.htm)






***

# File System
 It should provide management for secondary storage and  I/O devices. Basically we have a block of memory that we structure in some manner. After, OS uses this structure to store files.

<---------DISK----------->

| **MBR** | **Super Block** | <--**inodes**---> | data block1 | data block2 | ... |

                     
* **MBR** (master boot record) it contains boot loader if OS is present on Disk
* **Super Block**
 * tells about structure of partition
 * tells about size of the blocks into which we partioned disk
 * tells file system size = number_of_blocks * block_size
 * inode table length in blocks
 * bit-map location

*Example in Bash:*

{% highlight shell %}
$ls //it only parses names in the directory table
tests tutorial
$ls -ai //it parses directory table to show <inodes,filename> pair
1999 . 1950 .. 2003 tests   2004 tutorial
{% endhighlight %}

Note that inodes are allocated contiguously


## File
File is abstract data type with a purpose to store data on the disk. 
File should support:
* creating -> grab space + make entry in directory
* writing -> sys call with name + data to write
* reading -> sys call with name + where we gonna put data to read
* re-positioning within a file
* deleting a file
* truncating a file
 

## Directory
It is an abstract data type to organize files and another directories. In essence, it is a mapping table that associates names to the corresponding **inode**.

*Example of basic file directory implementation:*

{% highlight c %}

my_open("filename")
i_node=lookup_dir("filename")//copy of inode
fd = create_open_fieldesc() //allocates descriptor from file descriptor table
filedesc[fd].inodebuf=i_node_buffer //put inode into file descriptor
filedesc[fd].rwpointer=0 //tells us where we read write in the file
return fd

{% endhighlight %}

A lot of operations on the files require searching the directory table. OS prevents it by using open statement which retrieves a file name from directory and makes an entry in **open file table** which contains information about open files. When we want to write to this file, we can access it at the index in O(1). When we close a file corresponding entry is deleted from this table. Also, when we open a file we pass our own parameters that are then compared against file parameters. For example, our user id and mode. If we try to open file and one of the attributes does not much with **file information recorded in inode** os give an error.
 
## FUSE (file system in user space)

>  The idea here is that if there are tools to interact with an object in terms of a directory structure and file system operations, you can write a FUSE file system to provide that interaction. You just write code that implements file operations like open(), read(), and write(); when your file system is mounted, programs are able to access the data using the standard file operation system calls, which call your code.

You write an adapter for open(), read(), and write(). They can be then used by OS to access mounted file system. For example, if you plugin your windows phone through USB you cannot see system files of a phone. You can write FUSE file system to provide that interaction. We are going to fuse phone's file structure with PC's. 

*Example of FUSE wrapper:*

* It aggregates fuse functionalities into user friendly functions that can be used abstractly by another program
 
{% highlight c %}

#define FUSE_USE_VERSION 30

#include <fuse.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <fcntl.h>
#include <sys/stat.h>
#include <dirent.h>
#include <errno.h>
#include <sys/time.h>

static int fuse_getattr(const char *path, struct stat *stbuf)
{
    int res = 0;
    int size;
    
    memset(stbuf, 0, sizeof(struct stat));
    
    if (strcmp(path, "/") == 0) {
        stbuf->st_mode = S_IFDIR | 0755;
        stbuf->st_nlink = 2;
    } else if((size = sfs_getfilesize(path)) != -1) {
        stbuf->st_mode = S_IFREG | 0666;
        stbuf->st_nlink = 1;
        stbuf->st_size = size;
    } else
        res = -ENOENT;
    
    return res;
}

static int fuse_readdir(const char *path, void *buf, fuse_fill_dir_t filler,
        off_t offset, struct fuse_file_info *fi)
{
    char file_name[MAXFILENAME];
    
    if (strcmp(path, "/") != 0)
        return -ENOENT;
    
    filler(buf, ".", NULL, 0);
    filler(buf, "..", NULL, 0);
    
    while(sfs_getnextfilename(file_name)) {
        filler(buf, &file_name[1], NULL, 0);
    }
    
    return 0;
}

static int fuse_unlink(const char *path)
{
    int res;
    char filename[MAXFILENAME];
    
    strcpy(filename, path);
    res = sfs_remove(filename);
    if (res == -1)
        return -errno;
    
    return 0;
}

static int fuse_open(const char *path, struct fuse_file_info *fi)
{
    int res;
    char filename[MAXFILENAME];
    
    strcpy(filename, path);
    
    res = sfs_fopen(filename);
    if (res == -1)
        return -errno;
    
    sfs_fclose(res);
    return 0;
}

static int fuse_read(const char *path, char *buf, size_t size, off_t offset,
        struct fuse_file_info *fi)
{
    int fd;
    int res;
    
    char filename[MAXFILENAME];
    
    strcpy(filename, path);
    
    fd = sfs_fopen(filename);
    if (fd == -1)
        return -errno;
    
    if(sfs_fseek(fd, offset) == -1)
        return -errno;
    
    res = sfs_fread(fd, buf, size);
    if (res == -1)
        return -errno;
    
    sfs_fclose(fd);
    return res;
}

static int fuse_write(const char *path, const char *buf, size_t size,
        off_t offset, struct fuse_file_info *fi)
{
    int fd;
    int res;
    
    char filename[MAXFILENAME];
    
    strcpy(filename, path);
    
    fd = sfs_fopen(filename);
    if (fd == -1) 
        return -errno;
    
    if(sfs_fseek(fd, offset) == -1)
        return -errno;
    
    res = sfs_fwrite(fd, buf, size);
    if (res == -1)
        return -errno;
    
    sfs_fclose(fd);
    return res;
}

static int fuse_truncate(const char *path, off_t size)
{
    char filename[MAXFILENAME];
    int fd;
    
    strcpy(filename, path);
    
    fd = sfs_remove(filename);
    if (fd == -1)
        return -errno;
    
    fd = sfs_fopen(filename);
    sfs_fclose(fd);
    return 0;
}

static int fuse_access(const char *path, int mask)
{
    return 0;
}

static int fuse_mknod(const char *path, mode_t mode, dev_t rdev)
{
    return 0;
}

static int fuse_create (const char *path, mode_t mode, struct fuse_file_info *fp)
{
    char filename[MAXFILENAME];
    int fd;
    
    strcpy(filename, path);
    fd = sfs_fopen(filename);
    
    sfs_fclose(fd);
    return 0;
}

static struct fuse_operations xmp_oper = {
    .getattr = fuse_getattr,
    .readdir = fuse_readdir,
    .mknod = fuse_mknod,
    .unlink = fuse_unlink,
    .truncate = fuse_truncate,
    .open = fuse_open, 
    .read = fuse_read, 
    .write = fuse_write, 
    .access = fuse_access,
    .create = fuse_create,
};

int main(int argc, char *argv[])
{
    mksfs(1);
    
    return fuse_main(argc, argv, &xmp_oper, NULL);
}
{% endhighlight %}