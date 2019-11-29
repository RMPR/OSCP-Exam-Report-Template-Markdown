---
title: "Pre-engineer Internship Report"
subtitle: "Improve smbcmp the capture diff tool"
author: ["akoudanilo@gmail.com", "OSID: 15P080"]
date: "2019-11-04"
subject: "Markdown"
keywords: [smbcmp, wireshark, tshark, capture]
subtitle: "Improve smbcmp the capture diff tool"
lof: true
lang: "en"
titlepage: true
titlepage-color: "1E90FF"
titlepage-text-color: "FFFAFA"
titlepage-rule-color: "FFFAFA"
titlepage-rule-height: 2
book: true
classoption: oneside
code-block-font-size: \scriptsize
bibliography: project.bib
csl: mla.csl
margin-left: 70px
margin-right: 55px
margin-bottom: 100px
---

# Preamble

## Acknowledgement
I would like to express my deepest appreciation to all those who provided me the possibility to complete this report.
First thing first my parents, without who I wouldn't even exist in the first
place and logically God Who makes all things possible.
A special gratitude goes to my mentor, Mr Aurelien Aptel for his time taken to explain things to me, for cleaning up and correcting errors that I might have missed otherwise.
Furthermore I would also like to acknowledge with much appreciation the crucial role of my teachers, especially Mr KOUAMOU Georges, who gave me the basics in algorithms needed to achieve my work, specifically the algorithms I had to develop during this internship.
A special thanks goes to my family, friends and classmates who helped me in their own ways by providing support, empathy and courage to go through all that.

## Sigles et abbreviations
- **SMB**: Server Message Block
- **PDML**: Packet Description Markup Language
- **GSoC**: Google Summer of Code
- **cET**: cElementTree
- **CLI**: Command Line Interface
- **GUI**: Graphical User Interface
- **CIFS**: Common Internet File System

## Abstract
Smbcmp is a CLI tool for making diffs between two pcap files containing SMB packets and rendering them using ncurses.
The first part of the project consisted in making better diffs by using the PDML output of Tshark and the second part consisted in adding a GUI and port it to windows. More details in the [Appendix A](#appendix-a---proposal)


## Résumé
Smbcmp est un outil en ligne de commandes permettant de visualiser les différences entre deux fichiers pcap contenant des paquets SMB et de les afficher à l'aide de la bibliotheque ncurses .
La première partie du projet consistait à améliorer les différences en utilisant la sortie PDML de Tshark et la seconde à ajouter une interface graphique et à creer une version pour Windows. Plus de details sur la 
proposition dans l'[appendice A](#appendix-a---proposal)


## General Introduction
SMB is a protocol providing shared access to files, printers, and serial ports 
between nodes on a network, one version of SMB was known as CIFS. It also 
provides an authenticated inter-process communication mechanism. Most usage of 
SMB involves computers running Microsoft Windows, where it was known as 
"Microsoft Windows Network" before the introduction of Active Directory.
Knowing how to troubleshoot connectivity problems can be 
cumbersome, hence network sniffers like Wireshark/Tshark and many others. While 
this may be considered as good enough by many, the problem of comparison arise 
very quickly, i.e. the ability to know between two situations what exactly went 
wrong, an use case that Wireshark/Tshark can't handle properly that's why 
smbcmp has been created.

## Presentation of Google Summer of Code

![Google Summer of Code Logo](./img/google-summer-of-code-2018.png){width="50%"}

### Overview
Google Summer of Code is a global initiative focused on bringing more students
(undergrads, grads and post grads), mainly developers
into open source software development. Students work with an open
source organization on a 3 months programming project during their break from 
school, typically during the summer.

### How it works
As a part of Google Summer of Code, student participants are paired with a
mentor from the participating organizations, gaining exposure to real-world
software development and techniques. Students have the opportunity to spend the
break between their school semesters earning a stipend while working in areas
related to their interests. Practically, we have the following relationship.

*Students* contact the mentor organizations they want to work with and write up a
project proposal for the summer. If accepted, students spend a month
integrating with their organizations prior to the start of coding. Students
then have three months to code, meeting the deadlines agreed upon with their 
mentors.

*Open source projects* apply to be mentor organizations. Once accepted,
organizations discuss possible ideas with students and then decide on the 
proposals they wish to mentor for the summer. They provide mentors to help 
guide each student through the program.

*Existing contributors* with the organizations can choose to mentor a student 
project. Mentors and students work together to determine appropriate milestones 
and requirements for the summer. Mentor interaction is a vital part of the 
program.

In turn, the participating organizations are able to identify and bring in new 
developers who implement new features and hopefully continue to contribute to 
open source even after the program is over. Most importantly, more code is 
created and released for the use and benefit of all.

### History
The idea for the Summer of Code came directly from Google's founders, Sergey 
Brin and Larry Page.[@GoogleSummerCode2019] From 2007 until 2009 Leslie Hawthorn, who has been 
involved in the project since 2006, was the program manager. From 2010 until 
2015, Carol Smith was the program manager. In 2016, Stephanie Taylor took 
over management of the program. 
In 2005, more than 8,740 project proposals were submitted for the 200 available student positions. Due to the overwhelming response, Google expanded the program to 419 positions.

The mentoring organizations were responsible for reviewing and selecting 
proposals, and then providing guidance to those students to help them complete 
their proposal. Students that successfully completed their proposal to the 
satisfaction of their mentoring organization were awarded a stipend and a 
Google 
Summer of Code T-shirt, while $500 per project was sent to the mentoring 
organization. Approximately 80% of the projects were successfully completed 
in 2005, although completion rates varied by organization: Ubuntu, for example, 
reported a completion rate of only 64%, and KDE reported a 67% completion rate. Many projects were continued past summer, even though the SOC period was over, and some changed direction as they developed.

For the first Summer of Code, Google was criticized for not giving sufficient time to open source organizations so they could plan projects for the Summer of Code. Despite these criticisms there were 41 organizations involved, including FreeBSD, Apache, KDE, Ubuntu, Blender, Mozdev, and Google it

According to a blog post by Chris DiBona, Google's open source program manager, 
"something like 30 percent of the students stuck with their groups past SoC 
[Summer of Code]." Mozilla developer Gervase Markham also commented that none 
of the 10 Google-sponsored Mozilla projects survived after the event.
However, the Gaim (now Pidgin) project was able to enlist enough coding support
through the event to include the changes into Gaim (now Pidgin) 2.0; the Jabber 
Software Foundation (now XMPP Standards Foundation) and KDE project also 
counted a few surviving projects of their own from the event (KDE only counted 
1 continuing project from out of the 24 projects which it sponsored).
For the following years, the program kept growing with more and more applicants
among organizations and students, we have the following metrics [@StatisticsGoogleSummer] 

#### GSoC in numbers

- 2019

Coding Dates: May 27th - August 19th

1,276 students accepted from 63 countries

2,000+ mentors with active projects from 72 countries

201 open source organizations

89.05% overall success rate

- 2018

Coding Dates: May 14th - August 14th

1,264 students accepted from 62 countries

2,117 mentors with active projects from 73 countries

206 open source organizations

86.24% overall success rate

- 2017

Coding Dates: May 30th - August 29th

1,318 students accepted from 72 countries

1,647 mentors with active projects from 69 countries

3,439 registered mentors

198 open source organizations

86.2% overall success rate

- 2016

Coding Dates: May 23rd - August 23rd

1,206 students accepted from 67 countries

2,524 mentors from 66 countries

178 open source organizations

85.6% overall success rate

- 2015

Coding Dates: May 25th - August 21st

1,051 students accepted from 73 countries

1,903 mentors from 68 countries

137 open source organizations

88.2% overall success rate

- 2014

Coding Dates: May 19th - August 18th

1,307 students accepted from 72 countries

2,491 mentors from 78 countries

190 open source organizations

89.7% overall success rate

- 2013

Coding Dates: June 17th - September 27th

1,192 students accepted from 70 countries

2,212 mentors from 70 countries

177 open source organizations

88.9% overall success rate

- 2012

Coding Dates: May 21st - August 20th

1,212 students accepted from 69 countries

2,220 mentors from 66 countries

180 open source organizations

88.5% overall success rate

- 2011

Coding Dates: May 23rd - August 22nd

1,115 students accepted from 68 countries

2,096 mentors from 55 countries

175 open source organizations

88% overall success rate

- 2010

Coding Dates: May 24th - August 16th

1,026 students accepted from 69 countries

2,045 mentors from 52 countries

150 open source organizations

89% overall student success rate

- 2009

Coding Dates: May 23rd - August 25th

1,000 students accepted from 70 countries

1,991 mentors from 65 countries

150 open source organizations

85% overall student success rate

- 2008

Coding Dates: May 26th - August 18th

1,126 students accepted from 64 countries

2,164 mentors from 66 countries

175 open source organizations

83% overall student success rate

- 2007

Coding Dates: May 28th - August 31st

905 students accepted from 62 countries

1,819 mentors from 75 countries

135 open source organizations

81% overall student success rate

- 2006

Coding Dates: May 1st - September 8th

630 students accepted from 55 countries

1,252 mentors from 57 countries

102 open source organizations

82% overall student success rate 

In Cameroon, the university of Buea is the most represented institution with 37
students coming from it since the beginning of the program in 2015.

# State of the art of capture files diff tools

## Introduction
Let's say we have a network of computers with active directory configured, for
one reason or another, when we try to enable one feature of active directory 
the connectivity seems to have performance issues. The issues can be that 
the network is either failing or too
slow and we have two capture files obtained using any capture tool (wireshark,
tcpdump, ...) of the two states of the network. One question we can ask is
how can we 
analyze what changed in the network between the activation of the feature
and when everything worked perfectly ?

### Problematic

We want to visualize the diffs between 2 capture files 
and we are only interested in the SMB packets.

## Classical approach: view each file individually

&nbsp; Thanks to the open source community effort we have a very powerful network
analyzer: wireshark. There are other proprietary solutions, but generally they
lack many features that wireshark have. The idea is to find any misconfigured 
thing by 
browsing manually the first capture file then the second one, and since what
interest us here is the SMB protocol, we will use the smb or smb2 filters 
depending on which version of the protocol we are using.


## Middle approach: generate diffs using the text output of wireshark

There are many programs like that which typically rely on the textual output of
wireshark, and use a text differ to highlight changes, on Unix like systems a 
good diffs utility is `diff` present in the GNU coreutils. One example of a 
program like that is pcap-diff [@isginfIsginfPcapdiff2019].

## Our approach: Use the PDML output of Wireshark to make diffs
Knowing the advantages and the disadvantages of the two previous approachs, 
we tried to address all this using the XML output of Wireshark/Tshark in fact, 
there are typically 3 interesting, human-readable formats:

### Text
Each line is a packet and sub-packets are indented by one tab.
We can obtain it with the command:

```shell
tshark -2 -R "smb or smb2" -r /path/of/capture/file
```

![Textual output Wireshark](./img/tshark_std_output.png)


### Json
The json file is an array of objects and each object is a packet, each
packet contains many key/value pairs corresponding to a field or a subfield 
of the protocol used (header, flags, length, ...).
We can obtain it with the command:
```shell
tshark -2 -R "smb or smb2" -r /path/of/capture/file -T json
```

![Json output Wireshark](./img/tshark_json_output.png){width="70%"}

\pagebreak


### XML (PDML)
A PDML file contains multiple packets, denoted by the `<packet>` tag. A packet will contain multiple protocols, denoted by the `<proto>` tag.
A protocol might contain one or more fields, denoted by the `<field>` tag. 
We can obtain it with the command:

```shell
tshark -2 -R "smb or smb2" -r /path/of/capture/file -T PDML
```

![XML output Wireshark](./img/tshark_xml_output.jpg)

\pagebreak


# Design of the solution

## Analysis
In this section, we discuss about the stakeholders of our system, the different
actors, the functional and non-functional requirements and finally establish the
use cases and class diagram.

### Requirements engineering

#### Functionnal requirements
- visualize diffs side by side
- permute packets: Usually diffs are from packet A to packet B, if the user 
wants from B to A, he must not reload the packets
- Change different settings: colors, diff method
- search inside the packets

#### Non-functional requirements
- Use the programming language python
- Must use external dependencies only if necessary, moreover, the program must
work standalone
- No databases, store everything in files

### Use case diagram (GUI)

![Use case diagram smbcmp-gui](./img/use_case_diagram_smbcmp.png){width="80%"}

\pagebreak


- **Stakeholders**
The main stakeholder here are the people inside the company because it's an 
internal tool open sourced.

- **Actors**

- Main actor
  - User
- Secondary actor
  - smbcmp

- **Use cases**
- Open files : load the files inside the buffers
- Permute buffers : exchange files
- Change settings : like colors, diff engine
- Search packets : search packet in each buffer

### Class diagram 
We have the following class diagram:

![Class diagram](./img/class_diagram.png)

\pagebreak

## Technologies used 

### For the command line interface
The objective was to have as little dependencies as possible so in this part, 
there is only an *optionnal* dependency: `lxml` one may ask why, if it's 
optionnal
it can also be removed without many harm but in our research, lxml has been 
proven to be quicker than `cElementTree` (which is the standard library to 
parse xml files) for the operation we had to do on
the XML tree (mainly serialising and parsing)  as a side note, the two 
libraries propose the 
same API thus reducing the overhead of using them concurrently.

The following subsections are about the benchmark:
[@BenchmarksSpeed]

- **Serialising**

For serialising, lxml is more than 10 times as fast as the much improved 
ElementTree 1.3
in recent Python versions:
```shell
lxe: tostring_utf16  (S-TR T1)    7.9958 msec/pass
cET: tostring_utf16  (S-TR T1)   83.1358 msec/pass

lxe: tostring_utf16  (UATR T1)    8.3222 msec/pass
cET: tostring_utf16  (UATR T1)   84.4688 msec/pass

lxe: tostring_utf16  (S-TR T2)    8.2297 msec/pass
cET: tostring_utf16  (S-TR T2)   87.3415 msec/pass

lxe: tostring_utf8   (S-TR T2)    6.5677 msec/pass
cET: tostring_utf8   (S-TR T2)   76.2064 msec/pass

lxe: tostring_utf8   (U-TR T3)    1.1952 msec/pass
cET: tostring_utf8   (U-TR T3)   22.0058 msec/pass
```
- **Parsing**

For parsing, lxml.etree and cElementTree compete for the medal. Depending on the input, either of the two can be faster. The (c)ET libraries use a very thin layer on top of the expat parser, which is known to be very fast. Here are some timings from the benchmarking suite:

```shell
lxe: parse_bytesIO   (SAXR T1)   13.0246 msec/pass
cET: parse_bytesIO   (SAXR T1)    8.2929 msec/pass

lxe: parse_bytesIO   (S-XR T3)    1.3542 msec/pass
cET: parse_bytesIO   (S-XR T3)    2.4023 msec/pass

lxe: parse_bytesIO   (UAXR T3)    7.5610 msec/pass
cET: parse_bytesIO   (UAXR T3)   11.2455 msec/pass
```
- **Child Access**

The same tree overhead makes operations like collecting children as in list(element) more costly in lxml. Where cET can quickly create a shallow copy of their list of children, lxml has to create a Python object for each child and collect them in a list:

```shell
lxe: root_list_children        (--TR T1)    0.0038 msec/pass
cET: root_list_children        (--TR T1)    0.0010 msec/pass

lxe: root_list_children        (--TR T2)    0.0455 msec/pass
cET: root_list_children        (--TR T2)    0.0050 msec/pass
```

- **Element creation**

As opposed to ET, libxml2 has a notion of documents that each element must be in. This results in a major performance difference for creating independent Elements that end up in independently created documents:

```shell
lxe: create_elements           (--TC T2)    1.0045 msec/pass
cET: create_elements           (--TC T2)    0.0753 msec/pass
```

- **Merging different sources**

A critical action for lxml is moving elements between document contexts. It requires lxml to do recursive adaptations throughout the moved tree structure.

The following benchmark appends all root children of the second tree to the root of the first tree:
```shell
lxe: append_from_document      (--TR T1,T2)    1.0812 msec/pass
cET: append_from_document      (--TR T1,T2)    0.1104 msec/pass

lxe: append_from_document      (--TR T3,T4)    0.0155 msec/pass
cET: append_from_document      (--TR T3,T4)    0.0060 msec/pass
```

- **Deepcopy**
  
Deep copying a tree is fast in lxml:

```shell
lxe: deepcopy_all              (--TR T1)    3.1650 msec/pass
cET: deepcopy_all              (--TR T1)   53.9973 msec/pass

lxe: deepcopy_all              (-ATR T2)    3.7365 msec/pass
cET: deepcopy_all              (-ATR T2)   61.6267 msec/pass

lxe: deepcopy_all              (S-TR T3)    0.7913 msec/pass
cET: deepcopy_all              (S-TR T3)   13.6220 msec/pass
```

- **Tree traversal**

Another important area in XML processing is iteration for tree traversal which
is especially useful if the algorithms can benefit from step-by-step traversal 
of the XML tree bonus points if few elements are of interest or the target element tag name is known.

```shell
lxe: iter_all             (--TR T1)    1.0529 msec/pass
cET: iter_all             (--TR T1)    0.2635 msec/pass

lxe: iter_islice          (--TR T2)    0.0110 msec/pass
cET: iter_islice          (--TR T2)    0.0050 msec/pass

lxe: iter_tag             (--TR T2)    0.0079 msec/pass
cET: iter_tag             (--TR T2)    0.0112 msec/pass

lxe: iter_tag_all         (--TR T2)    0.1822 msec/pass
cET: iter_tag_all         (--TR T2)    0.5343 msec/pass
```

From this benchmark, it's clear that lxml outperforms blatantly cET for our
use cases and it even propose methods not implemented yet in cET, like a xpath
engine but for practical reasons (keep the dependecy optionnal) we only used
common features.

### Graphical User Interface

In the Python GUI multiplatform programming ecosystem, there are a few
legit choices (mature enough to be considered):

- **WXpython**: Python bindings of the WXWidgets framework 
- **Tkinter**: Standard library for GUI programming
- **Pyside 2 (Qt for python)**: Python bindings for the QT framework
- **PyQT**
- **Kivy**
- **PyGTK**
- **PySimpleGUI**

There is a comparative table for all of them based on the following criteria:

Framework | License | Documentation | Modern UI | Wysiwyg | Target | Native
----------|----------|--------------|-----------|---------|-----|-----
 Wxpython | GPL v3 | Good | Yes | Yes | Desktop | Yes
 Tkinter | GPL v3 | Good | No | No | Desktop | No
 Pyside 2 | GPL v3 | Poor | Yes | Yes | Desktop | No
 PyQT | Commercial | Medium | Yes | Yes | Desktop | No
 Kivy | BSD | Good | Yes | No | Mobile | No
 PyGTK | GPL v3 | Medium | No | Yes | Desktop | Only on Gnome
 PySimpleGUI | GPL v3 | Medium | Yes | No | Desktop | Yes

Because fo the requirements and the documentation, we ended up choosing 
*WXpython* for the project.

# Implementation
For this part, we had to implement a few algorithms, here we hightlight 2 of
those which are recursive:

## Algorithm 1: recursive diffs between 2 nodes of the XML graph

### Principle 
We have two top-level packets `A` and `B`, we face the following cases:

- **Cases where A and B have different numbers of children**

In this case we identify each child, match those which are the same and 
mark the remaining as either removed or added depending on which the parent,
if it's `A` we mark added and if it's `B` we mark removed.

- **1 Leaf node vs 1 Folder node**

If not child_a.children or not child_b.children:
In this case we just keep going deeper inside the folder node, and 
we print all that as diffs.

- **Terminal cases: 2 leaf nodes**

If the nodes are roughly the same (just the values changed) we store this 
change else we mark as two different lines.

- **Recursive case: 2 Tree nodes**
 
We call the function again on the two nodes.

### Pseudo-code
The pseudo-code is quite long, you can find it in [Appendix B](#appendix-b---recursive-PDML-diffs-algorithm)

### Complexity analysis 
We make a few assumptions, the packets `A` and `B` which are represented
as trees have respectively n and m elements 

#### Time complexity
In the worst case, the packets aren't the same but have the same structure with
the same number of nodes, in this case, we have to go to each leaf just to
notice that the leafs aren't the same and marked the first one as added and the
second one as removed. We have obviously O(n^2)


#### Space complexity
We store the both trees and use a constant space to make diffs, so O(n^2).

## Algorithm 2: Circular search inside a non-circular list

### Principle

We have a list of strings (packets) `data` inside a `buffer` and we are looking
for a certain string `text` and if we find it, (even a partial match) we update the `buffer` selected item and stop the search.

In order to achieve that, we make a 
fuzzy search (finding approximate substring matches inside a given list of strings) and 
stop at the next occurrence, repeat this process while the user hit the search
button and if we reach the bottom of the list, the search restart automatically
at the top of the list.

### Pseudo code
```python
    function search(text, rec=True):
        """ Cyclic search inside summaries

        Keyword arguments:

        text -- expression searched
        rec -- keep track of recursive calls
        """
        sel = buffer.GetSelection()

        if rec:
            search_index = sel
        else:
            search_index = -1

        for i in range(search_index + 1, len(data) + 1):
            #  edge case : The last packet is selected
            if i >= len(data):
                found = False
                break

            found = text.lower() in data[i].lower()
            if found:
                search_index = i
                break

        if not found:
            if rec:
                search(text, rec=False)
        else:
            buffer.SetSelection(search_index)
```


### Complexity analysis

#### Time complexity
If we assume that the list `data` has n elements and the max size of elements
inside data is p ( p = max(data[i], 0 < i < n+1) ), in the worst case there are no matchs in the whole list, the complexity is then O(n).

#### Space complexity
The space corresponding is O(n)


# Evaluation of results
Typically, SMB 2 Protocol messages begin with a fixed-length SMB2 header that 
is 
described is the SMB specifications [@openspecs-officeMSSMB2ServerMessage]
The SMB2 header contains a Command field indicating the operation code that is 
requested by the
client or responded to by the server. An SMB 2 Protocol message is of variable 
length, depending on
the **Command field** in the SMB2 header and on whether the SMB 2 Protocol 
message is a client
request or a server response.

So basically, when you are trying to make diffs on SMB packets, there are two
possibilities:

- the command fields of the packets are different thus the packets aren't of the
same type
- the command fields are the same thus the packets are of the same type

## Diffs

### Different command fields

![Plain text output for different command fields](./img/plain_text_different_command.png)

\pagebreak

![PDML output for different command fields](./img/pdml_different_command.png)

\pagebreak

- **Interpretation**
As you can notice from the previous plain text output, diffs were done blocks
by blocks instead of in this case line by line which lead to something like
the `Credit Charge` field has only one minor change but in the plain text 
output it's considered as part of the 6 lines change while in this case
the `Credit Charge: 0 -> 1` is clearly highlighted. Moreover, in the PDML
output, the following changes are highlighted one at the time so that the user
know exactly what really change without putting too much effort of making 
the visual correspondance by him

\pagebreak
### Same command fields

![Plain text output for same command fields](./img/plain_text_same_command.png)

\pagebreak

![PDML output for same command fields](./img/pdml_same_command.png)

\pagebreak

- **Interpretation**
Here it's even more visible, using the plain text output, it's very difficult
to separate a field from his value, that's what you can notice from the first 
output; the fields `Time for request`, `Preaut Hash` and `Current Time` are only
changing values but with the first version they are highlighted as one line
change while the second are hightlighted more precisely as values change.

\pagebreak

## GUI
One objective was to port to windows, 
the tests have been realized on Windows 10 Professional and Fedora Workstation
31 the views are following:

### Windows
![Windows view smbcmp](./img/smbcmp_windows.png)

\pagebreak

### Linux
![Linux view smbcmp](./img/smbcmp_linux.png)

\pagebreak


# Prospect
To improve the usefulness of our system, we can think of many things that can
be added or done differently:

## Add documentation for config file and usage

About :
- file format of the config file (possible options, possible values,...)
- the location it will be loaded from
- how to use -k, how to generate keys from kernel or samba

## Install documentation and desktop file in packaging

Modify packaging scripts for all OS to install the man pages and desktop file
to make it available on all major Linux distributions, actually it's only on 
Opensuse repository.

## Pass down arguments passed to windows launcher to the Python scripts

The windows port is not perfect, there are some features present on the Linux
version which aren't yet on windows.
Add arguments to smbcmp.exe (-h) and Drag and drop support to load capture files
more easily and intuitively.

## Implement ignored fields in smbcmp-gui

Add or remove ignored field from the diff view (right click)
Add a main menu item to list and manage ignored fields. Implement ignored 
fields filters :

- value greater than, equal to, less than
- value is one of x, y, z
- value contains x

We can think of creating a little programming language with its interpreter
which will be able to parse and validate rules.



# Conclusion
Our target during this internship at Samba conducted by Google was to improve
an existing tool by adding features (better and deeper diffs, GUI, 
Windows port). For that, we had to make some research about the state of the 
art of Capture diff tooling, after what we started developing our solution
according to the software reuse principle called : encapsulating an existing
system, we ended up with 3 components : smbcmp (the CLI), smbcmp-gui (the GUI) 
and smbcmp-common (a set of common utilities for the previous components).
Despite the fact that we were new in the working internals of a protocol
namely SMB which has been around for quite a while, with multiple revisions
through the years, we developed a working solution. At the end, we developed
some skills in Python programming (debugging, testing), remote work, steady 
communication, Git and Github workflow.

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
after some researches, I found a resource explaining the specifications http://xml.coverpages.org/PDML.html
I think that in order to do that, I'll use the ##-TPDML## option of tshark and use the
output to apply filters (add/ignore rules).

##### Add an html output 
According to https://wiki.wireshark.org/PDML, make an html output from the xml one is
pretty easy, with ##xsltproc## from the xslt library with a simple call, then style
the webpage to render a pretty thus more readable output.

##### Deliverable 
A pull request containing all the changes ready for review.

#### Make smbcmp highlight diffs from the packet summary listing
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



## Appendix B - Recursive PDML diffs algorithm
```python
function smb_diff():
    """Compute PDML diff and return DiffOutput instance"""

    output = DiffOutput()

    function rec_diff(a, b, n=0):
        final_eq = SAME

        #
        # Diff a folder attributes
        #

        eq, ign = diff_attr_with_rules(a, b)
        if eq == SAME:
            # doesnt matter which
            output.print_field(OUT_SAME, a, indent=n)
        elif eq == MOD:
            output.print_mod_field(a, b, indent=n)
            if not ign:
                final_eq = max(final_eq, eq)
        elif eq == DIFF:
            output.dump(OUT_REM, a, ignored=ign, indent=n)
            output.dump(OUT_ADD, b, ignored=ign, indent=n)
            if not ign:
                final_eq = max(final_eq, eq)
            return final_eq

        #
        # Diff the children of the folder
        #

        sm = difflib.SequenceMatcher(None, a.children, b.children)
        for tag, i1, i2, j1, j2 in sm.get_opcodes():
            if tag == 'delete':
                for child_a in a.children[i1:i2]:
                    ign = ignored_with_rules(child_a)
                    output.dump(OUT_REM, child_a, ignored=ign, indent=n+1)
                    if not ign:
                        final_eq = max(final_eq, eq)
                continue
            elif tag == 'insert':
                for child_b in b.children[j1:j2]:
                    ign = ignored_with_rules(child_b)
                    output.dump(OUT_ADD, child_b, ignored=ign, indent=n+1)
                    if not ign:
                        final_eq = max(final_eq, eq)
                continue
            elif tag == 'equal':
                for child_a, child_b in zip_longest(a.children[i1:i2],
                                                    b.children[j1:j2]):
                    eq, ign = diff_field_with_rules(child_a, child_b)
                    if eq == SAME:
                        # doesnt matter which
                        output.dump(OUT_SAME, child_a, indent=n+1)
                    elif eq == MOD:
                        output.print_mod_field(child_a, child_b,
                                                ignored=ign, indent=n+1)
                        if not ign:
                            final_eq = max(final_eq, eq)
                    else:
                        raise Exception("nodes should not be diff here")
                continue
            elif tag == 'replace':
                for child_a, child_b in zip_longest(a.children[i1:i2],
                                                    b.children[j1:j2]):

                    #
                    # Cases where A and B have different numbers
                    # of children

                    if child_a is None:
                        # B has more children
                        ign = ignored_with_rules(child_b)
                        output.dump(OUT_ADD, child_b, ignored=ign, indent=n+1)
                        if not ign:
                            final_eq = max(final_eq, eq)
                        continue

                    if child_b is None:
                        # A has more children
                        ign = ignored_with_rules(child_a)
                        output.dump(OUT_REM, child_a, ignored=ign, indent=n+1)
                        if not ign:
                            final_eq = max(final_eq, eq)
                        continue

                    #
                    # Terminal cases: 2 leaf nodes
                    #

                    if not child_a.children and not child_b.children:
                        eq, ign = diff_field_with_rules(child_a,
                                                              child_b)
                        if eq == SAME:
                            # doesnt matter which
                            output.dump(OUT_SAME, child_a, indent=n+1)
                        elif eq == MOD:
                            output.print_mod_field(child_a, child_b,
                                                    ignored=ign, indent=n+1)
                            if not ign:
                                final_eq = max(final_eq, eq)
                        else:
                            output.dump(OUT_REM, child_a, ignored=ign,
                                        indent=n+1)
                            output.dump(OUT_ADD, child_b, ignored=ign,
                                        indent=n+1)
                            if not ign:
                                final_eq = max(final_eq, eq)
                        continue


                    #
                    # 1 Leaf node vs 1 Folder node
                    #

                    if not child_a.children or not child_b.children:
                        # doesn't make sense to diff deeper,
                        # consider one has been removed
                        # and the other added

                        ign = ignored_with_rules(child_a) and \
                            ignored_with_rules(child_b)
                        output.dump(OUT_REM, child_a, ignored=ign,
                                    indent=n+1)
                        output.dump(OUT_ADD, child_b, ignored=ign,
                                    indent=n+1)
                        if not ign:
                            final_eq = max(final_eq, DIFF)
                        continue

                    #
                    # Recursive case: 2 Tree nodes
                    #

                    eq = rec_diff(child_a, child_b, n=n+1)
                    final_eq = max(final_eq, eq)

        return final_eq

    rec_diff(pkt_a, pkt_b)
    return output
```

# Bibliography
