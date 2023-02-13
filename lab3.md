# Lab 3 Lab Report

## Command
`grep`

I am using `man grep` to get options for the `grep` command.

Using `-l` just to find the file names. I am just showing `STDOUT`.

### Options
`-i, --ignore-case`
`-c, --count`
`-r, --recursive`
`-w, --word-regexp`

#### `-i` Examples
`grep "flamingo" written_2/*/*/* -l`
Output
```
written_2/travel_guides/berlitz1/WhatToMadeira.txt
written_2/travel_guides/berlitz1/WhereToFrance.txt
written_2/travel_guides/berlitz2/Bahamas-WhereToGo.txt
written_2/travel_guides/berlitz2/Bermuda-WhereToGo.txt
written_2/travel_guides/berlitz2/CostaBlanca-WhatToDo.txt
written_2/travel_guides/berlitz2/Costa-WhereToGo.txt
written_2/travel_guides/berlitz2/Cuba-WhereToGo.txt
```
`grep "flamingo written_2/*/*/* -il`

```
written_2/travel_guides/berlitz1/HistoryLasVegas.txt
written_2/travel_guides/berlitz1/WhatToMadeira.txt
written_2/travel_guides/berlitz1/WhereToFrance.txt
written_2/travel_guides/berlitz2/Bahamas-WhereToGo.txt
written_2/travel_guides/berlitz2/Bermuda-WhereToGo.txt
written_2/travel_guides/berlitz2/Cancun-WhereToGo.txt
written_2/travel_guides/berlitz2/CostaBlanca-WhatToDo.txt
written_2/travel_guides/berlitz2/Costa-WhereToGo.txt
written_2/travel_guides/berlitz2/Cuba-WhereToGo.txt
written_2/travel_guides/berlitz2/Vallarta-History.txt
written_2/travel_guides/berlitz2/Vallarta-WhatToDo.txt
```
I found this option through the man page of `grep`.
This option is useful in `grep` if you want to find a word but do not know its captialization
or if you want to know the number of times a word comes up regardless of its Case.
This code looks for the text `flamingo` with any type of cases like `Flamingo` or `FLAMINgO`.

#### Example 2
```
grep "Luca" written_2/*/*/* -il
written_2/travel_guides/berlitz1/WhatToIstanbul.txt
written_2/travel_guides/berlitz1/WhereToFrance.txt
written_2/travel_guides/berlitz1/WhereToItaly.txt
written_2/travel_guides/berlitz1/WhereToMallorca.txt
written_2/travel_guides/berlitz2/Bahamas-History.txt
written_2/travel_guides/berlitz2/Bahamas-Intro.txt
written_2/travel_guides/berlitz2/Bahamas-WhatToDo.txt
written_2/travel_guides/berlitz2/Bahamas-WhereToGo.txt
written_2/travel_guides/berlitz2/Berlin-WhereToGo.txt
written_2/travel_guides/berlitz2/Costa-WhereToGo.txt
```
grep "Luca" written_2/*/*/* -l
written_2/travel_guides/berlitz1/WhereToFrance.txt
written_2/travel_guides/berlitz1/WhereToItaly.txt
written_2/travel_guides/berlitz2/Bahamas-History.txt
written_2/travel_guides/berlitz2/Bahamas-Intro.txt
written_2/travel_guides/berlitz2/Bahamas-WhatToDo.txt
written_2/travel_guides/berlitz2/Bahamas-WhereToGo.txt
written_2/travel_guides/berlitz2/Berlin-WhereToGo.txt
```

With `-i` we can see that we find more files where "Luca" matches. 
Similar to above, it looks for any casing of Luca. 
This is nice to find any occurence of a word and not a specific casing.

#### Count Examples

```
grep "yes" written_2/non-fiction/OUP/Berk/* -c

written_2/non-fiction/OUP/Berk/ch1.txt:3
written_2/non-fiction/OUP/Berk/ch2.txt:2
written_2/non-fiction/OUP/Berk/CH4.txt:3
written_2/non-fiction/OUP/Berk/ch7.txt:0
```
This code uses `-c` which counts the number of occurences of a word in the file and prints the number of matches in the line.
This is useful for a quick look at some files as well as get the number of matches in each file.

#### Example 2
```
grep "no" written_2/non-fiction/OUP/Berk/* -c
written_2/non-fiction/OUP/Berk/ch1.txt:89
written_2/non-fiction/OUP/Berk/ch2.txt:81
written_2/non-fiction/OUP/Berk/CH4.txt:91
written_2/non-fiction/OUP/Berk/ch7.txt:63
```
This code is great to do a specific word count as you can extract these numbers later with regular expressions.
It just counts the number of occurences of this string in the files.
This was found with `man grep`.

#### Recursive
```
grep "yes" written_2/non-fiction/OUP/ -rl
written_2/non-fiction/OUP/Abernathy/ch14.txt
written_2/non-fiction/OUP/Abernathy/ch3.txt
written_2/non-fiction/OUP/Abernathy/ch8.txt
written_2/non-fiction/OUP/Berk/ch1.txt
written_2/non-fiction/OUP/Berk/ch2.txt
written_2/non-fiction/OUP/Berk/CH4.txt
written_2/non-fiction/OUP/Castro/chA.txt
written_2/non-fiction/OUP/Castro/chB.txt
written_2/non-fiction/OUP/Castro/chM.txt
written_2/non-fiction/OUP/Castro/chN.txt
written_2/non-fiction/OUP/Castro/chP.txt
written_2/non-fiction/OUP/Castro/chR.txt
written_2/non-fiction/OUP/Castro/chZ.txt
written_2/non-fiction/OUP/Fletcher/ch1.txt
written_2/non-fiction/OUP/Fletcher/ch10.txt
written_2/non-fiction/OUP/Fletcher/ch5.txt
written_2/non-fiction/OUP/Kauffman/ch1.txt
written_2/non-fiction/OUP/Kauffman/ch10.txt
written_2/non-fiction/OUP/Kauffman/ch3.txt
written_2/non-fiction/OUP/Kauffman/ch4.txt
written_2/non-fiction/OUP/Kauffman/ch5.txt
written_2/non-fiction/OUP/Kauffman/ch6.txt
written_2/non-fiction/OUP/Kauffman/ch8.txt
written_2/non-fiction/OUP/Rybczynski/ch1.txt
```
This is really useful if you don't want to use filecards and want to search the entire directory easily.
This allows `grep` to search all the sub-directories.

```
grep "Lucayans" written_2/ -rl
written_2/travel_guides/berlitz2/Bahamas-History.txt
```
Using `grep -r` allows me to search the entire directory without using wildcards over and over again to traverse the directories.
This will expand all the directories and recursively search sub-directories.
This was found with `man grep`.

#### -w Word
```
grep "yes" written_2/ -wlr
written_2/non-fiction/OUP/Abernathy/ch14.txt
written_2/non-fiction/OUP/Berk/ch1.txt
written_2/non-fiction/OUP/Berk/CH4.txt
written_2/non-fiction/OUP/Fletcher/ch1.txt
written_2/non-fiction/OUP/Fletcher/ch10.txt
written_2/non-fiction/OUP/Kauffman/ch1.txt
written_2/non-fiction/OUP/Kauffman/ch10.txt
written_2/non-fiction/OUP/Kauffman/ch3.txt
written_2/non-fiction/OUP/Kauffman/ch4.txt
written_2/non-fiction/OUP/Kauffman/ch5.txt
written_2/non-fiction/OUP/Kauffman/ch6.txt
written_2/non-fiction/OUP/Kauffman/ch8.txt
written_2/travel_guides/berlitz1/IntroMadrid.txt
written_2/travel_guides/berlitz1/WhatToLasVegas.txt
written_2/travel_guides/berlitz1/WhereToIndia.txt
written_2/travel_guides/berlitz1/WhereToItaly.txt
written_2/travel_guides/berlitz2/Bahamas-WhatToDo.txt
```
This option to get exact words and not just matches for characters. This is useful if a word is part of a lot of other words and you wan tto get the exact matches.
It searches for the word itself and not jusjt the string of characters.

```
grep "123" written_2/ -wlr
written_2/non-fiction/OUP/Berk/ch2.txt
written_2/travel_guides/berlitz1/HistoryIndia.txt
written_2/travel_guides/berlitz1/HistoryMallorca.txt
written_2/travel_guides/berlitz1/WhatToIbiza.txt
written_2/travel_guides/berlitz1/WhatToLosAngeles.txt
written_2/travel_guides/berlitz1/WhereToLosAngeles.txt
written_2/travel_guides/berlitz2/Athens-WhereToGo.txt
written_2/travel_guides/berlitz2/Berlin-WhereToGo.txt
written_2/travel_guides/berlitz2/Boston-WhereToGo.txt
written_2/travel_guides/berlitz2/California-WhatToDo.txt
written_2/travel_guides/berlitz2/California-WhereToGo.txt
written_2/travel_guides/berlitz2/China-WhereToGo.txt
written_2/travel_guides/berlitz2/Poland-History.txt
```
Similarly this looks for a word starting with `123` exact and looks at the lines that it is in.
This is great if you want to search for an exact number instead of the sequence of numbers in the decimal or inside a number.
This was found from the `man grep` page. 
