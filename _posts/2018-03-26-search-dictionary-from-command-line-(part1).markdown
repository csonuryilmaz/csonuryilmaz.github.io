---
layout: post
title:  "Search Dictionary From Command Line (Part 1)"
date:   2018-03-26 00:00:00 +0300
categories: curl dict
comments: true
---

Do you know that you can search [dict.org](http://www.dict.org/bin/Dict) from command-line? By using **curl** it's possible. Also it saves you lots of time when on a remote server which has no user interface than black terminal.

Here is the simplest example:
```
$ curl dict://dict.org/d:joystick
220 pan.alephnull.com dictd 1.12.1/rf on Linux 4.4.0-1-amd64 <auth.mime> <41128842.13277.1491053298@pan.alephnull.com>
250 ok
150 1 definitions retrieved
151 "joystick" wn "WordNet (r) 3.0 (2006)"
joystick
    n 1: a lever used by a pilot to control the ailerons and
         elevators of an airplane [syn: {stick}, {control stick},
         {joystick}]
    2: a manual control consisting of a vertical handle that can
       move freely in two directions; used as an input device to
       computers or to devices controlled by computers
.
250 ok [d/m/c = 1/0/26; 0.000r 0.000u 0.000s]
221 bye [d/m/c = 0/0/0; 0.000r 0.000u 0.000s]
```
As you see, by default it used "WordNet (r) 3.0 (2006)" dictionary in order to search keyword _"joystick"_.

We list dictionaries by:
```
$ curl dict://dict.org/show:db
220 pan.alephnull.com dictd 1.12.1/rf on Linux 4.4.0-1-amd64 <auth.mime> <41129103.18366.1491053624@pan.alephnull.com>
250 ok
110 72 databases present
gcide "The Collaborative International Dictionary of English v.0.48"
wn "WordNet (r) 3.0 (2006)"
moby-thesaurus "Moby Thesaurus II by Grady Ward, 1.0"
elements "The Elements (07Nov00)"
vera "V.E.R.A. -- Virtual Entity of Relevant Acronyms (September 2014)"
jargon "The Jargon File (version 4.4.7, 29 Dec 2003)"
foldoc "The Free On-line Dictionary of Computing (18 March 2015)"
easton "Easton's 1897 Bible Dictionary"
hitchcock "Hitchcock's Bible Names Dictionary (late 1800's)"
bouvier "Bouvier's Law Dictionary, Revised 6th Ed (1856)"
devil "The Devil's Dictionary (1881-1906)"
world02 "CIA World Factbook 2002"
gaz2k-counties "U.S. Gazetteer Counties (2000)"
gaz2k-places "U.S. Gazetteer Places (2000)"
gaz2k-zips "U.S. Gazetteer Zip Code Tabulation Areas (2000)"
fd-tur-eng "Turkish-English FreeDict Dictionary ver. 0.2.1"
fd-por-deu "Portuguese-German FreeDict Dictionary ver. 0.1.1"
fd-nld-eng "Dutch-English Freedict Dictionary ver. 0.1.3"
fd-eng-ara "English-Arabic FreeDict Dictionary ver. 0.6.2"
fd-spa-eng "Spanish-English FreeDict Dictionary ver. 0.1.1"
fd-eng-hun "English-Hungarian FreeDict Dictionary ver. 0.1"
fd-ita-eng "Italian-English FreeDict Dictionary ver. 0.1.1"
fd-wel-eng "Welsh-English Freedict dictionary"
fd-eng-nld "English-Dutch FreeDict Dictionary ver. 0.1.1"
fd-fra-eng "French-English FreeDict Dictionary ver. 0.3.4"
fd-tur-deu "Turkish-German FreeDict Dictionary ver. 0.1.1"
fd-swe-eng "Swedish-English FreeDict Dictionary ver. 0.1.1"
fd-nld-fra "Nederlands-French FreeDict Dictionary ver. 0.1.1"
fd-eng-swa "English-Swahili xFried/FreeDict Dictionary"
fd-deu-nld "German-Dutch FreeDict Dictionary ver. 0.1.1"
fd-fra-deu "French-German FreeDict Dictionary ver. 0.1.1"
fd-eng-cro "English-Croatian Freedict Dictionary"
fd-eng-ita "English-Italian FreeDict Dictionary ver. 0.1.1"
fd-eng-lat "English-Latin FreeDict Dictionary ver. 0.1.1"
fd-lat-eng "Latin-English FreeDict Dictionary ver. 0.1.1"
fd-fra-nld "French-Dutch FreeDict Dictionary ver. 0.1.2"
fd-ita-deu "Italian-German FreeDict Dictionary ver. 0.1.1"
fd-eng-hin "English-Hindi FreeDict Dictionary ver. 1.5.1"
fd-deu-eng "German-English FreeDict Dictionary ver. 0.3.4"
fd-por-eng "Portuguese-English FreeDict Dictionary ver. 0.1.1"
fd-lat-deu "Latin - German FreeDict dictionary ver. 0.4"
fd-jpn-deu "Japanese-German FreeDict Dictionary ver. 0.1.1"
fd-eng-deu "English-German FreeDict Dictionary ver. 0.3.6"
fd-eng-scr "English-Serbo-Croat Freedict dictionary"
fd-eng-rom "English-Romanian FreeDict Dictionary ver. 0.6.1"
fd-iri-eng "Irish-English Freedict dictionary"
fd-cze-eng "Czech-English Freedict dictionary"
fd-scr-eng "Serbo-Croat-English Freedict dictionary"
fd-eng-cze "English-Czech fdicts/FreeDict Dictionary"
fd-eng-rus "English-Russian FreeDict Dictionary ver. 0.3"
fd-afr-deu "Afrikaans-German FreeDict Dictionary ver. 0.3"
fd-eng-por "English-Portuguese FreeDict Dictionary ver. 0.2.2"
fd-hun-eng "Hungarian-English FreeDict Dictionary ver. 0.3.1"
fd-eng-swe "English-Swedish FreeDict Dictionary ver. 0.1.1"
fd-deu-ita "German-Italian FreeDict Dictionary ver. 0.1.1"
fd-cro-eng "Croatian-English Freedict Dictionary"
fd-dan-eng "Danish-English FreeDict Dictionary ver. 0.2.1"
fd-eng-tur "English-Turkish FreeDict Dictionary ver. 0.2.1"
fd-eng-spa "English-Spanish FreeDict Dictionary ver. 0.2.1"
fd-nld-deu "Dutch-German FreeDict Dictionary ver. 0.1.1"
fd-deu-por "German-Portuguese FreeDict Dictionary ver. 0.2.1"
fd-swa-eng "Swahili-English xFried/FreeDict Dictionary"
fd-hin-eng "English-Hindi Freedict Dictionary [reverse index]"
fd-deu-fra "German-French FreeDict Dictionary ver. 0.3.1"
fd-eng-fra "English-French FreeDict Dictionary ver. 0.1.4"
fd-slo-eng "Slovak-English Freedict dictionary"
fd-gla-deu "Scottish Gaelic-German FreeDict Dictionary ver. 0.1.1"
fd-eng-wel "English-Welsh Freedict dictionary"
fd-eng-iri "English-Irish Freedict dictionary"
english "English Monolingual Dictionaries"
trans "Translating Dictionaries"
all "All Dictionaries (English-Only and Translating)"
.
250 ok
221 bye [d/m/c = 0/0/0; 0.000r 0.000u 0.000s]
```
For example, to search a word in a specific dictionary:
```
$ curl dict://dict.org/d:computer:fd-eng-tur
220 pan.alephnull.com dictd 1.12.1/rf on Linux 4.4.0-1-amd64 <auth.mime> <41129217.22200.1491053844@pan.alephnull.com>
250 ok
150 1 definitions retrieved
151 "computer" fd-eng-tur "English-Turkish FreeDict Dictionary ver. 0.2.1"
computer
 1. kompüter; hesap eden kimse
 2. elektronik hesap makinası; elektronik beyin. computer hardware kompüterin esas kısımları. computer software yapılacak işe göre değiştirilen kompüterin yardımcı aksamı. analogue computer kendisine verilen rakamlan elektronik nicelikler şeklinde kullanarak hesap çıkaran makina. diqital computer kendisine verilen rakamları ikili rakam olarak kullanarak hesap çıkaran makina.
.
250 ok [d/m/c = 1/0/15; 0.000r 0.000u 0.000s]
221 bye [d/m/c = 0/0/0; 0.000r 0.000u 0.000s]
```
It used "English-Turkish FreeDict Dictionary ver. 0.2.1" dictionary in order to search keyword _"computer"_.

My following blog post will be about a simple script that uses internally this approach and simplifies the usage. It also has an install option so that you can invoke it from anywhere, globally.

Happy coding.

{% include share-post-on-twitter.html %}