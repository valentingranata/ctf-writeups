# Bandit

## Introduction

Before the game, please read the [Introduction](https://overthewire.org/wargames/bandit/) and try solving the levels by **yourself** (here is where the real learning happens).

I would also suggest to try to use `man` instead of searching for hint or docs online, 90% of the games commands are fully documented, just use `man`.

If you are stuck, search for hints **not solutions!**
If you can't figure it out after some hints, try doing the level the day after or just read someone's walk through, maybe more than one (I would avoid watching tutorials).

Here you can see my **solutions** and **takeaways**, the `flags` can change but the process should be the same or very similar.

## Lvl 0

**Goal:** Log into the game using SSH. 

If you are not familiar with `ssh`, check the manual: `man ssh`.
If you are not familiar with `man`, check the manual: `man man`.

```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

**Takeaway:** `ssh` uses `-p` to specify a non-default port. 

---

## Lvl 0 → 1

**Goal:** Read the `readme` file in the home directory. 

Connect using the credentials from `Lvl 0`.

If you are not familiar with `ls` or `cat` commands, check the manual: `man <command>`.

Use `ls` (optional, we already know the filename) to list files and folders of the current working directory.
Use `cat` to print the `readme` file containing the flag.

```bash
cat readme 
```

**Flag**: `ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If`

**Takeaway:** Using `ls` we list the contents of the current directory and using `cat [filename]` we can print the content of a file to the screen. 

---

## Lvl 1 → 2

**Goal:** Read a file named `-` in the home directory. 

Use `./` prefix before `-` to print the file containing the flag.

```bash
cat ./- 
```

**Flag**: `263JGJPfgU6LtdEvgfWU1XP5yac29mFx`

**Takeaway:** Files named with special characters can be accessed using the `./` prefix.

--- 

## Lvl 2 → 3

**Goal:** Read a file named `--spaces in this filename--` in the home directory. 

Use `./` prefix before `--spaces` and `\` prefix before ` ` (spaces) to print the file containing the flag.

```bash
cat ./--spaces\ in\ this\ filename-- 
```

Using `""` around the filename is also valid.

```bash
cat "./--spaces in this filename--"
```

**Flag**: `MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx`

**Takeaway:** Filenames with spaces can be escaped with `\` before each space ` `, or by wrapping the full path in quotes (`""` or `''`).

--- 

## Lvl 3 → 4

**Goal:** Find and read the content of a **hidden file** inside the `inhere` directory.

Let's start by changing directory to `inhere`.

```bash
cd ./inhere
```

To see the hidden files inside the current directory, just use the `ls` command with `-a` option.

```bash
ls -a
```

Here we'll see a file named `...Hiding-From-You`, let's print it to the screen.

```bash
cat ./...Hiding-From-You
```

**Flag:** `2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ`

**Takeaway:** Files prefixed with `.` are hidden in Linux. Use `ls -a` to list all files including hidden ones.

---

## Lvl 4 → 5

**Goal:** Find and read the only **human-readable** file inside the `inhere` directory. 

Let's start by changing directory to `inhere`.

```bash
cd ./inhere
```

By doing `ls` inside the `inhere` directory we can see that we have 10 files, let's avoid checking them one by one, let's try to use the `file` command and `*` wildcard (represents "zero or more characters").

```bash
file ./-file*
```

In the output we can see that one of the files is an `ASCII text` file, let's print the content of the file to the screen.

```bash
cat ./-file07
```

**Flag:** `4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw`

**Takeaway:** We can determine the **file type** of a file with the `file` command. Using the `*` wildcard we can match multiple files with one command avoiding manual repetition.

---

## Lvl 5 → 6

**Goal:** We have to find a file with this properties:
- human-readable
- 1033 bytes in size
- not executable 
inside the `inhere` directory.

Let's start by changing directory to `inhere`.

```bash
cd ./inhere
```

By doing `ls` inside the `inhere` folder we'll instantly understand that checking each folder one by one is not a good idea.
Let's use the `find` command to try to filter the files with certain properties.
To get a better understanding of each option used in the command below, check the manual for `find` command: `man find`.

```bash
find ./ -type f -size 1033c -not -executable
```

After the `find` command we'll see only one file seems to match our search, let's print the content of that file to the screen

```bash
cat ./maybehere07/.file2
```

**Flag:** `HWasnPhtq9AVKe0dmk45nxy20cvUa6EG`

**Takeaway:** We can use the `find` command to filter files by type (`-type f`), size (`-size 1033c`, where `c` = bytes), and permissions (`-not -executable`), making it powerful for locating specific files in large directory trees.

---

## Lvl 6 → 7

**Goal:** Find a file with the following properties: 
- owned by user `bandit7`
- owned by group `bandit6`
- `33 bytes` in size

When we login we can see that there is nothing inside the home directory, not even `ls -a` can save us.

Let's try to use the `find` command on the root `/` directory with the required properties.

```bash
find / -user "bandit7" -group "bandit6" -size 33c 2> /dev/null
```

To avoid useless `Permission denied` messages, we can redirect the standard error (`stderr`) to `/dev/null` folder that acts as a "black hole" on linux, deleting anything that's going inside.

Now we should see only one file, let's print it to the screen.

```bash
cat /var/lib/dpkg/info/bandit7.password
```

**Flag:** `morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj`

**Takeaway:** We used the `find` command again but with different options, such as: `-user` and `-group` (to specify the owner and group of the file). We also learned how to redirect `stderr` to `/dev/null` (`... 2> /dev/null`), a special file that acts as a **black hole** (anything written to it is permanently discarded).

---

## Lvl 7 → 8

**Goal:** Search for the flag inside `data.txt` file next to the word `millionth`. 

To find the flag we can use the `grep` command with the following pattern `"millionth"` on the `data.txt` file.

```bash
grep "millionth" data.txt
```

**Flag:** `dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc` 

**Takeaway:** Instead of reading a huge file line by line, we can use the `grep` command to print only the line that match a **pattern** . 

---

## Lvl 8 → 9

**Goal:** Find the flag inside `data.txt`, it's the only line of text that occurs only once. 

To find the flag inside the `data.txt`, we first need to **print** the data, **sort** them and use the `uniq -c` command to print the only unique line in the sorted file (it's important for the `uniq` command to work properly that the data is sorted).

```bash
cat data.txt | sort | uniq -u
```

**Flag:** `4CKMh1JI91bUIZZPXDqGanal4xvAg0JM` 

**Takeaway:** The `|` (pipe) operator passes the output of left command to the input of the right command. `sort` orders lines alphabetically, and `uniq -u` prints only lines that appear exactly once (but `uniq` requires sorted input to work correctly).

---

## Lvl 9 → 10

**Goal:** Find the flag in the `data.txt` file, the flag is preceded by several `=` chars. 

To solve this level we have to use the `strings` command to print all printable characters in the file, pipe `|` the result with `grep` to filter for `=` chars. 

```bash
strings data.txt | grep "====="
```

**Flag:** `FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey` 

**Takeaway:** The `strings` command extracts **human-readable text** from binary files. Combined with `grep`, it let's us filter for specific patterns (here the `=` characters that precede the flag).

---

## Lvl 10 → 11

**Goal:** Read the flag from `data.txt` file encoded in base64.

We can use the `base64` command with the `-d` option to **decode** a base64 encoding.

```bash
base64 -d data.txt
```

**Flag:** `dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr` 

**Takeaway:** When working with base64 encoding we can use the `base64` command to **encode** or **decode** the data. 

---

## Lvl 11 → 12

**Goal:** Rotate the text inside the `data.txt` file by 13 positions. 

To rotate the letters by 13 positions, we can print the `data.txt` file and pipe it into the `tr` command to map each letter to the one 13 positions forward in the alphabet.

```bash
cat data.txt | tr [a-zA-Z] [n-za-mN-ZA-M]
```

**Flag:** `7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4` 

**Takeaway:** We can replace or delete chars by using the `tr` command. 

---

## Lvl 12 → 13

**Goal:** Decompress `data.txt` multiple times to find the **flag**. 

First as suggested, let's create a temporary folder to work in.

```bash
mktemp -d
cd <path/to/temp/folder>
```

Let's copy the `data.txt` file to the temporary folder.

```bash
cp ~/data.txt ./
```

If we print the `data.txt` file, we can see that it's a hexdump of a binary, so lets convert it back to a binary with the `xxd -r` command.

```bash
xxd -r data.txt > binary.gz
```

Now we'll check the file type with the `file` command and decompress the data until we find the **flag**.

```bash
gzip -dc binary.gz > data2.bz2 
bzip2 -dc data2.bz2 > data3.gz
gzip -dc data3.gz > data4.tar
tar -xOf data4.tar > data5.tar
tar -xOf data5.tar > data6.bz2
bzip2 -dc data6.bz2 > data7.tar
tar -xOf data7.tar > data8.gz
gzip -dc data8.gz > data9.txt
```

Last step is to print the `data9.txt` to stdout.

```bash
cat data9.txt
```

This is the final one-liner:

```bash
xxd -r data.txt > binary.gz && gzip -dc binary.gz > data2.bz2 && bzip2 -dc data2.bz2 > data3.gz && gzip -dc data3.gz > data4.tar && tar -xOf data4.tar > data5.tar && tar -xOf data5.tar > data6.bz2 && bzip2 -dc data6.bz2 > data7.tar && tar -xOf data7.tar > data8.gz && gzip -dc data8.gz > data9.txt && cat data9.txt
```

**Flag:** `FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn` 

**Takeaway:** In this challenge we learned about different commands (`gzip`, `bzip2`, `tar`) to compress and decompress files. Use the `man` command to learn more about them.

---

## Lvl 13 → 14

**Goal:** 

```bash

```

**Flag:** `` 

**Takeaway:** 

---

## Lvl 14 → 15

**Goal:** 

```bash

```

**Flag:** ``  

**Takeaway:** 

---

## Lvl 15 → 16

**Goal:** 

```bash

```

**Flag:** `` 

**Takeaway:** 

---

## Lvl 16 → 17

**Goal:** 

```bash

```

**Flag:** `` 

**Takeaway:** 

---

## Lvl 17 → 18

**Goal:** 

```bash

```

**Flag:** `` 

**Takeaway:** 

---

## Lvl 18 → 19

**Goal:** 

```bash

```

**Flag:** `` 

**Takeaway:** 

---

## Lvl 19 → 20

**Goal:** 

```bash

```

**Flag:** `` 

**Takeaway:** 

---

## Lvl 20 → 21

**Goal:** 

```bash

```

**Flag:** `` 

**Takeaway:** 

---

## Lvl 21 → 22

**Goal:** 

```bash

```

**Flag:** `` 

**Takeaway:** 

---

## Lvl 22 → 23

**Goal:** 

```bash

```

**Flag:** `` 

**Takeaway:** 

---

## Lvl 23 → 24

**Goal:** 

```bash

```

**Flag:** `` 

**Takeaway:** 

---

## Lvl 24 → 25

**Goal:** 

```bash

```

**Flag:** `` 

**Takeaway:** 

---

## Lvl 25 → 26

**Goal:** 

```bash

```

**Flag:** `` 

**Takeaway:** 

---

## Lvl 26 → 27

**Goal:** 

```bash

```

**Flag:** `` 

**Takeaway:** 

---

## Lvl 27 → 28

**Goal:** 

```bash

```

**Flag:** `` 

**Takeaway:** 

---

## Lvl 28 → 29

**Goal:** 

```bash

```

**Flag:** `` 

**Takeaway:** 

---

## Lvl 29 → 30

**Goal:** 

```bash

```

**Flag:** `` 

**Takeaway:** 

---

## Lvl 30 → 31

**Goal:** 

```bash

```

**Flag:** `` 

**Takeaway:** 

---

## Lvl 31 → 32

**Goal:** 

```bash

```

**Flag:** `` 

**Takeaway:** 

---

## Lvl 32 → 33

**Goal:** 

```bash

```

**Flag:** `` 

**Takeaway:** 

---

## Lvl 33 → 34

**Goal:** Just ssh into the level to check the password.

**Flag:** `no flag is provided for the last level`

**Takeaway:** Congratulations on completing all the bandit levels! 


