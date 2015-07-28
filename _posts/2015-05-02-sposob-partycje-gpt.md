---
layout: post
title: Partycje GPT
---
Robimy partycje na dużym dysku (lub mamłym, bo mamy taką fantazje) - używamy tablicy GPT. 

>parted /dev/sdt mklabel gpt 

>parted -a optimal /dev/sdt mkpart primary 0% 90%
