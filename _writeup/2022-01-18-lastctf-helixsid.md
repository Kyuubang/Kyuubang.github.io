---
title: "CTF: LastCTF Helixsid"
tag:
- docker
- osint
- fernet
imagepreview: https://user-images.githubusercontent.com/56214296/150106770-c98d66bc-4653-4cf1-a5e3-ad0345d1bb55.png
---

A few days ago I joined the CTF competition called LastCTF hosted by the Helixsid 
community held from 25 December 2021 until 11 January 2022. I think this is good 
for filling free time during school holidays. Mechanism the competition in 
jeopardy. the challenge would be spawned regularly.

I reach the top 13 in the competition succeed with 7 flags. Not bad a start, keep it up!

![lastctf preview](/assets/images/preview/lastctf-thumb.png)

> **Notice** <br />
> The information provided on this site is for educational purposes regarding 
> pentesting. The author of the site will not be held any responsibility for any 
> misuse of the information from this site.

#### Summary

* [Ba Dum Base](#ba-dum-base)
* [Minecraft](#minecraft)
* [Sejarah WWW](#sejarah-www)
* [Docker Rescue](#docker-rescue)
* [Pernet](#pernet)
* [Lambe Rekta](#lambe-rekta)
* [Para Penggila Musik (Osint)](#para-penggila-musik-osint)

### Ba Dum Base

given text file contains a collection of text that looks like forming letters, 
and try to concatenate the file.

![ba-dum-base](/assets/images/writeup/ba-dum-base.png)

after guessing every character, I found in this text. look good for base64 format. 
I try to decode as base64, and yeah I found the flag.

![ba-dum-base](/assets/images/writeup/ba-dum-base-flag.png)

### Minecraft

given image file. looks like this,

![Minecraft.png](/assets/images/writeup/minecraft.png.png)

after displaying the image file look like that. I try the simple dumping strings 
in the file.

```shell
strings Minecraft.png
```

![mincraft-flag](/assets/images/writeup/minecraft-flag.png)

at the bottom, I see the flag.

### Sejarah WWW

> Pengembangan WWW dimulai pada 1989 oleh Tim Berners-Lee dan
rekan-rekannya yang berbasis di Jenewa, Swiss, pada bulan november 1990
Nicola Pellow bergabung dan mengerjakan line-mode Browser, cari tau nama
rekan yang membuat interface CERNVM "FIND",

yeah, OSINT challenge!! the objective of the question is who the creator of 
interface CERNVM "FIND". Let’s find it out of our search engine. first, I try 
from wiki site. with the simple keyword ``creator of interface CERNVM "FIND"``.

![wiki](/assets/images/writeup/wiki-www.png)

not enough relevant yet. let's try with another keyword. for example history of 
interface CERNVM "FIND"

![www-flag](/assets/images/writeup/osint-www.png)

I saw the creator on the top site Bernd Pollermann. Let's put in format flag.

```text
LastCTF{Bernd Pollermann}
```

### Docker Rescue

next, is one of the interesting challenges. Docker Rescue we should dumping file 
in docker image in /var/lastctf/flag.txt but, if we ran the docker image flag 
would be removed.

let's pull the image from the docker hub https://hub.docker.com/r/wenkhairu/lastctf. 
before pull we can see the layer of the docker image, at the last layer flag is 
removed.

![docker hub](/assets/images/writeup/docker-layer.png)

```bash
docker pull wenkhairu/lastctf
```
and try to convert into tar format, and we can extract everything in the image.

```shell
docker image save wenkhairu/lastctf:latest -o flag.tar
```

extract tar file 

```shell
tar -xf flag.tar
```

![extracted docker image](/assets/images/writeup/docker-extracted.png)

let us inspect manifest.json files where contained manifest the image.

![manifest.json](/assets/images/writeup/docker-manifest.png)

The first 2 layer is the base docker layer, and the rest is the Dockerfile layer. 
we can assume the layer contained copy command in layer 5 with hash sha 65ec...
lets extract the layer.

```shell
tar -xf 65ec1f23eb207d60889555603041f8a10166e37e3867280c452fd12982a6f0de/layer.tar
```

![flag](/assets/images/writeup/docker-flag.png)

and then we can capture the flag because we take state before the layer that 
removes the flag.

### Pernet

given a zip file containing the question.zip flag.zip and hint.txt in it, if we 
try to unzip the question.zip file it will cause an error because the file has 
the extension 7z (7zip).

![](/assets/images/writeup/pernet-zip.png)

after we extracted, found hint.txt and containing hint

![](/assets/images/writeup/pernet-hing.png)

the hint instructed to brute force soal.zip with number range 0 - 100000. I use 
python to create a num list that is used for brute force attack.

![](/assets/images/writeup/pernet-numlist-py.png)

I use tools from Github to brute force 7zip file, you can access the tool here 
https://github.com/Seyptoo/7z-BruteForce and then I ran tools with numlist that 
was created.

![](/assets/images/writeup/pernet-found-pass.png)

tools success found the password. and let's extract soal.zip with the password 
we found. there is a file containing ciphertext and key, hint says it is pernet. 
so let's fernet decoder online here https://asecuritysite.com/encryption/ferdecode.

![](/assets/images/writeup/pernet-key-chiper.png)

paste key and ciphertext and decode. we found a number and assume that is the 
next password for flag.zip and pwned!

![](/assets/images/writeup/pernet-flag-pass.png)

### Lambe Rekta

Given file called runme.txt that contains text below.

![](/assets/images/writeup/lambe-runme.png)

I assume that this is a file containing prints of 0 and 1 with a lot of whitespace 
in it, if we look at the 0 and 1 on each line containing 8 bits, we can assume 
that this is binary ASCII then I try to parse it using python.

![](/assets/images/writeup/lambe-parsing.png)

next, we have parsed binary here, let's decode to ASCII.

![](/assets/images/writeup/lambe-binary.png)

I use binary to ASCII online, you can visit here https://www.rapidtables.com/convert/number/binary-to-ascii.html

![](/assets/images/writeup/lambe-flag.png)

and boom PWNED!               

then I was curious about what the flag said, “whitelips programming”, after I 
searched on my duckduckgo, I got this website https://vii5ard.github.io/whitespace/ 
after I tried pasting the contents of the runme.txt text and then I got the flag, 
hahaha.. to be honest, I just heard about whitespace programming, it’s unique

![](/assets/images/writeup/lambe-whitelips.png)

### Para Penggila Musik (Osint)

The objective of this challenge is to Search Deto Agung’s comment that fans with 
clean bandit. I try a simple keyword in the google search bar, Deto Agung 
"Clean Bandit".

![](/assets/images/writeup/para-penggila-musik.png)

in the top site, I found a site from sound cloud, and visit the website. In the 
comment section, I found Deto Agung’s comment and there is a flag.

![](/assets/images/writeup/para-penggila-musik-flag.png)
