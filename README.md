# LoBue_Project2

## **MOTIVATION** 
This script was written to calculate the GC content of all the sequences in any given file. In order to run this script 
you must include the file containing the sequences as a command line argument. Also, there is a limit of one argument per execution.
### **ARGUMENT VALIDATION**
  The first "if, else" statement is used to confirm only one argument was included with the script. If there is only one argument 
  you will get a message saying "Correct number of arguments. If number of arguments is not one than you will get an error message, 
  "Wrong number of arguments.
  
    #! /bin/bash
  
    ##check to see the number of arguments####
    if [ $# -eq 1 ] ; then
      echo "Correct number of arguments"
    else
      echo "Wrong number of arguments"
    fi

  This "if, else" statement is used to confirm the argument included with the script actually exists and has content. If this is not the 
  case then you will get an error, "Include file as arguement when executing." If the argument does exist and has content then you will recieve a message 
  saying, "orrect arguement input".

    ###Check for correct command line arguement and count sequences####
    if [ -e $1 ] ; then
      echo "Correct arguement input"
    else
      echo "Include file as arguement when executing."
    fi

### **VARIABLE ASSIGNMENT**
  Here we will assign variables for the number of sequences included in the given file which will help later in the script. 
  We also assign variables to the name of each sequence and to the sequence itself. This will make it easier when calculating the GC count and when
  moving these outputs to the output file.
  
    ###create variables for the number of sequences, the names of the sequences,
    ###and the sequences

    SN=$(grep '>' $1 | wc -l)
    #echo $SN

    names=($(grep '>' $1))
    #echo "${names[2]}"

    seq=($(grep -v '>' $1))
    #echo "${seq[0]}"

### CALCULATING GC CONTENT
  The first step in this section is to create the output file and input the headers for each column of this file. 
  Next we need to create a loop to run as many times as there are sequences in the file. This is where we utilize the 
  variable for the number of sequences in the file created earlier. Within each loop we want the script to count the total number of 
  each nucleotide per sequence and then calculate the percentage of G's and C's and move this information to the output file. 
  
    ###Create the output file and insert the header###
      touch GCcount.txt
      echo -e "Sequence name\tGC percentage" > GCcount.txt

    ###create for loop to count the GC content for each sequence in the
    ###given file and export this data to the output file
    for (( i=0; i<=$SN; i++ ))
    do

    G=($( echo "${seq[$i]}" | grep -o -e "G" | wc -l ))
    C=($( echo "${seq[$i]}" | grep -o -e "C" | wc -l ))
    A=($( echo "${seq[$i]}" | grep -o -e "A" | wc -l ))
    T=($( echo "${seq[$i]}" | grep -o -e "T" | wc -l ))
    GC=`expr $G + $C`
    ALL=`expr $GC + $A + $T`
    #echo $GC
    #echo $ALL
    GCpercent=`expr $GC \* 100 / $ALL`
    #echo $GCpercent
    echo -e "${names[$i]}\t$GCpercent%" >> GCcount.txt

    done
    
### **OUTPUT TABLE**

| Sequence name |	GC percentage |
| ------------ |  ------------- |
| >DI245396.1 |	43% |
| >DI245395.1 |	42% |
| >HW262829.1 |	43% |
| >546218138 |	42% |
| >X13802.1 |	39% |
| >NM_001179558.3 |	51% |
| >NM_001178613.2 |	45% |
| >AY558240.1 |	51% |
| >AB052924.1 |	51% |

### **REFLECTION**

_Why do you think that many biologists skip the step of making their code publicly available and adding documentation?_

In my opinion, most people who devote a lot of time and effort into a product or service do not want to give away said product or service for free. Although in making ones code public they will recieve credit for the code where ever it is used, I do not think it is worth the time and effort for most people to take the extra step in making there script publicly available. 

_Why do you think I asked you to write sanity checks into your code? Do you see room for additional sanity checks to be added?_

I assume the reasoning for the sanity checks was to help us understand the different parts of the script. It is a great tool to add sanity checks because it can tell you exactly where the script is malfunctioning, simplifying the troubleshooting process. There is room for many more sanity checks. I made a sanity check after each one of my variables in order to confirm their operation. 

_In my experience, loops are the hardest concept for students to learn. Why do you think this is? What struggles did you have when trying to write a loop into your code?_

Loops are difficult, but I am not sure if they are more difficult than any other coding element. One of the hardest parts for me is knowing the correct syntax and utilizing the correct flags. "The (), {}, [], "", '', and backtick characters are all similiar to me and it is tough to understand when to use each one. 

### ACKNOWLEDGEMENT
Although this was a very tough project I really enjoyed it! It is very rewarding to learn this new skill as it is very important to my current work and most likely will have a major imapact on my work for the rest of my life. It does take a lot of time and struggle to learn this new skill but it is worth it!

Thank you Amanda and Dr. Stevison for being availbale and reponding quickly to all of my help requests, I would not have been able to complete this project without your help.
