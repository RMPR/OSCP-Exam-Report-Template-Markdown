---
title: "Pre-engineer Internship Report"
subtitle: "Improve smbcmp the capture diff tool"
author: ["akoudanilo@gmail.com", "OSID: 15P080"]
date: "2019-11-04"
subject: "Markdown"
keywords: [smbcmp, wireshark, tshark, capture]
subtitle: "Improve smbcmp the capture diff tool"
lang: "en"
titlepage: true
titlepage-color: "1E90FF"
titlepage-text-color: "FFFAFA"
titlepage-rule-color: "FFFAFA"
titlepage-rule-height: 2
book: true
classoption: oneside
code-block-font-size: \scriptsize
---

# Introduction

## Acknowledgement
I would like to express my deepest appreciation to all those who provided me the possibility to complete this report. 
A special gratitude goes to my mentor, Mr Aurelien Aptel for his time taken to explain things to me, for cleaning up and for correcting any errors that I might have missed otherwise.
Furthermore I would also like to acknowledge with much appreciation the crucial role of my teachers, especially Mr KOUAMOU Georges, who gave me the basics in algorithms needed to achieve my work specifically the algorithms I had to develop during this internship.
A special thanks goes to my family, friends and classmates who helped me in their own ways by providing support, empathy and courage to go through all that.

## Sigles et abbreviations
- **SMB**: Server Message Block
- **PDML**: Packet Description Markup Language

## Abstract
Smbcmp is a cli tool for making diffs between two pcap files containing SMB packets and rendering them using curses.
The first part of the project consisted in making better diffs by using the pdml output of Tshark and the second part consisted in adding a GUI and port it to windows. 
More details in the ![Appendix A]

## Résumé
Smbcmp est un outil en ligne de commandes permettant de visualiser les différences entre deux fichiers pcap contenant des paquets SMB et de les afficher à l'aide de la bibliotheque ncurses .
La première partie du projet consistait à améliorer les différences en utilisant la sortie pdml de Tshark et la seconde à ajouter une interface graphique et à la transférer vers Windows. Lire la proposition initiale complète

## Liste des tableaux

## Table des figures

## General Introduction

# Etat des lieux

## Presentation de l'entreprise

### Historique et localisation

#### Historique

#### Localisation

#### Fiche d'identification

# Etat de l'art

# Presentation de l'existant

# Choix de la solution

# Implementation de la solution

## Technologies used 

# Conclusion

# Glossary

# References
- https://www.gnu.org/software/ncurses/

# Appendix 
## Appendix A - Proposal

### Introduction
At present, the smbcmp tool can view single capture or diff 2 captures side by side
with a diff on the bottom pane but smbcmp has a very little user base a is nothing more
finally than a script, at first my reflex was to verify if it's available on the main
repository of my distro but I think this is such a great program which deserves more than
git clone && chmod +x, instead, regular updates and improvements.

### Project Goals
The aim of this project is (as stated in the title) to improve the tool by adding some 
features, the first are the ideas took on the idea list, then some of my ideas to actually improve the tooling

#### Use or combine current tshark output with the XML output
This is for doing better and deeper diffs by ignoring indentation differences, adding
ways to let users add/ignore rules, etc. 
The XML output is known in the Wireshark world as PDML (Product Data Markup Language)
after some researches, I found a resource explaining the specifications http://xml.coverpages.org/pdml.html
I think that in order to do that, I'll use the ##-Tpdml## option of tshark and use the
output to apply filters (add/ignore rules).

##### Add an html output 
According to https://wiki.wireshark.org/PDML, make an html output from the xml one is
pretty easy, with ##xsltproc## from the xslt library with a simple call, then style
the webpage to render a pretty thus more readable output.

##### Deliverable 
A pull request containing all the changes ready for review.

#### Make smbcmpp highlight diffs from the packet summary listing
The current implementation of this is satisfying (at least for me) but one thing I think I can
add is a descriptive text stating that white packets are the same, it may be confusing at first for
newbies who doesn't really know the structure of a samba packet.

##### Deliverable
A pull request

#### Automate the creation of .smbcmp file
For now, we need to manually copy the settings from the sample in the readme then
paste it and tweak to our desires, first i want to automate the process of
a config file, and maybe add a frontend for configuration.

##### Deliverable
A pull request

#### Make a proper delivery way
By packaging as flatpak, appimage or snap. At least creating an "install/configure" script

##### Deliverable
It depends on what will be choosen, but at this point, the Readme of the repos should be updated with
the new delivery method.

#### Correct soome flaws of the tool

##### smbcmp throws an error if there is no samba packet in the pcap file
this is already listed on this issue https://github.com/aaptel/smbcmp/issues/3, I plan to tackle it
in my work.

##### smbcmp close unexpectedly after resizing to critical values

### Timeline
The breakdown structure here is just as the program announce it and as an approximative indication on what I will be
working on.

#### Community Bonding Period[May 6 - May 26] 
Since I have exams starting in this period, I'll need a little break of maximum 3 weeks but during that time I won't
be idle as I can start prototyping the html page output and get insight and advices about my weak areas + a better
understanding on how the core samba protocol works, I will talk regurlarly to my mentor(s) about any specifications
on various functionnalities.

#### Coding officially begins [May 27]
I will be able to work full time on the project after the end of my exams (around the 7th of June). The classes restart 
by the 9th of September.

##### June 7 - June 28 (First second and third Weeks)
I will be working on the use and combination of xml output for better overview of results + the implementation of the
html view and diff highlighting, on the official timeline it's stated that from 24 to 28 of June there are evaluations
it's also included in my planning

##### June 28 - July 26 (Weeks 4 - 7)
Automate the creation of .smbcmp file and correct some flaws of the tools, including those that I would have eventually
added. This phase is also marked by and evaluation.

##### July 26 - August 19 (Week 7 - end) 
Work on the delivery process of the application, here I will make the final decision on how I should distribute it.

### About Me

#### Name 
Mairo Paul Rufus

#### Email
akoudanilo@gmail.com

#### Github
https://github.com/RMPR

#### Phone Number
+237 690823108

#### Summary
Currently I'm pursuing a Bachelor degree in computer engineering (2015-2020) in
National Advanced School of Engineering of Yaounde, Cameroon. I'm a seft taugh considering
python since we are most working with JAVA. As for my work experience, last year we 
implemented a recommender system hosted at http://52.23.196.12 on an AWS machine and
the years before that I used to do "IT-hoping" (test many IT fields in order to see
what suits me the most, hence many contributions in web development).

#### Contributions to OSS :

##### Merged PR
- [Powerline](https://github.com/powerline/fonts/pull/293)
- [Telegram Desktop](https://github.com/telegramdesktop/tdesktop/pull/5502)
- [Awesome design tools](https://github.com/LisaDziuba/Awesome-Design-Tools/pull/114)
- [Awesome design tools](https://github.com/LisaDziuba/Awesome-Design-Tools/pull/149)
- [Smbcmp](https://github.com/aaptel/smbcmp/pull/2)
- [Source code pro](https://github.com/adobe-fonts/source-code-pro/pull/216)
     

##### Unmerged PR
- [Daily Coding Problem](https://github.com/r1cc4rdo/daily_coding_problem/pull/7)
- [yii2-user-extended](https://github.com/cinghie/yii2-user-extended/pull/13)
- [Typescript react starter](https://github.com/Microsoft/TypeScript-React-Starter/pull/230)
- [Polybar](https://github.com/jaagr/polybar/pull/1712)



## Appendix B - Recursive pdml diffs algorithm

For the exam, I used my Metasploit/Meterpreter allowance on the following machine: `192.168.x.x`

## Appendix C - Circular search inside a list

```
code here
```
