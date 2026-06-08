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

**Goal:** use the **private key** in the home directory to log in to the next level and read the password.  

On login, list the files in the home directory with `ls`

```bash
ls
```

Then print the contents of `sshkey.private` and copy them.

```bash
cat sshkey.private
```

Now exit from the **ssh session** with the `exit` command. 
On your local machine, create a file `privatekey` and put the content inside.

```bash
echo "-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAxkkOE83W2cOT7IWhFc9aPaaQmQDdgzuXCv+ppZHa++buSkN+
gg0tcr7Fw8NLGa5+Uzec2rEg0WmeevB13AIoYp0MZyETq46t+jk9puNwZwIt9XgB
ZufGtZEwWbFWw/vVLNwOXBe4UWStGRWzgPpEeSv5Tb1VjLZIBdGphTIK22Amz6Zb
ThMsiMnyJafEwJ/T8PQO3myS91vUHEuoOMAzoUID4kN0MEZ3+XahyK0HJVq68KsV
ObefXG1vvA3GAJ29kxJaqvRfgYnqZryWN7w3CHjNU4c/2Jkp+n8L0SnxaNA+WYA7
jiPyTF0is8uzMlYQ4l1Lzh/8/MpvhCQF8r22dwIDAQABAoIBAQC6dWBjhyEOzjeA
J3j/RWmap9M5zfJ/wb2bfidNpwbB8rsJ4sZIDZQ7XuIh4LfygoAQSS+bBw3RXvzE
pvJt3SmU8hIDuLsCjL1VnBY5pY7Bju8g8aR/3FyjyNAqx/TLfzlLYfOu7i9Jet67
xAh0tONG/u8FB5I3LAI2Vp6OviwvdWeC4nOxCthldpuPKNLA8rmMMVRTKQ+7T2VS
nXmwYckKUcUgzoVSpiNZaS0zUDypdpy2+tRH3MQa5kqN1YKjvF8RC47woOYCktsD
o3FFpGNFec9Taa3Msy+DfQQhHKZFKIL3bJDONtmrVvtYK40/yeU4aZ/HA2DQzwhe
ol1AfiEhAoGBAOnVjosBkm7sblK+n4IEwPxs8sOmhPnTDUy5WGrpSCrXOmsVIBUf
laL3ZGLx3xCIwtCnEucB9DvN2HZkupc/h6hTKUYLqXuyLD8njTrbRhLgbC9QrKrS
M1F2fSTxVqPtZDlDMwjNR04xHA/fKh8bXXyTMqOHNJTHHNhbh3McdURjAoGBANkU
1hqfnw7+aXncJ9bjysr1ZWbqOE5Nd8AFgfwaKuGTTVX2NsUQnCMWdOp+wFak40JH
PKWkJNdBG+ex0H9JNQsTK3X5PBMAS8AfX0GrKeuwKWA6erytVTqjOfLYcdp5+z9s
8DtVCxDuVsM+i4X8UqIGOlvGbtKEVokHPFXP1q/dAoGAcHg5YX7WEehCgCYTzpO+
xysX8ScM2qS6xuZ3MqUWAxUWkh7NGZvhe0sGy9iOdANzwKw7mUUFViaCMR/t54W1
GC83sOs3D7n5Mj8x3NdO8xFit7dT9a245TvaoYQ7KgmqpSg/ScKCw4c3eiLava+J
3btnJeSIU+8ZXq9XjPRpKwUCgYA7z6LiOQKxNeXH3qHXcnHok855maUj5fJNpPbY
iDkyZ8ySF8GlcFsky8Yw6fWCqfG3zDrohJ5l9JmEsBh7SadkwsZhvecQcS9t4vby
9/8X4jS0P8ibfcKS4nBP+dT81kkkg5Z5MohXBORA7VWx+ACohcDEkprsQ+w32xeD
qT1EvQKBgQDKm8ws2ByvSUVs9GjTilCajFqLJ0eVYzRPaY6f++Gv/UVfAPV4c+S0
kAWpXbv5tbkkzbS0eaLPTKgLzavXtQoTtKwrjpolHKIHUz6Wu+n4abfAIRFubOdN
/+aLoRQ0yBDRbdXMsZN/jvY44eM+xRLdRVyMmdPtP8belRi2E2aEzA==
-----END RSA PRIVATE KEY-----" > privatekey
```

Now we have to log in to **bandit14** using the **privatekey** file we created, but before that we need to set the right **permissions** on the `privatekey` file.

```bash
chmod 600 privatekey
```

```bash
ssh bandit14@bandit.labs.overthewire.org -p 2220 -i privatekey
```

Now that we are inside, let's `cat` the bandit14's password.

```bash
cat /etc/bandit_pass/bandit14
```

**Flag:** `MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS` 

**Takeaway:** In this level, we learned how to use the `ssh` command with a **private key**, which greatly increases security on our servers. 

---

## Lvl 14 → 15

**Goal:** Submit the current level's password to `localhost` on port `30000`. 

The first thing that comes to mind is to scan port `30000` using `nmap`.

```bash
nmap localhost -p30000
```

Now we know that the port is **open**, the `ndmps` service is running and the internet protocol used is tcp.
Let's try the `nc` command to send the current password to that port.

```bash
cat /etc/bandit_pass/bandit14 | nc localhost 30000
```

**Flag:** `8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo`  

**Takeaway:** In this challenge we learned how to use `netcat` to send a message to a service given the **IP** and **port**.

---

## Lvl 15 → 16

**Goal:** Submit the current level's password to `localhost` on port `30001` using SSL/TLS encryption. 

Here we can use the `openssl s_client` command to submit this level's password to **localhost:30001** (remember that the password is located at `/etc/bandit_pass/bandit15`).

```bash
cat /etc/bandit_pass/bandit15 | openssl s_client -quiet -connect localhost:30001
```

**Flag:** `kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx` 

**Takeaway:** In this level we learned how to use the `openssl s_client` command to send an encrypted message between client (us) and server (in this case also us, but this is not always the case).

---

## Lvl 16 → 17

**Goal:** Submit the current level's password to `localhost` on one of the ports from `31000` to `32000` using SSL/TLS encryption (if the server responds with the current level's password, that means you hit the wrong port). 

Here the first objective is to find the correct port to send the **message** to, to do so, we'll use the `nmap` command to scan a range of ports on `localhost`.

```bash
nmap localhost --script ssl-cert -p31000-32000
```

After the scan this will be the result:

```bash
PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
| ssl-cert: Subject: commonName=SnakeOil
| Issuer: commonName=SnakeOil
| Public Key type: rsa
| Public Key bits: 4096
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-06-10T03:59:50
| Not valid after:  2034-06-08T03:59:50
| MD5:   fa04:c746:b0d0:c3a1:984c:0708:ed4c:4505
|_SHA-1: 323a:f3b1:4fc7:1b0f:f71a:1931:8ff3:62a1:49ac:735a
31691/tcp open  unknown
31790/tcp open  unknown
| ssl-cert: Subject: commonName=SnakeOil
| Issuer: commonName=SnakeOil
| Public Key type: rsa
| Public Key bits: 4096
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-06-10T03:59:50
| Not valid after:  2034-06-08T03:59:50
| MD5:   fa04:c746:b0d0:c3a1:984c:0708:ed4c:4505
|_SHA-1: 323a:f3b1:4fc7:1b0f:f71a:1931:8ff3:62a1:49ac:735a
31960/tcp open  unknown
```

Now that we have identified the possible **ports**, let's use the `openssl s_client` command to send the current level's password to `localhost:port`, by trying each port that has SSL/TLS.

```bash
cat /etc/bandit_pass/bandit16 | openssl s_client -quiet -connect localhost:31518
```
The first attempt gives us back the current level's password so it's not correct. Let's try the other port.

```bash
cat /etc/bandit_pass/bandit16 | openssl s_client -quiet -connect localhost:31790
```

This time the **output** is a **Private Key**, this must be the solution to this level!

To make use of the **Private Key**, let's save it to a file named `bandit17_privatekey`.

```bash 
echo "-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----" > bandit17_privatekey
```

The last step is to give the right permissions to the private key.

```bash
chmod 600 bandit17_privatekey
```

**Flag:** The **private key** saved in this file `bandit17_privatekey` 

**Takeaway:** In this level we learned how to use the `nmap` command to scan for open ports on a server, we also used the `--script` option to analyze the ports for `SSL/TLS` encryptions, after that we used the `openssl s_client` command to send a message with the current level's password.

---

## Lvl 17 → 18

**Goal:** Find the only line that has been changed in the `passwords.new` compared to `passwords.old`.

To compare two files line by line, we can use the `diff` command, the flag will be the text preceded by `>`.

```bash
diff passwords.old passwords.new
```

**Flag:** `x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO` 

**Takeaway:** In this level we have learned how to use the `diff` to detect changed content line by line. 

---

## Lvl 18 → 19

**Goal:** Print the `readme` file in the home directory, the only problem is that the `.bashrc` file was modified to log you out as soon as you log in with SSH. 

To bypass the log out problem, we can specify the command to run directly when connecting to the machine with SSH.

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 "cat readme"
```

**Flag:** `cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8` 

**Takeaway:** In this challenge we learned that we can specify a command to run directly on connection using `ssh`, in this case we print the `readme` file that we knew the existence of but in other cases we could have used the `ls` command before the `cat` command.

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


