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
    Ja
    ```

- Can you go through the folders?
    ```
    Ja
    ```

- Can you view the content?
    ```
    Nee, wachtwoord wordt gevraagd
    ```
    
Rename the zip file to "todolist.txt" and open it with a hex editor of choice.
```console
$ xxd todolist.txt
00000000: 504b 0304 3300 0100 6300 2380 2754 0000  PK..3...c.#.'T..
00000010: 0000 2300 0000 0700 0000 0700 0b00 6161  ..#...........aa
00000020: 612e 7478 7401 9907 0002 0041 4503 0000  a.txt......AE...
00000030: 6a21 2d41 79d3 9844 ef56 8e4a 8d09 a755  j!-Ay..D.V.J...U
00000040: 04f7 8201 463d dbdb 8513 4004 c247 3629  ....F=....@..G6)
00000050: 6517 1850 4b03 0414 0000 0000 0024 8027  e..PK........$.'
00000060: 5400 0000 0000 0000 0000 0000 0005 0000  T...............
00000070: 0074 7765 652f 504b 0304 3300 0100 6300  .twee/PK..3...c.
00000080: 2480 2754 0000 0000 2300 0000 0700 0000  $.'T....#.......
00000090: 0c00 0b00 7477 6565 2f62 6262 2e74 7874  ....twee/bbb.txt
000000a0: 0199 0700 0200 4145 0300 00b5 7c77 fcd4  ......AE....|w..
000000b0: 90cb b59b f4d6 943b c0b5 9dd8 bceb 267d  .......;......&}
000000c0: c444 c5aa 1b3d bec7 d765 69c9 a9b4 504b  .D...=...ei...PK
000000d0: 0304 1400 0000 0000 2680 2754 0000 0000  ........&.'T....
000000e0: 0000 0000 0000 0000 0a00 0000 7477 6565  ............twee
000000f0: 2f64 7269 652f 504b 0304 3300 0100 6300  /drie/PK..3...c.
00000100: 2680 2754 0000 0000 2300 0000 0700 0000  &.'T....#.......
00000110: 1100 0b00 7477 6565 2f64 7269 652f 6363  ....twee/drie/cc
00000120: 632e 7478 7401 9907 0002 0041 4503 0000  c.txt......AE...
00000130: 1c1b 1025 2a03 a88b b9bf 3c41 47c7 2acb  ...%*.....<AG.*.
00000140: 98a9 6189 8094 e406 0d92 a908 a06f c134  ..a..........o.4
00000150: 9f81 d050 4b01 023f 0033 0001 0063 0023  ...PK..?.3...c.#
00000160: 8027 5400 0000 0023 0000 0007 0000 0007  .'T....#........
00000170: 002f 0000 0000 0000 0020 0000 0000 0000  ./....... ......
00000180: 0061 6161 2e74 7874 0a00 2000 0000 0000  .aaa.txt.. .....
00000190: 0100 1800 77aa 5364 d703 d801 2c3c 7b75  ....w.Sd....,<{u
000001a0: d703 d801 77aa 5364 d703 d801 0199 0700  ....w.Sd........
000001b0: 0200 4145 0300 0050 4b01 023f 0014 0000  ..AE...PK..?....
000001c0: 0000 0024 8027 5400 0000 0000 0000 0000  ...$.'T.........
000001d0: 0000 0005 0024 0000 0000 0000 0010 0000  .....$..........
000001e0: 0053 0000 0074 7765 652f 0a00 2000 0000  .S...twee/.. ...
000001f0: 0000 0100 1800 5703 2366 d703 d801 cdd4  ......W.#f......
00000200: 8166 d703 d801 9398 9c61 d703 d801 504b  .f.......a....PK
00000210: 0102 3f00 3300 0100 6300 2480 2754 0000  ..?.3...c.$.'T..
00000220: 0000 2300 0000 0700 0000 0c00 2f00 0000  ..#........./...
00000230: 0000 0000 2000 0000 7600 0000 7477 6565  .... ...v...twee
00000240: 2f62 6262 2e74 7874 0a00 2000 0000 0000  /bbb.txt.. .....
00000250: 0100 1800 5703 2366 d703 d801 cdd4 8166  ....W.#f.......f
00000260: d703 d801 5703 2366 d703 d801 0199 0700  ....W.#f........
00000270: 0200 4145 0300 0050 4b01 023f 0014 0000  ..AE...PK..?....
00000280: 0000 0026 8027 5400 0000 0000 0000 0000  ...&.'T.........
00000290: 0000 000a 0024 0000 0000 0000 0010 0000  .....$..........
000002a0: 00ce 0000 0074 7765 652f 6472 6965 2f0a  .....twee/drie/.
000002b0: 0020 0000 0000 0001 0018 0062 c54b 68d7  . .........b.Kh.
000002c0: 03d8 0162 c54b 68d7 03d8 01ba be9c 61d7  ...b.Kh.......a.
000002d0: 03d8 0150 4b01 023f 0033 0001 0063 0026  ...PK..?.3...c.&
000002e0: 8027 5400 0000 0023 0000 0007 0000 0011  .'T....#........
000002f0: 002f 0000 0000 0000 0020 0000 00f6 0000  ./....... ......
00000300: 0074 7765 652f 6472 6965 2f63 6363 2e74  .twee/drie/ccc.t
00000310: 7874 0a00 2000 0000 0000 0100 1800 62c5  xt.. .........b.
00000320: 4b68 d703 d801 d159 d868 d703 d801 62c5  Kh.....Y.h....b.
00000330: 4b68 d703 d801 0199 0700 0200 4145 0300  Kh..........AE..
00000340: 0050 4b05 0600 0000 0005 0005 00ee 0100  .PK.............
00000350: 0053 0100 0000 00                        .S.....
```
- Can you see the file type (zip)? Tip: the zip file format has a certain [magic number](https://en.wikipedia.org/wiki/File_format#Magic_number)
    ```
    Ja, eerste lijn. Magic number is "50 4B 03 04" of "PK" voor zip files
    ```
- Can you see the folders and filenames of the content?
    ```
    Ja
    ```
- Can you see the content of the files?
    ```
    Nee
    ```
How could you make sure the folders and filenames are also hidden?
```
Niet met .zip files, encrypteert enkel de file inhoud niet metadata. 7zip files kunnen dit bv wel.
```


### Cracking zip files with John the Ripper
Let's try cracking some zip files. To do this we will use [John the Ripper](https://www.openwall.com/john/). You can install this from the website (Windows) or use your package manager (Linux).

Create an encrypted zip file with the password "123".
```console
$ echo "Hallo" >> test.txt
$ zip -p123 secret.zip test.txt
  adding: test.txt (stored 0%)
```

John the Ripper is a tool for cracking hashes. So we need to extract the hash with the password from the zip first and store it in a txt file (for example, "hashes.txt"). You can use the script `zip2john`. If `zip2john` is not found on your system, then the JtR Jumbo package is not installed (some linux package managers only provide the basic package without the extra scripts and features). In that case, you'll need to download and [compile the JtR Jumbo package yourself](https://openwall.info/wiki/john/tutorials/Ubuntu-build-howto) as shown here:

```console
$ cd ~/src
$ sudo apt install libc6-dev libssl-dev
$ git clone git://github.com/magnumripper/JohnTheRipper -b bleeding-jumbo john
$ cd ~/src/john/src
$ ./configure && make -s clean && make -sj4
```

Now release JtR on the txt file. Since the password is very short, it should give you the password soon.
```console
$ john-the-ripper.zip2john secret.zip > hashes.txt
ver 1.0 efh 5455 efh 7875 secret.zip/test.txt PKZIP Encr: 2b chk, TS_chk, cmplen=18, decmplen=6, crc=5D7BCD2
$ john-the-ripper hashes.txt 
Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 2 OpenMP threads
Proceeding with single, rules:Single
Press 'q' or Ctrl-C to abort, almost any other key for status
Warning: Only 4 candidates buffered for the current salt, minimum 8 needed for performance.
Warning: Only 6 candidates buffered for the current salt, minimum 8 needed for performance.
Warning: Only 4 candidates buffered for the current salt, minimum 8 needed for performance.
Almost done: Processing the remaining buffered candidate passwords, if any.
Proceeding with wordlist:/snap/john-the-ripper/current/run/password.lst, rules:Wordlist
123              (secret.zip/test.txt)
1g 0:00:00:00 DONE 2/3 (2022-01-07 16:40) 20.00g/s 840280p/s 840280c/s 840280C/s 123456..Peter
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

We have provided 3 zip files for you to crack: can you open the file inside?
- easy.zip (password: "letmein"): Solved within a second on an Intel i7-6700K.
- difficult.zip (password: "h4ck"): Solved in 1h 34m 2s on an Intel i7-6700K.
- [extra] superduperdifficult.zip (password: unknown): not yet solved, but expected to take at least a week. Actual time is unknown.

Examine the output of JtR when cracking "easy.zip" and "difficult.zip". Do they use the same approach (wordlist, bruteforce, heuristics, ...)? Tip: press \<space> while JtR is trying the crack the hash, it will show you the time and progress.
```console
$ zip2john easy.zip > hashes.txt
$ john hashes.txt 
Using default input encoding: UTF-8
Loaded 1 password hash (ZIP, WinZip [PBKDF2-SHA1 256/256 AVX2 8x2])
Cost 1 (HMAC size) is 4633948 for all loaded hashes
Will run 8 OpenMP threads
Proceeding with single, rules:Single
Press 'q' or Ctrl-C to abort, almost any other key for status
Almost done: Processing the remaining buffered candidate passwords, if any.
Proceeding with wordlist:/opt/johntheripper/john/run/password.lst
letmein          (easy.zip/easy.jpg)     
1g 0:00:00:00 DONE 2/3 (2021-10-29 14:43) 1.315g/s 72768p/s 72768c/s 72768C/s 123456..skyline!
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```
```console
$ zip2john difficult.zip > hashes.txt
$ john hashes.txt 
Using default input encoding: UTF-8
Loaded 1 password hash (ZIP, WinZip [PBKDF2-SHA1 256/256 AVX2 8x2])
Cost 1 (HMAC size) is 101658 for all loaded hashes
Will run 8 OpenMP threads
Proceeding with single, rules:Single
Press 'q' or Ctrl-C to abort, almost any other key for status
Almost done: Processing the remaining buffered candidate passwords, if any.
Proceeding with wordlist:/opt/johntheripper/john/run/password.lst
Proceeding with incremental:ASCII
h4ck             (difficult.zip/difficult.jpg)
1g 0:01:34:02 DONE 3/3 (2021-10-29 19:18) 1.315g/s 72768p/s 72768c/s 72768C/s s41z..4dy7
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```

### VeraCrypt
Zip files are a quick and dirty solutions. As we have seen, they have their drawbacks. To truly encrypt and hide files or folders, we can use [VeraCrypt](https://www.veracrypt.fr/). To hide sensitive information, always prefer tools like VeraCrypt over zip files! Zip files offer encryption as an extra feature, tools like VeraCrypt are specifically build for this purpose.

Create a VeraCrypt encrypted file container and insert the same structure as in your encrypted zip file. AES for encryption and SHA-512 for hashing are solid choices.

[Youtube tutorial](https://www.youtube.com/watch?v=C25VWAGl7Tw)

What is the difference between a standard and a hidden VeraCrypt volume? For which use case should you use a hidden container over a standard container? Create a hidden volume and try it out.
```
Standard volume: Geëncrypteerd volume dat zichtbaar is
Hidden volume: Geëncrypteerd volume in een Standard volume dat niet zichtbaar is en wordt weergegeven als vrije ruimte op dat Standard volume. Handig wanneer je verplicht wordt het wachtwoord van een Standard volume op te geven.
```
Rename the VeraCrypt container to "anothertodolist.txt" and open it with a hex editor of choice.

- Can you see the file type (veracrypt container)?
    ```
    Nee
    ```

- Can you see the folders and filenames of the content?
    ```
    Nee
    ```

- Can you see the content of the files?
    ```
    Nee
    ```