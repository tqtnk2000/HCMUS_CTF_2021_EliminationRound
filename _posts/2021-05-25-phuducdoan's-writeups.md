---
layout: post
title: phuducdoan's writeups
subtitle: Hello guys, it's phuducdoan.
tags: [web, crypto, forensics]
comments: true
---

# Web
### EasyLogin

&emsp;Login with admin account with easy SQL Injection. For example: **username: admin' --** ==> Don't have any clue with Mario Image =)))

&emsp;Guess that a flag is stored in the database, so I use SQL Blind Injection to get a flag. 

&emsp;First of all, I need to know a table name that contains a flag. After that, guessing a flag that is stored inside a table database. Two-step can be done by using SQL Blind Injection.

&emsp;Example of SQL Blind Injection table name:

~~~
Username: admin
Password: ' OR '{character}' = SUBSTR((SELECT name FROM sqlite_master WHERE type='table' ORDER BY name ASC LIMIT 1), {position}, 1) --
~~~

&emsp;Brute force every (position, character), we will get a table name: **flagtablewithrandomname**.

&emsp;With a similar idea, get a flag that is stored in a database.

> Flag: **HCMUS-CTF{easY_sql_1nj3ctIon}**

# Forensics
### NiceEars

&emsp;Open the audio file with Sonic Visualizer, add Spectrogram layer, adjust some params to see text inside this layer clearly. We have password: **M0nK3y_doNkeY**

&emsp;Unzip zip file with password, get a flag.

> Flag: **HCMUS-CTF{Just_give_you_some_points_from_audio_stuff}**

### TestYourCmd

&emsp;Modify master image header into PNG header. After that, we could open it and get a hint: 


&emsp;Extract all content that matches with this hint by a short script python:

~~~
import os
directory = '.'

evidences = []
 
for filename in os.listdir(directory):
 if ('.' not in filename):
   subdir = directory + "/" + filename
   for f in os.listdir(subdir):
     fin = open(os.path.join(subdir, f), "r")
     text = fin.read()
     if (("Send To: Ronaldo" in text) and ("From: Messi") in text):
       evidences.append(text)
      
print(evidences[1])
~~~

&emsp;Run the script, we get email content in base64, decode it and get a paraphrase: **SuPer_Gold_3ymArJr**

&emsp;Using steghide to extract flag inside image with the parapharase: **steghide extract sf {image_file} -p  SuPer_Gold_3ymArJr.**

&emsp;We have to solve it with a lot of images, so you need to write a short bash code to run steghide and get a flag.

> Flag: **HCMUS-CTF{at_least_I_hope_you_can_code_a_bit}**

# Crypto
### Permutation

&emsp;Build a graph with 512 vertexes, edge from i to p[i]. Because p is a permutation, so the graph will exist some cycles. Assume that this graph has M cycles with lengths are k[1], k[2], ..., k[M] respectively. Therefore, p<sup>X</sup> = p when X = LCM(k[1], k[2], ..., k[M]) (Least Common Multiple).

&emsp;Connect to server several times, we collect a different set of (perm, perm_power). After that, calculate X = LCM(k[1], k[2], k[M]). Brute force to find R (remainder) such that perm<sup>R</sup> = perm_power. So we got one equation **flag mod X = R** with one pair (perm, perm_power).

&emsp;With 300 equations (to make modulo large enough) collect above, we solve them with the Chinese Remainder Theorem to get a flag.


> Flag: **HCMUS-CTF{discrete_log_is_easy_on_permutation_group}**

