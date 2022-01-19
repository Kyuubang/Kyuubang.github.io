---
title: "CTF: LastCTF Helixsid"
tag:
- docker
- osint
- fernet
imagepreview: https://user-images.githubusercontent.com/56214296/150106770-c98d66bc-4653-4cf1-a5e3-ad0345d1bb55.png
---


Few days ago i joining the CTF competition called LastCTF hosted by Helixsid community held from 25 December 2021 
until 11 January 2022. I think this is good for filling free time during school holidays. Mechanism the competition is 
jeopardy. the challenge would be spawned regularly. 

i succeed 7 flag, and reach top 13 the competition. Not bad a start, keep it up!

![lastctf preview](/assets/images/preview/lastctf-thumb.png)

> **Notice** <br />
> The information provided in this site is for educational purposes regarding
> pentesting. The author of the site will not be held any responsibility for any
> misuse of the information from this site.

#### Summary

* [Ba Dum Base](### Ba Dum Base)
* [Minecraft]
* [Sejarah WWW]
* [Docker Rescue]
* [Pernet]
* [Lambe Rekta]
* [Para Penggila Musik (Osint)]

### Ba Dum Base

given text file contains a collection of text that looks like forming letters. and try concatenate the file

![ba-dum-base](/assets/images/writeup/ba-dum-base.png)

after guessing every character, i found this text. look good for base64 format. i try to decode as base64, and yeah i 
found the flag.

![ba-dum-base](/assets/images/writeup/ba-dum-base-flag.png)

### Minecraft

given image file. 

![Minecraft.png](/assets/images/writeup/minecraft.png.png)

after displayed the image file look like that. i try the simple dumping strings in the file.

```shell
strings Minecraft.png
```

![mincraft-flag](/assets/images/writeup/minecraft-flag.png)

at the bottom i see the flag.

### Sejarah WWW

> Pengembangan WWW dimulai pada 1989 oleh Tim Berners-Lee dan
rekan-rekannya yang berbasis di Jenewa, Swiss, pada bulan november 1990
Nicola Pellow bergabung dan mengerjakan line-mode Browser, cari tau nama
rekan yang membuat interface CERNVM "FIND",

yeah osint challenge!! the objective from the question is `who creator of interface CERNVM "FIND"`. Let's find it out 
our search engine. first i try from wiki site. with the simple keyword `creator of interface CERNVM "FIND"`

![wiki](/assets/images/writeup/wiki-www.png)

not relevant yet. lets try with another keyword. for example `history of interface CERNVM "FIND"`

![www-flag](/assets/images/writeup/osint-www.png)

i saw the creator in the top site Bernd Pollermann. lets put in format flag.

```text
LastCTF{Bernd Pollermann}
```

### Docker Rescue

next, one of the interested challenge. Docker Rescue we should dumping file in docker image in `/var/lastctf/flag.txt` 
but, if we running the docker image flag would be removed. 

lets pull image from docker hub https://hub.docker.com/r/wenkhairu/lastctf. before pull we can see the layer of docker 
image, at last layer flag is removed.

![docker hub](/assets/images/writeup/docker-layer.png)

```bash
docker pull wenkhairu/lastctf
```

and try to convert into tar format, and we can extract everyhing in the image.

```shell
docker image save wenkhairu/lastctf:latest -o flag.tar
```

extract tar file 

```shell
tar -xf flag.tar
```

![extracted docker image](/assets/images/writeup/docker-extracted.png)

lets inspect manifest.json files where contained manifest the image.

![manifest.json](/assets/images/writeup/docker-manifest.png)

first 2 layer is base docker layer, and the rest is Dockerfile layer. we can assume layer contained copy command in 
layer 5 with hash sha 65ec..

lets extract the layer.

```shell
tar -xf 65ec1f23eb207d60889555603041f8a10166e37e3867280c452fd12982a6f0de/layer.tar
```

![flag](/assets/images/writeup/docker-flag.png)

and then we can the flag, because we take state before layer that remove the flag.

### Pernet

given a zip file containing the question.zip flag.zip and hint.txt in it, if we try to unzip the question.zip file it 
will cause an error, because the file actually has the extension 7z (7zip).

![](/assets/images/writeup/pernet-zip.png)

after we extracted, found hint.txt and containing hint

![](/assets/images/writeup/pernet-hing.png)

instructed to brute force soal.zip with number range 0 - 100000. i use python to create numlist that used for brute 
force attack.

![](/assets/images/writeup/pernet-numlist-py.png)

i use tools from github to brute force 7zip file, you can access
tool here https://github.com/Seyptoo/7z-BruteForce. 

and then i ran tools with numlist that created.

![](/assets/images/writeup/pernet-found-pass.png)

tools success found the password. and lets extract soal.zip with the password we found. there is a file containing chiper 
text and key, hint say it is pernet. so lets fernet decoder online here https://asecuritysite.com/encryption/ferdecode.

![](/assets/images/writeup/pernet-key-chiper.png)

paste key and chiper and decode. we found number and assume that is a next password for flag.zip 

![](/assets/images/writeup/pernet-flag-pass.png)

and pwned!

### Lambe Rekta

Given file called runme.txt that contain text below.

![](/assets/images/writeup/lambe-runme.png)

I assume that this is a file containing prints of 0 and 1 with a lot of whitespace in it, if we look at the 0 and 1 on each 
line containing 8 bits, we can assume that this is binary ascii then I try to parse it using python.

![](/assets/images/writeup/lambe-parsing.png)

next we have parsed binary here, lets decode to ascii.

![](/assets/images/writeup/lambe-binary.png)

i use binary to ascii online, you can visit here https://www.rapidtables.com/convert/number/binary-to-ascii.html

![](/assets/images/writeup/lambe-flag.png)

and boom PWNED!               

then I was curious about what the flag said, "whitelips programming", after I searched on my duckduckgo, I got this 
website https://vii5ard.github.io/whitespace/ after I tried pasting the contents of the runme.txt text and then I got 
the flag, hahaha.. to be honest, I just heard about whitespace programming, it's quite unique

![](/assets/images/writeup/lambe-whitelips.png)

### Para Penggila Musik (Osint)

The objective in this challenge is  Search Deto Agung's comment that fans with clean bandit. i try simple keyword 
in google search bar, `Deto Agung "Clean Bandit"`.

![](/assets/images/writeup/para-penggila-musik.png)

in top site, i found site from sound cloud, and visit the website. In the comment section i found Deto Agung's comment
and there is a flag.

![](/assets/images/writeup/para-penggila-musik-flag.png)
