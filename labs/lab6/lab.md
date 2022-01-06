# Lab 6: Zip / VeraCrypt
This lab tests 2 methods for symmetric encryption: using zip files and VeraCrypt containers.

## Zip
### Creating encrypted zip files
Using zip files is a "poor man's" encryption method. It doesn't have as much features and could still leak some information, but will do in a pinch. To do this, use an archive manager of your choice that supports zip files. Any archive manager should be able to create password protected zip files. If you don't yet have an archive manager, you can use [7-zip](https://www.7-zip.org/) or [PeaZIP](https://peazip.github.io/).

Create a zip file with the following structure. Make sure it is password protected (use an up to date encryption algorithm such as AES-256).
```console
$ tree
.
├── aaa.txt
└── twee
    ├── bbb.txt
    └── drie
        └── ccc.txt
        
2 directories, 3 files
$ cat aaa.txt 
a1a1a1
$ cat twee/bbb.txt 
b2b2b2
$ cat twee/drie/ccc.txt 
c3c3c3
```
```console
$ mkdir -p twee/drie
$ echo "a1a1a1" > aaa.txt
$ echo "b2b2b2" > twee/bbb.txt
$ echo "c3c3c3" > twee/drie/ccc.txt
$ tree
.
├── aaa.txt
└── twee
    ├── bbb.txt
    └── drie
        └── ccc.txt

2 directories, 3 files
```

What can you see without entering the password?

- Can you see the filenames in the zip?
    ```
    Enkel de eerste folder-laag
    ```

- Can you go through the folders?
    ```
    Nee
    ```

- Can you view the content?
    ```
    Nee
    ```
    
Rename the zip file to "todolist.txt" and open it with a hex editor of choice.
$ xxd