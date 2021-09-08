---
title : "CTF: LKSN CyberSecurity 2020"
toc   : true
categories:
    - ctf
    - writeup
---

## Forensics

### File Signature

| Challenge : File Signature                                                                                                                                                                                            |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| diberikan satu file dengan ekstensi yang belum jelas yaitu`flag.jpg.exe.doc.pdf`. kita diperintahkan untuk mencari ekstensi file yang sebenarnya dan jika perlu di recovery agar dapat dibuka dan mendapatkan flagnya |
| **Answer : LKSSMK28{Yuu-kita-ke-jakarta}**                                                                                                                                                                            |

1. pertama, untuk mencegah kerusakan file kami membackup file dan mencari hashnya terlebih dahulu dan mengisolasinya  
   
   ```bash
   [demo@demo] on ~/…/LKSN-CyberSecurity-2020/CTF/FORENSIC/file signature
   $ tree ori && cat ori/SHA1SUM.txt 
   ori
   ├── flag.jpg.exe.doc.pdf
   └── SHA1SUM.txt
   
   0 directories, 2 files
   50b75ee2d3d5b4619f3cffe7d02dbed278c877f4  flag.jpg.exe.doc.pdf
   ```

2. pertama, cari type file dengan menggunakan `file` command untuk mencari tau type filenya
   
   ```bash
   [demo@demo] on ~/…/LKSN-CyberSecurity-2020/CTF/FORENSIC/file signature
   $ file flag.jpg.exe.doc.pdf 
   flag.jpg.exe.doc.pdf: data
   ```

3. selanjutnya kami mencoba melihat hex file dengan `xxd`
   
   ```bash
   $ xxd flag.jpg.exe.doc.pdf | head   
   00000000: 0000 0000 0010 4a46 4946 0001 0101 0090  ......JFIF......
   00000010: 0090 0000 ffe1 005e 4578 6966 0000 4d4d  .......^Exif..MM
   00000020: 002a 0000 0008 0005 0112 0003 0000 0001  .*..............
   00000030: 0001 0000 0302 0002 0000 000c 0000 004a  ...............J
   00000040: 5110 0001 0000 0001 0100 0000 5111 0004  Q...........Q...
   00000050: 0000 0001 0000 1625 5112 0004 0000 0001  .......%Q.......
   00000060: 0000 1625 0000 0000 4943 4320 5072 6f66  ...%....ICC Prof
   00000070: 696c 6500 ffe2 0e74 4943 435f 5052 4f46  ile....tICC_PROF
   00000080: 494c 4500 0101 0000 0e64 6170 706c 0210  ILE......dappl..
   00000090: 0000 6d6e 7472 5247 4220 5859 5a20 07e2  ..mntrRGB XYZ ..
   ```

4. dilihat dari header hex formatnya file itu merupakan JPG file, lalu kami mengeditnya menggunakan hexeditor 2 blok hexa pertama menjadi `ffd8 ffe0`
   
   | Before                                                                                                    | After                                                                                                     |
   | --------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
   | <img title="" src="file:///home/bayhaqi/Documents/CyberSecurity/CTF/file-signature/hex-bfore.png" alt=""> | <img title="" src="file:///home/bayhaqi/Documents/CyberSecurity/CTF/file-signature/hex-after.png" alt=""> |

5. lalu ganti ekstensi file menjadi `.jpg` lalu buka image nya <mark>`#PWNED`</mark>
   
   <img title="" src="file:///home/bayhaqi/Documents/CyberSecurity/CTF/file-signature/foto.png" alt="">

### md5 and metadata file

| Challenge : metadata and md5 file                                                                                                                      |
| ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| diberikan satu file gambar dengan ekstensi file `png` kami diperintahkan untuk mencari mcat d5 hash dan location dari file tersebut                    |
| **Answer : <br/>LKSSMK28{c0b7d53ada2ad6858df4ada15f40b550}<br/>LKSSMK28{Cibogor, Kecamatan Bogor Tengah, Kota Bogor, Jawa Barat 16124_stasiun bogor}** |

1. untuk mendapat kan md5sum dari file kami meggunakan `md5sum` 
   
   ```bash
   $ md5sum 2018-09-22\ 09.31.07.jpg
   c0b7d53ada2ad6858df4ada15f40b550  2018-09-22 09.31.07.jpg
   ```

2. untuk mendapatkan detail location pada file kami menggunkan `exiftool` untuk melihat metadata yang ada
   
   ```bash
   $ exiftool 2018-09-22\ 09.31.07.jpg 
   ExifTool Version Number         : 12.13
   File Name                       : 2018-09-22 09.31.07.jpg
   Directory                       : .
   File Size                       : 1319 KiB
   File Modification Date/Time     : 2018:09:22 02:31:08+07:00
   File Access Date/Time           : 2021:01:06 13:16:05+07:00
   File Inode Change Date/Time     : 2021:01:01 22:19:52+07:00
   File Permissions                : rw-r--r--
   File Type                       : JPEG
   File Type Extension             : jpg
   MIME Type                       : image/jpeg
   Exif Byte Order                 : Big-endian (Motorola, MM)
   Resolution Unit                 : inches
   Make                            : LGE
   Camera Model Name               : Nexus 5X
   Software                        : bullhead-user 8.1.0 OPM6.171019.030.K1 4947289 release-keys
   Modify Date                     : 2018:09:22 09:31:08
   Orientation                     : Horizontal (normal)
   Y Cb Cr Positioning             : Centered
   ISO                             : 50
   Exposure Program                : Not Defined
   F Number                        : 2.0
   Exposure Time                   : 1/237
   Sensing Method                  : Unknown (0)
   Sub Sec Time Digitized          : 403858
   Sub Sec Time Original           : 403858
   Sub Sec Time                    : 403858
   Subject Distance Range          : Distant
   Focal Length                    : 2.6 mm
   Flash                           : No Flash
   Metering Mode                   : Unknown
   Scene Capture Type              : Standard
   Interoperability Index          : R98 - DCF basic file (sRGB)
   Interoperability Version        : 0100
   Focal Length In 35mm Format     : 0 mm
   Create Date                     : 2018:09:22 09:31:08
   Exposure Compensation           : 0
   Exif Image Height               : 1944
   White Balance                   : Auto
   Date/Time Original              : 2018:09:22 09:31:08
   Brightness Value                : 0
   Exif Image Width                : 2592
   Exposure Mode                   : Auto
   Aperture Value                  : 2.0
   Components Configuration        : Y, Cb, Cr, -
   Color Space                     : sRGB
   Scene Type                      : Unknown (0)
   Shutter Speed Value             : 235.6
   Exif Version                    : 0220
   Flashpix Version                : 0100
   GPS Latitude Ref                : South
   GPS Longitude Ref               : East
   GPS Altitude Ref                : Above Sea Level
   GPS Time Stamp                  : 02:30:55
   GPS Date Stamp                  : 2018:09:22
   X Resolution                    : 72
   Y Resolution                    : 72
   Profile CMM Type                : 
   Profile Version                 : 4.0.0
   Profile Class                   : Display Device Profile
   Color Space Data                : RGB
   Profile Connection Space        : XYZ
   Profile Date Time               : 2016:12:08 09:38:28
   Profile File Signature          : acsp
   Primary Platform                : Unknown ()
   CMM Flags                       : Not Embedded, Independent
   Device Manufacturer             : Google
   Device Model                    : 
   Device Attributes               : Reflective, Glossy, Positive, Color
   Rendering Intent                : Perceptual
   Connection Space Illuminant     : 0.9642 1 0.82491
   Profile Creator                 : Google
   Profile ID                      : 75e1a6b13c34376310c8ab660632a28a
   Profile Description             : sRGB IEC61966-2.1
   Profile Copyright               : Copyright (c) 2016 Google Inc.
   Media White Point               : 0.95045 1 1.08905
   Media Black Point               : 0 0 0
   Red Matrix Column               : 0.43604 0.22249 0.01392
   Green Matrix Column             : 0.38512 0.7169 0.09706
   Blue Matrix Column              : 0.14305 0.06061 0.71391
   Red Tone Reproduction Curve     : (Binary data 32 bytes, use -b option to extract)
   Chromatic Adaptation            : 1.04788 0.02292 -0.05019 0.02959 0.99048 -0.01704 -0.00922 0.01508 0.75168
   Blue Tone Reproduction Curve    : (Binary data 32 bytes, use -b option to extract)
   Green Tone Reproduction Curve   : (Binary data 32 bytes, use -b option to extract)
   Image Width                     : 2592
   Image Height                    : 1944
   Encoding Process                : Baseline DCT, Huffman coding
   Bits Per Sample                 : 8
   Color Components                : 3
   Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
   Aperture                        : 2.0
   Image Size                      : 2592x1944
   Megapixels                      : 5.0
   Shutter Speed                   : 1/237
   Create Date                     : 2018:09:22 09:31:08.403858
   Date/Time Original              : 2018:09:22 09:31:08.403858
   Modify Date                     : 2018:09:22 09:31:08.403858
   GPS Altitude                    : 274 m Above Sea Level
   GPS Date/Time                   : 2018:09:22 02:30:55Z
   GPS Latitude                    : 6 deg 35' 44.28" S
   GPS Longitude                   : 106 deg 47' 25.47" E
   Focal Length                    : 2.6 mm
   GPS Position                    : 6 deg 35' 44.28" S, 106 deg 47' 25.47" E
   Light Value                     : 10.9
   ```

3. terlihat pada output bahwa foto memiliki detail geografi, untuk mencari tau dimana tepatnya tempat tersebut kita dapat menggunakan google [maps](maps.google.com) atau sejenisnya, untuk media command line kita dapat menggunakan
   
   ```bash
   GPS Altitude                    : 274 m Above Sea Level
   GPS Date/Time                   : 2018:09:22 02:30:55Z
   GPS Latitude                    : 6 deg 35' 44.28" S
   GPS Longitude                   : 106 deg 47' 25.47" E
   Focal Length                    : 2.6 mm
   GPS Position                    : 6 deg 35' 44.28" S, 106 deg 47' 25.47" E
   ```

### Image pass

| Challenge : URL IMAGE                                                                                                                                                                                                            |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Selain memperhatikan jenis file gambar.<br/>Para IT juga diharuskan mengecek data image dengan detail.<br/>terdapat clue pada soal tentang url image jadi kita akan fokus untuk mencari url yang tersembunyi pada image tersebut |
| **Answer : LKSSMK28{https://pastebin.com/kwfWbHcb}**                                                                                                                                                                             |

1. kami melakukan prosedur untuk investigasi forensik, yaitu membackup file terlebih dahulu dan mengambil hash dari file tersebut

2. kami memeriksa type file dan metadata pada file tersebut
   
   ```bash
   $ file image_password.jpg; exif image_password.jpg
   image_password.jpg: JPEG image data, JFIF standard 1.01, resolution (DPI), 
   density 96x96, segment length 16, Exif Standard: [TIFF image data, big-endian, 
   direntries=3, copyright=https://pastebin.com/kwfWbHcb], baseline, precision 8, 
   1460x958, components 3
   EXIF tags in 'image_password.jpg' ('Motorola' byte order):
   --------------------+----------------------------------------------------------
   Tag                 |Value
   --------------------+----------------------------------------------------------
   Copyright           |https://pastebin.com/kwfWbHcb (Photographer) - [None] (Edi
   Padding             |2060 bytes undefined data
   X-Resolution        |72
   Y-Resolution        |72
   Resolution Unit     |Inch
   Padding             |2060 bytes undefined data
   Exif Version        |Exif Version 2.1
   FlashPixVersion     |FlashPix Version 1.0
   Color Space         |Uncalibrated
   --------------------+----------------------------------------------------------
   ```

3. untuk memastikan tidak ada url lagi yang terdapat pada gambar kami memeriksa hexa pada file gambar tersebut, kami menemukan url yang sama pada metadata
   
   ```bash
   $ xxd image_password.jpg
   ...
   ...
   00000850: 0000 0000 0000 0000 0000 0000 6874 7470  ............http
   00000860: 733a 2f2f 7061 7374 6562 696e 2e63 6f6d  s://pastebin.com
   00000870: 2f6b 7766 5762 4863 6200 0001 ea1c 0007  /kwfWbHcb.......
   ...
   ...
   ```

4. dikarenakan url nya mati jadi kami tidak bisa membukanya

## MISC

### QRCode

| Challenge : find flag on qrcode                                                                                                                                                                             |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Kode QR atau biasa dikenal dengan istilah QR Code adalah bentuk evolusi kode batang dari satu dimensi menjadi dua dimensi.<br/><br/>Terdapat pesan rahasia enkripsi Binary yang terdapat di file qrC0d3.png |
| **Answer : LKSSMK28{IT_s3cuR1Ty_2020}**                                                                                                                                                                     |

1. kami mencoba membuka gambar nya dengan imagemagick, terlihat gambar qrcode yang cukup besar, kemungkinan isinya akan panjang
   
   <img title="" src="file:///home/bayhaqi/Documents/CyberSecurity/CTF/qrcode/qrcode.png" alt="" data-align="center">

2. lalu kami mecoba membaca isinya menggunakan zebra bar `zbarimg` dan sedikit script python yang saya tulis <mark>`#PWNED`</mark>
   
   ```bash
   $ zbarimg qrC0d3.png 2> /dev/null | cut -d ':' -f2 > qr.txt; ./script.py qr.txt
   Keamanan komputer atau dikenal juga dengan sebutan cybersecurity atau 
   IT security adalah keamanan informasi yang diaplikasikan kepada komputer dan 
   jaringannya. Keamanan komputer bertujuan membantu pengguna LKSSMK28{IT_s3cuR1Ty_2020} 
   agar dapat mencegah penipuan atau mendeteksi adanya usaha penipuan di sebuah sistem yang 
   ```

## Reversing

### Base64

| Challenge : Simple Encrypt                                                                                                                                                                                                              |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Gabungkan Enkripsi di bawah ini :<br/>                                                                                                                                          <br/>TEtTU01LMjh7Y<br/>mFzZTY0X0VuY3<br/>J5cHQxMDBufQ== |
| **Answer : LKSSMK28{base64_Encrypt100n}**                                                                                                                                                                                               |

1. terlihat dari judulnya base64 maka dari itu kami mencoba menggunakan algorithm base64 untuk mendecode  <mark>`#PWNED`</mark>
   
   ```bash
   $ echo "TEtTU01LMjh7YmFzZTY0X0VuY3J5cHQxMDBufQ==" | base64 -d
   LKSSMK28{base64_Encrypt100n}
   ```

### PDF CRACK

| Challenge : Crack PDF file                                                                                                                                                                |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| hai teman-teman semua.<br/>terdapat file yang harus di crack untuk mendapatkan Flag <br/>Teliti terlebih dahulu file yang dikirim sebelum teman-teman mendapatkan file PDF untuk di crack |
| **Answer : LKSSMK28{crack1n9_docum3nT}**                                                                                                                                                  |

1. kami mengecek type file yang di berikan yaitu `file.exe` 
   
   ```bash
   $ file file.exe 
   file.exe: Zip archive data, at least v2.0 to extract
   ```

2. dikarekan file tersebut adalah file zip lalu kami mengekstrak nya 
   
   ```bash
   $ mv file.exe file.zip
   
   $ unzip file.zip                         
   Archive:  file.zip
     inflating: rockyou-20.txt          
      creating: __MACOSX/
     inflating: __MACOSX/._rockyou-20.txt  
     inflating: file_secret.pdf         
     inflating: __MACOSX/._file_secret.pdf
   
   $ ls
   file_secret.pdf  file.zip  __MACOSX  rockyou-20.txt
   ```

3. terlihat terdapat satu file pdf `file_secret.pdf` dan list word `rockyou-20.txt`, setelah itu kami mencoba membuka `file_secret.pdf` dan bisa dilihat bahwa ini dikunci
   
   <img title="" src="file:///home/bayhaqi/Documents/CyberSecurity/CTF/pdf-crack/password.png" alt="">

4. untuk membuka filenya kami mencoba menggunakan `pdfcrack` dengan wordlist yang sudah diberikan `rockyou-20.txt` 
   
   ```bash
   $ pdfcrack -f file_secret.pdf -w rockyou-20.txt 
   PDF version 1.6
   Security Handler: Standard
   V: 2
   R: 3
   P: -4
   Length: 128
   Encrypted Metadata: True
   FileID: 462671d75db518e9e2ee11c6c2596a5d
   U: b8c415bc865170c8b909b869a36df3e700000000000000000000000000000000
   O: 3bc4b947d0559c5a443d7d23d3773959bdc41c9b77a5fba3bb488b9f2ca0bc17
   found user-password: 'hellokitty'
   ```

5. setelah mendapatkan passwordnya selanjutnya kami, membukanya dan di temukan teks terenkripsi kemungkinan menggunakan base64 
   
   <img title="" src="file:///home/bayhaqi/Documents/CyberSecurity/CTF/pdf-crack/isi-pdf.png" alt="">

6. kami mencoba mendecrypt teks tersebut, dan di temukan flagnya! <mark>`#PWNED`</mark>
   
   ```bash
   $ echo "TEtTU01LMjh7Y3JhY2sxbjlfZG9jdW0zblR9==" | base64 -d
   LKSSMK28{crack1n9_docum3nT}
   ```

### Reverse 1

| Challenge : Reverse 1                                                                                       |
| ----------------------------------------------------------------------------------------------------------- |
| diberikan satu file bertype `ELF` kami diperintahkan untuk mereverse file tersebut agar mendapatkan flagnya |
| **Answer : LKSSMK28{01c9fsd3gt34zxxcb0eb8a42d3c534rf3c570703e3t}**                                          |

1. pertama kami mencari tau type file dari yang di berikan 
   
   ```bash
   $ file reverse1 
   reverse1: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), 
   dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, 
   BuildID[sha1]=73dfc3067c6b6a300c90ed9c29f3925c9d2f20a7, for 
   GNU/Linux 3.2.0, not stripped
   ```

2. saat kami memberikan execute permissiion untuk mengeksekusi file tersebut
   
   ```bash
   $ chmod a+x ./reverse1
   ```

3. setelah itu kami mencoba mengeksekusinya
   
   ```bash
   $ ./reverse1         
   
                .--``..---.                
    .CTF:`    .LKS-SMK28` 
                `.--..`          
   
   CTF LKS SMK28
    password:> password
    SMK BISA! 
   ```

4. sepertinya kami harus mencari passwordnya, kami menggunakan `ltrace` untuk mentracing library tersebut
   
   ```bash
   $ ltrace ./reverse1  
   _ZNSt8ios_base4InitC1Ev(0x5604e12382b9, 0xffff, 0x7fff7d84b2f8, 224) = 0
   __cxa_atexit(0x7f23c9f64a40, 0x5604e12382b9, 0x5604e1238060, 6) = 0
   strcpy(0x7fff7d84b0a3, "k0o")                   = 0x7fff7d84b0a3
   strcat("k0o", "pi_h")                           = "k0opi_h"
   _ZNSolsEPFRSoS_E(0x5604e1238080, 0x7f23c9fd36d0, 4, 0x685f6970
   ) = 0x5604e1238080
   _ZStlsISt11char_traitsIcEERSt13basic_ostreamIcT_ES5_PKc(0x5604e1238080, 0x5604e1236010, 0, 3072) = 0x5604e1238080
   _ZNSolsEPFRSoS_E(0x5604e1238080, 0x7f23c9fd36d0, 0x5604e1238080, 3072             .--``..---.                
   ) = 0x5604e1238080
   _ZStlsISt11char_traitsIcEERSt13basic_ostreamIcT_ES5_PKc(0x5604e1238080, 0x5604e1236039, 0, 3072) = 0x5604e1238080
   _ZNSolsEPFRSoS_E(0x5604e1238080, 0x7f23c9fd36d0, 0x5604e1238080, 3072 .CTF:`    .LKS-SMK28` 
   ) = 0x5604e1238080
   _ZStlsISt11char_traitsIcEERSt13basic_ostreamIcT_ES5_PKc(0x5604e1238080, 0x5604e1236058, 0, 3072) = 0x5604e1238080
   _ZNSolsEPFRSoS_E(0x5604e1238080, 0x7f23c9fd36d0, 0x5604e1238080, 3072             `.--..`          
   ) = 0x5604e1238080
   _ZNSolsEPFRSoS_E(0x5604e1238080, 0x7f23c9fd36d0, 0x5604e1238080, 3072
   ) = 0x5604e1238080
   _ZStlsISt11char_traitsIcEERSt13basic_ostreamIcT_ES5_PKc(0x5604e1238080, 0x5604e1236077, 0, 3072) = 0x5604e1238080
   _ZNSolsEPFRSoS_E(0x5604e1238080, 0x7f23c9fd36d0, 0x5604e1238080, 0x38324b4d5320534bCTF LKS SMK28
   ) = 0x5604e1238080
   _ZStlsISt11char_traitsIcEERSt13basic_ostreamIcT_ES5_PKc(0x5604e1238080, 0x5604e1236085, 0, 3072) = 0x5604e1238080
   _ZStrsIcSt11char_traitsIcEERSt13basic_istreamIT_T0_ES6_PS3_(0x5604e12381a0, 0x7fff7d84b0e0, 0x7f23ca0733d0, 0x203e3a64726f7773 password:>  <no return ...>
   --- SIGWINCH (Window changed) ---
   --- SIGWINCH (Window changed) ---
   --- SIGWINCH (Window changed) ---
   --- SIGWINCH (Window changed) ---
   k0opi_h
   <... _ZStrsIcSt11char_traitsIcEERSt13basic_istreamIT_T0_ES6_PS3_ resumed> ) = 0x5604e12381a0
   strcat("k0opi_h", "ita")                        = "k0opi_hita"
   strcat("k0opi_hita", "m_pht")                   = "k0opi_hitam_pht"
   strcmp("k0opi_h", "k0opi_hitam_pht")            = -105
   _ZStlsISt11char_traitsIcEERSt13basic_ostreamIcT_ES5_PKc(0x5604e1238080, 0x5604e123609f, 105, 0x68705f6d) = 0x5604e1238080
   _ZNSolsEPFRSoS_E(0x5604e1238080, 0x7f23c9fd36d0, 0x5604e1238080, 0x202141534942204b SMK BISA! 
   ) = 0x5604e1238080
   +++ exited (status 0) +++
   ```

5. dan kami tertarik untuk mencoba string yang terdapat pada fungsi `strcat` lalu kami coba masukkan string tersebut dan kami mendapatkan flagnya! <mark>`#PWNED`</mark>
   
   ```bash
   $ ./reverse1   
   
                .--``..---.                
    .CTF:`    .LKS-SMK28` 
                `.--..`          
   
   CTF LKS SMK28
    password:> k0opi_hitam_pht
   
   LKSSMK28{01c9fsd3gt34zxxcb0eb8a42d3c534rf3c570703e3t} 
   ```

### Reverse 2

| Challenge : Reverse 2                                                                                       |
| ----------------------------------------------------------------------------------------------------------- |
| diberikan satu file bertype `ELF` kami diperintahkan untuk mereverse file tersebut agar mendapatkan flagnya |
| **Answer : <br/>LKSSMK28{r3v3rs1ng}<br/>LKSSMK28{LKSSMK28_486619254b9c9f6e6800cfae77}**                     |

1. seperti biasa kami memeriksa type file terlebih dahulu untuk memastikan
   
   ```bash
   $ file reverse2 
   reverse2: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), 
   dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, 
   BuildID[sha1]=20f01148e7568ccfd9e0162018d8ca8111e419c4, for 
   GNU/Linux 3.2.0, not stripped
   ```

2. lalu kami mencoba melihat isi file menggunakan `strings` dan didapatkan satu flag! <mark>`#PWNED`</mark>
   
   ```bash
   $ strings ./reverse2 
   /lib64/ld-linux-x86-64.so.2
   libc.so.6
   __isoc99_scanf
   puts
   putchar
   printf
   __cxa_finalize
   strcmp
   __libc_start_main
   GLIBC_2.7
   GLIBC_2.2.5
   _ITM_deregisterTMCloneTable
   __gmon_start__
   _ITM_registerTMCloneTable
   u/UH
   0x00007fH
   trolf
   p4ssw0rdH
   LKSSMK28H
   yoik, yoH
   ur flag H
   is: LKSSH
   MK28{r3vH
   3rs1ng}
   %16s
   48661925H
   4b9c
   You FailH
   []A\A]A^A_
   ...
   ...
   ...
   ```

3. kami tertarik dengan kata yang di temukan ketika kami menggunakan `strings` yaitu `p4ssw0rd`. karena kami masih penasaran dengan isi file yang sebenarnya jadi kami mencoba menggunakan `ltrace` kembali
   
   ```bash
   $  ltrace ./reverse2    
   puts("||=============================="...||====================================================================||
   )     = 73
   puts("||//////////////////////////////"...||////////////////////////////////////////////////////////////////////||
   )     = 73
   puts("||()========================| CT"...||()========================| CTF |==========================()||
   )     = 66
   puts("||()======================LKS SM"...||()======================LKS SMK 28=========================()||
   )     = 66
   puts("||//////////////////////////////"...||////////////////////////////////////////////////////////////////////||
   )     = 73
   puts("||=============================="...||====================================================================||
   )     = 73
   puts("Password:"Password:
   )                               = 10
   __isoc99_scanf(0x556fd5dab13c, 0x7ffe756f6920, 0, 0x7fea1e3c8ed3) = 1
   strcmp("0x00007fff", "puts("||========")        = -64
   puts("You Failed"You Failed
   )                              = 11
   +++ exited (status 0) +++
   ```

4. kami tertarik dengan isi fungsi `strcmp` terdapat kata yang sepertinya di compare untuk mencocokan password jadi kami mencobanya dan alhamdulillah berhasil! <mark>`#PWNED`</mark>
   
   ```bash
   $ ./reverse2 
   ||====================================================================||
   ||////////////////////////////////////////////////////////////////////||
   ||()========================| CTF |==========================()||
   ||()======================LKS SMK 28=========================()||
   ||////////////////////////////////////////////////////////////////////||
   ||====================================================================||
   Password:
   0x00007fff
   You Win
   LKSSMK28{LKSSMK28_486619254b9c9f6e6800cfae77}  
   ```

### Reverse 3

| Challenge : Reverse 3                                                                                       |
| ----------------------------------------------------------------------------------------------------------- |
| diberikan satu file bertype `ELF` kami diperintahkan untuk mereverse file tersebut agar mendapatkan flagnya |
| **Answer : LKSSMK28{JJJJJJJJJJJJJJBxs}**                                                                    |

1. seperti biasa kami memastikan type file terlebih dahulu untuk memastikan
   
   ```bash
   $ file crackme 
   crackme: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), 
   dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, 
   BuildID[sha1]=9283bfe84e4b274b954a775f733a5c9de669675a, for 
   GNU/Linux 3.2.0, not stripped
   ```

2. lalu kami mencoba melihat isi file tersebut menggunakan `strings` terlihat sepertinya flag harus di reverse 
   
   ```bash
   $ strings crackme       
   /lib64/ld-linux-x86-64.so.2
   NK'K
   Jw_s:\
   mgUa
   libc.so.6
   puts
   stdin
   printf
   fgets
   memset
   malloc
   __cxa_finalize
   strcmp
   __libc_start_main
   free
   GLIBC_2.2.5
   _ITM_deregisterTMCloneTable
   __gmon_start__
   _ITM_registerTMCloneTable
   u/UH
   []A\A]A^A_
   Input Your Password
   Please try later.
   MANTUL, flag is LKSSMK28{s}
   Password Salah!
   ;*3$"
   sGCC: (Debian 9.2.1-19) 9.2.1 20191109
   crtstuff.c
   deregister_tm_clones
   ....
   ....
   ....
   ```

3. selanjutnya kami menggunakan `ltrace` untuk mentracing file tersebut
   
   ```bash
   $ ltrace ./crackme 
   puts("Hi!\nInput Your Password"Hi!
   Input Your Password
   )                = 24
   malloc(18)                                      = 0x5565bf7216b0
   memset(0x5565bf7216b0, '\0', 18)                = 0x5565bf7216b0
   fgets(password
   "password\n", 18, 0x7f14edf29980)         = 0x5565bf7216b0
   strcmp("password\n", "JJJJJJJJJJJJJJBxs")       = 38
   puts("Password Salah!"Password Salah!
   )                         = 16
   free(0x5565bf7216b0)                            = <void>
   +++ exited (status 0) +++
   ```

4. kami tertarik pada kata yang terdapat pada fungsi `strcmp` jadi kami mencobanya alhamdulillah ternyata benar! <mark>`#PWNED`</mark>
   
   ```bash
   $ ./crackme                                                            127 ⨯
   Hi!
   Input Your Password
   JJJJJJJJJJJJJJBxs
   MANTUL, flag is LKSSMK28{JJJJJJJJJJJJJJBxs}
   ```

### bongkarzzz

| Challenge : bongkarzzz                                                                                                                                                                                                                 |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| untuk menyelesaikan challenge ini teman-teman diharuskan membongkar file bongkarzzz untuk login ke website<br/><br/>http://202.148.27.84/bongkar/<br/><br/>akses user dan password dari hasil teman-teman mereverse program bongkarzzz |
| **Answer :**                                                                                                                                                                                                                           |

1. pertama-tama seperti biasa kami mengecek type file yang diberikan, bisa dilihat file tersebut merupakan ELF (Executable Linked File) 32 bit dan not stripped
   
   ```bash
   $ file bongkarzzz 
   bongkarzzz: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV),
   dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux 
   2.6.32, BuildID[sha1]=2e97060d42116e12d32b27d76c5af8202e980d2f, 
   not stripped
   ```

2. setelah di eksekusi filenya, terdapat input untuk memasukkan username dan jika salah akan mengeluarkan output `Cari password :(` 
   
   ```bash
   $ ./bongkarzzz 
   Cari Username : ;alskdjf
   Cari Password :(
   ```

3. setelah itu kami mencoba memreverse file tersebut dengan menggunakan `gdb` (GNU Debugger) dan mencoba melihat function apa saja yang tersedia
   
   ```bash
   $ gdb bongkarzzz
   GNU gdb (Ubuntu 9.2-0ubuntu1~20.04) 9.2
   Copyright (C) 2020 Free Software Foundation, Inc.
   License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
   This is free software: you are free to change and redistribute it.
   There is NO WARRANTY, to the extent permitted by law.
   Type "show copying" and "show warranty" for details.
   This GDB was configured as "x86_64-linux-gnu".
   Type "show configuration" for configuration details.
   For bug reporting instructions, please see:
   <http://www.gnu.org/software/gdb/bugs/>.
   Find the GDB manual and other documentation resources online at:
       <http://www.gnu.org/software/gdb/documentation/>.
   
   For help, type "help".
   Type "apropos word" to search for commands related to "word"...
   Reading symbols from bongkarzzz...
   (No debugging symbols found in bongkarzzz)
   (gdb) info functions
   All defined functions:
   
   Non-debugging symbols:
   0x080482f4  _init
   0x08048330  printf@plt
   0x08048340  puts@plt
   0x08048350  __gmon_start__@plt
   0x08048360  __libc_start_main@plt
   0x08048370  __isoc99_scanf@plt
   0x08048380  _start
   0x080483b0  __x86.get_pc_thunk.bx
   0x080483c0  deregister_tm_clones
   0x080483f0  register_tm_clones
   0x08048430  __do_global_dtors_aux
   0x08048450  frame_dummy
   0x0804847b  main
   0x080484f0  __libc_csu_init
   0x08048560  __libc_csu_fini
   0x08048564  _fini
   (gdb)
   ```

4. kami meneruskan dengan melihat isi function main
   
   ```bash
   (gdb) disassemble main
   Dump of assembler code for function main:
      0x0804847b <+0>:     lea    0x4(%esp),%ecx
      0x0804847f <+4>:     and    $0xfffffff0,%esp
      0x08048482 <+7>:     pushl  -0x4(%ecx)
      0x08048485 <+10>:    push   %ebp
      0x08048486 <+11>:    mov    %esp,%ebp
      0x08048488 <+13>:    push   %ecx
      0x08048489 <+14>:    sub    $0x14,%esp
      0x0804848c <+17>:    sub    $0xc,%esp
      0x0804848f <+20>:    push   $0x8048580
      0x08048494 <+25>:    call   0x8048330 <printf@plt>
      0x08048499 <+30>:    add    $0x10,%esp
      0x0804849c <+33>:    sub    $0x8,%esp
      0x0804849f <+36>:    lea    -0xc(%ebp),%eax
      0x080484a2 <+39>:    push   %eax
      0x080484a3 <+40>:    push   $0x8048591
      0x080484a8 <+45>:    call   0x8048370 <__isoc99_scanf@plt>
      0x080484ad <+50>:    add    $0x10,%esp
      0x080484b0 <+53>:    mov    -0xc(%ebp),%eax
      0x080484b3 <+56>:    cmp    $0x111ee0,%eax
      0x080484b8 <+61>:    je     0x80484cc <main+81>
      0x080484ba <+63>:    sub    $0xc,%esp
      0x080484bd <+66>:    push   $0x8048594
      0x080484c2 <+71>:    call   0x8048340 <puts@plt>
      0x080484c7 <+76>:    add    $0x10,%esp
      0x080484ca <+79>:    jmp    0x80484e1 <main+102>
      0x080484cc <+81>:    sub    $0x8,%esp
      0x080484cf <+84>:    push   $0x65a4d9
      0x080484d4 <+89>:    push   $0x80485a6
      0x080484d9 <+94>:    call   0x8048330 <printf@plt>
      0x080484de <+99>:    add    $0x10,%esp
      0x080484e1 <+102>:   mov    $0x0,%eax
      0x080484e6 <+107>:   mov    -0x4(%ebp),%ecx
      0x080484e9 <+110>:   leave  
      0x080484ea <+111>:   lea    -0x4(%ecx),%esp
      0x080484ed <+114>:   ret    
   End of assembler dump.
   ```

5. kami tertarik dengan assembly operation `cmp` yang berarti pengecekan / percabangan 
   
   ```bash
   0x080484b3 <+56>:    cmp    $0x111ee0,%eax
   ```
   
   jika dilihat cmp tersebut mencoba mencocokan registers `EAX` dengan `$0x111ee0`

6. kami mencoba melihat value dari `$0x111ee0` menggunakan python
   
   ```python
   $ python3
   Python 3.8.5 (default, Jul 28 2020, 12:59:40) 
   [GCC 9.3.0] on linux
   Type "help", "copyright", "credits" or "license" for more information.
   >>> 0x111ee0
   1122016
   ```

7. isinya adalah sebuah int `1122016` kami mencoba menginputkannya ke dalam script tersebut, mendapatkan output yang berbeda yeayy! <mark>`#PWNED`</mark>
   
   ```bash
   $ ./bongkarzzz 
   Cari Username : 1122016
   6661337
   ```

