---
layout: post
title: Debian (Jessie) සඳහා Brackets install කිරීම
---

[brackets][1] කියන්නෙ web designing/development කරනවා නම් ගොඩක් ප්‍රයෝජනවත් වෙන text editor එකක්. කොහොමවුනත් Debian Jessie, Ubuntu Vivid Vervet වගේ අලුත් OS  වලට brackets සාමාන්‍ය ක්‍රමයට install කරගන්න බෑ. මොකද brackets වල dependency එකක් විදිහට තියෙන `libgcrypt11` කියන library එක Debian Jessie වල නැති නිසා, ඒ වෙනුවට තියෙන්නෙ `libgcrypt20` කියන අලුත් library එක. මේ නිසා අපිට brackets site එකෙන් download කරන `.deb` file එක පාවිච්චි කරලා brackets install කරන්න බෑ.

කොහොම වුණත්, ඒ `.deb` file එකම පොඩ්ඩක් වෙනස් කරලා අපිට මේ අවුල හදාගන්න පුළුවන්. මේ තියෙන්නෙ මම මේක හදාගත්ත හැටි. මේ වැඩේ මම කලේ Debian 8 (Jessie) OS එකක. Ubuntu වල මම මේක test කලේ නෑ. ඒ නිසා විශ්වාසයක් නැතිනම් මේ වැඩේ Ubuntu (හෝ Debian 8 නොවෙන) OS එකක කරන්න යන්න එපා.

**පියවර 1**, මුලින්ම අපි අවශ්‍ය කරන packages download කරගන්න ඕනෙ.

* [brackets][1] වෙබ් අඩවියෙන් අවශ්‍ය කරන `Brackets.Release.1.8.64-bit.deb` package එක download කරගන්න.[^1]

* [libgcrypt11][2] debian package එක download කරගන්න. පහල තියෙන direct links වලින් amd64 (64bit), i386 (32bit) packages දෙක download කරගන්න පුළුවන්  

    * [i386][3]
    * [amd64][4]

**පියවර 2**, ඊලඟට මේ `Brackets.Release.1.8.64-bit.deb` file එක extract කරගන්න අවශ්‍යයයි. මොකද අපිට මෙතැනදි `libgcrypt11` කියන dependency එක මේකෙන් අයින් කරන්න වෙනවා.

```bash
$ dpkg-deb --raw-extract Brackets.Release.1.8.64-bit.deb Brackets
```

**පියවර 3**, දැන් මේ package එකෙන් `libgcrypt11` කියන dependency එක package එකෙන් අයින් කරන්න ඕනෙ.

```bash
$ sed -i 's/ libgcrypt11 (>= [0-9.]*),//' ./Brackets/DEBIAN/control
```

**පියවර 4**, ඊලඟට `libgcrypt11` package එක extract කරගන්න.

```bash
$ dpkg-deg --raw-extract libgcrypt11_1.5.0-5+deb7u5_amd64.deb libgcrypt11
```

**පියවර 5**, දැන් මේ library එක brackets package එකට copy කරන්න ඕනෙ.

```bash
$ cp -v libgcrypt11/lib/*-linux-gnu/libgcrypt.so.11* Brackets/opt/brackets/
```

**පියවර 6**, දැන් මේ Brackets කියන directory එක[^2] `.deb` package එකක් බවට හරවන්න ඕනෙ.

```bash
$ dpkg-deb --build Brackets brackets.deb
```

දැන් අපිට මේ `brackets.deb` කියන package එක පාවිච්චි කරලා brackets install කරගන්න පුළුවන්

```bash
$ dpkg -i bracket.deb
```


[^1]: package එකේ නම වෙනස් වෙන්න පුළුවන්
[^2]: අපි `Brackets.Release.1.8.64-bit.deb` package එක extract කරපු directory එක.


[1]: http://brackets.io/  
[2]: https://packages.debian.org/wheezy/libgcrypt11
[3]: http://security.debian.org/debian-security/pool/updates/main/libg/libgcrypt11/libgcrypt11_1.5.0-5+deb7u5_i386.deb
[4]: http://security.debian.org/debian-security/pool/updates/main/libg/libgcrypt11/libgcrypt11_1.5.0-5+deb7u5_amd64.deb
