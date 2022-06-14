# Bash Cheat Sheet
Bash Cheat Sheet with the most needed stuff..



<br><br>



Nice2Know
- You can not change the values of parent shell variables when you inside of sub shells. As example:
```bash
# Not working
AmountOfMatches=0
ConvertAndReplace() {
    for d in *; do

        if [ -d "$d" ]; then
          (cd -- "$d" && ConvertAndReplace) # <-- Created sub shell so we can later not re-assign AmountOfMatches
        else

          for f in *.bson;
          do
            resultAmount=$(cat $JSONfile | grep -E $RegexMatchS3Host | wc -l)
            AmountOfMatches=$(($AmountOfMatches + $resultAmount))
            printf "\n Inside check: $AmountOfMatches \n\n"
          done

        fi

    done
}
ConvertAndReplace
printf "\n Amount of files where we found s3 links: $AmountOfMatches"






# Working
AmountOfMatches=0
ConvertAndReplace() {
    for d in *; do

        if [ -d "$d" ]; then
          pushd "$d"
          ConvertAndReplace
          popd
        else

          for f in *.bson;
          do
            resultAmount=$(cat $JSONfile | grep -E $RegexMatchS3Host | wc -l)
            AmountOfMatches=$(($AmountOfMatches + $resultAmount))
            printf "\n Inside check: $AmountOfMatches \n\n"
          done

        fi

    done
}
ConvertAndReplace
printf "\n Amount of files where we found s3 links: $AmountOfMatches"
```







































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>





# string

<br><br>

## convert string to number
```bash
AmountOfMatches=0
result="2"
AmountOfMatches=$(($AmountOfMatches + $result))
```





















<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>



# variable

<br><br>

## create variable
```bash
AmountOfMatches=1
Names="Peter"
```

<br><br>

## re-assign variable
```bash
AmountOfMatches=1
AmountOfMatches=2
```


<br><br>

## create variable with command inside
```bash
isDirectory=$([[ -d $SourcePath ]] && echo true)
```

<br><br>

## create variable with command inside and run as sudo
```bash
isDirectory=$(sudo bash -c "[[ -d $SourcePath ]] && echo true")
```





































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


# functions

<br><br>

## Passing String as Argument
```bash
PATH='/mnt/temp'

coolFunction(){
cd "$1"
printf "We will display now the current directory used:"; pwd
}

coolFunction $PATH
```

<br><br>

## Passing Array as Argument
```bash
# ---- Method #1 -----
array=("one" "two" "three")

createPath() {
  for path in "$@"
  do
    printf "\n Current path: $path"
  done
}

createPath "${array[@]}"






# ---- Method #2 -----
array=("one" "two" "three")

createPath() {
  arr=("$@")
  for path in "${arr[@]}"
  do
    printf "\n Current path: $path"
  done
}

createPath "${array[@]}"
```






































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# number

<br><br>

## increase number
```bash
AmountOfMatches=1

for link in "${array[@]}"
do
  let "AmountOfMatches++"
  printf "\n AmountOfMatches: $AmountOfMatches"
done
```
















































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>



# cd

<br><br>

## cd to home directory
```bash
# method #1
cd --

# method #2
cd ~
```

<br><br>

## cd to specific path and then later go back to path where you cd
```bash
pushd ./anypath
ConvertAndReplace
popd
```
















































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# read file
```bash
cat file | while read line
do
  echo "a line: $line"
done
```

<br><br>

## read file into variable
```bash
#!/bin/bash
value=$(<config.txt)
echo "$value"
```


































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# find

<br><br>

## find amount of files recursive
```bash
find . -type f | wc -l
```














<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# shebang
- Put this on the first line of your script to tell in which way you want to execute the script.
```bash
#!/bin/sh — Execute the file using sh, the Bourne shell, or a compatible shell
#!/bin/csh — Execute the file using csh, the C shell, or a compatible shell
#!/usr/bin/perl -T — Execute using Perl with the option for taint checks
#!/usr/bin/php — Execute the file using the PHP command line interpreter
#!/usr/bin/python -O — Execute using Python with optimizations to code
#!/usr/bin/ruby — Execute using Ruby
#!/bin/ksh
#!/bin/awk
#!/bin/expect
```








































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# delete

## remove files/folder
```bash
git rm -f sample.txt
git rm -rf folder
```































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# create

<br><br>

## folder

<br><br>

#### create folder
```bash
mkdir /anyfolder
```

<br><br>

#### create folder recursive
```bash
mkdir -p /anyfolder/path/here
```
































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


# comment

<br><br>

## comment single line
```bash
# any comment here..
```

<br><br>


## comment multiple lines
```bash
: '
lorum ipsum
sample text..
'
```






























<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


## cd current directory
```bash
cd "$(dirname "$0")"; printf "\nCurrent working directory:"; pwd
```

























<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


# regex

<br><br>

## use in condition
```bash
BRANCH_REGEX="^(develop$|release//*)"
if [[ $BRANCH =~ $BRANCH_REGEX ]];
then
    echo "BRANCH '$BRANCH' matches BRANCH_REGEX '$BRANCH_REGEX'"
else
    echo "BRANCH '$BRANCH' DOES NOT MATCH BRANCH_REGEX '$BRANCH_REGEX'"
fi



# example #2 - If regex does not match
BRANCH_REGEX="^(develop$|release//*)"
if [[ ! $BRANCH =~ $BRANCH_REGEX ]];
then
    echo "BRANCH '$BRANCH' DOES NOT MATCH BRANCH_REGEX '$BRANCH_REGEX'"
else
    echo "BRANCH '$BRANCH' matches BRANCH_REGEX '$BRANCH_REGEX'"
fi
```


<br><br>

## match something and return to variable
```bash
[[ $line =~ \/(.*).git ]]
printf "\n match: ${BASH_REMATCH[0]}";
```




<br><br>

## match something in file
```bash
resultNames=$(cat $JSONfile | grep -ohE $RegexMatchS3)
printf "\n Links of the matched s3 files: $resultNames"
```





























<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>



# match

<br><br>


## match something with regex and return the matched group
```bash
text="'fqwfqwfdqwf 33 wfefewfwfebla6666666666666666666666 bla bla|bla|bla'|fwefwefwefwef3"
string="'bla6666666666666666666666 bla bla|bla|bla'"
s=`echo $text | grep -Eo "'.*\|.*'"`
echo 's'$s
```






<br><br>
<br><br>




## grep


<br><br>

#### Match specific text in while and show aswell 4 lines before
```bash
grep -B 4 '40413531' /home/username/export_online_b-edited.tsv
```
<br><br>


















<br><br>


<br><br>



# Replace

<br><br>

## sed


<br><br>

#### Remove white space with character class
```bash
sed -r -e "/'.*\|.*'/s/\|/-/g" < $DUMB >> 'temp.csv'
```





<br><br>

#### Remove last character from string
```bash
remove last character
${VARIABLEHERE%?}

remove the last 2 characters
${VARIABLEHERE%??}
```




<br><br>

#### If you want to use the matched group from your sed command you must use extended regex with the r flag
```bash
sed -E 's/[[:space:]]+/,/g' orig.txt > modified.txt

# The character class [:space:] will match all whitespace (spaces, tabs, etc.). If you just want to replace a single character, eg. just space, use that only.
# EDIT: Actually [:space:] includes carriage return, so this may not do what you want. The following will replace tabs and spaces.

sed -E 's/[[:blank:]]+/,/g' orig.txt > modified.txt

# as will

sed 's/[\t ]+/,/g' orig.txt > modified.txt
```

<br><br>

#### When you work with big data never save the result in a variable and then do other operations with it. Save the current work data to a file and then read it again
```bash
sed -r -e 's/.*/\1/g')
```

<br><br>

#### work with variable and save to file
```bash
sed -r -e "/'.*\|.*'/s/\|/-/g" $DUMB > 'temp.csv'
```


<br><br>

#### work with filee and save to file
```bash
sed -r -e "/'.*\|.*'/s/\|/-/g" < './file.csv' >> 'temp.csv'
```












<br><br>
<br><br>


## Replace text and create new variable
```bash
# method #1 - Using tr and save stream to file
tr "|" "," < '/home/a.csv' > '/home/a-edited.csv'

# method #2
PROJECTNAME=$(echo ${BASH_REMATCH[0]} | sed 's/oldtext/newtext/g')

# use regex
PROJECTNAME=$(echo ${BASH_REMATCH[0]} | sed -E 's/^coolregex$/newtext/g')

# extend matched stuff with something
PROJECTNAME=$(echo ${BASH_REMATCH[0]} | sed -E 's/^coolregex$/\1 added text here/g')

# use regex and variable - We can use double quotes to be able to use variable
REGEXHERE="^coolregex$"
PROJECTNAME=$(echo ${BASH_REMATCH[0]} | sed -E "s/$REGEXHERE/newtext/g")

# Use multiple statements
PROJECTNAME=$(echo ${BASH_REMATCH[0]} | sed 's/ab/~~/g; s/bc/ab/g; s/~~/bc/g')


# save the matched text to variable
string="'bla6666666666666666666666 bla bla|bla|bla'"
s=`echo $string | sed -E "s/'.*[|].*'/&/g"` # You can use \1 only when you make group matching in regex. e.q. ('.*[|].*')
echo $s
```


<br><br>


## match string and then replace with another statment
```bash
#!/bin/sh
text="'fqwfqwfdqwf 33 wfefewfwfebla6666666666666666666666 bla bla|bla|bla'|fwefwefwefwef3"
string="'fqwfqwfdqwf 33 wfefewfwfebla6666666666666666666666 bla bla|bla|bla'"

# the first statement is the
result=`echo $text | sed -r -e "/'.*\|.*'/s/\|/-/g"`
echo 'result:'$result
```
























<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

## Open script in new window
```bash
# method 1 (you can basicly use any terminal emulator for this)
gnome-terminal -- yourscript.sh
```




































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


# variables
```bash
INTRO=./intros/*
CPUCORES=1
```

<br><br>


# concate variables
```bash
tempDIR="$PROJECTNAME-$DATE"
```
<br><br>

  

## create variable of echo
```bash
a=$(echo '111 222 33' | awk '{print $3;}' )
```

<br><br>


## use home path in variable (**$HOME**)
```bash
EXPORT_PATH="$HOME/Documents"
cd "$EXPORT_PATH"
```































<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# Iterate

## for loop array
```bash
for i in "${arrayName[@]}"
do
   # do whatever on $i
done
```

<br><br>

## parallel (async) for loop

```bash
test(){
  sample="sample.."
  sample2="$1"
  # and other complex stuff..
}

for d in "${arrayName[@]}"
do
  test $d &
done
wait
```

<br><br>

## Select files from folder
```bash

SRC=/home/username/Downloads/vids/*

for FILE in $SRC
do

        filepath=$FILE
        echo "Current file path: $filepath"

        filename=$(basename -- "$FILE")
        echo "Current FULL file name: $filename"

        extension=${filename##*.}
        echo "Current file extension: $extension"


done
echo "font for loop was done.."
```





<br><br>

## Select files from folder recursive
```bash
#!/bin/sh
PROJECTPATH=~/Projects/gitlab/example

recursiverm() {
  for d in *;
  do
    if [ -d "$d" ]; then
      (cd -- "$d" && recursiverm)
    fi
    echo "Current file path: "; pwd

    filename=$(basename -- "$d")
    echo "Current FULL file name: $filename"

    extension=${filename##*.}
    echo "Current file extension: $extension"
  done
}

(cd $PROJECTPATH; recursiverm)








# method #2
: '
-r or -R is recursive,
-n is line number, and
-w stands for match the whole word.
-l (lower-case L) can be added to just give the file name of matching files.
-e is the pattern used during the search
'

S3REGEX="s3[-]eu[-]central"
MATCHES="$(grep --exclude=\*.{jpg,png} -rnw $PROJECTPATH -e $S3REGEX)"

echo $MATCHES | while read line
do
    echo "Current line: $line"
    #echo "Current file path: "; pwd

    filename=$(basename -- "$1")
    #echo "Current FULL file name: $filename"

    extension=${filename##*.}
    #echo "Current file extension: $extension"

done
```













<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# Conditions





<br><br>

## if else
``` bash
# method 1
if [ $introname = "green" ]; then

                   AUDIOSRC=./audio/delay/green/*
                   echo "Current audio source folder: $AUDIOSRC"

elif [ $introname = "blue" ]; then

                   AUDIOSRC=./audio/delay/blue/*
                   echo "Current audio source folder: $AUDIOSRC"

else

                   AUDIOSRC=./audio/delay/red/*
                   echo "Current audio source folder: $AUDIOSRC"

fi

# method 2 - 1 liner if else
[ -d $REPONAME ] && repoExist || repoNotExist;
```

<br><br>

## if else multiple conditions
- You can use either [[ or (( keyword. When you use [[ keyword, you have to use string operators such as -eq, -lt. I think, (( is most preferred for arithmetic, because you can directly use operators such as ==, < and >.
``` bash
# Using [[ operator
a=$1
b=$2
if [[ a -eq 1 || b -eq 2 ]] || [[ a -eq 3 && b -eq 4 ]]
then
     echo "Error"
else
     echo "No Error"
fi


# Using (( operator
a=$1
b=$2
if (( a == 1 || b == 2 )) || (( a == 3 && b == 4 ))
then
     echo "Error"
else
     echo "No Error"
fi
```

<br><br>

## check if value is bla or not
``` bash
phone_missing=false
if [ "$phone_missing" != false ]; then
    echo "phone_missing is not 'false' (but may be non-true, too)"
fi
if [ "$phone_missing" == true ]; then
    echo "phone_missing is true."
fi
```





<br><br>

## check if variable is longer than specific character limit
``` bash
if [[ ${#detect} -gt 8 ]] ; then
    echo "Greater than 8 characters we exit now..."
    exit 1
else
    echo "Good to go..."
    exit 0
fi
```

<br><br>













<br><br>

## check if folder exist (-d)
``` bash
# method 1
if [ -d "$PROJECTNAME" ]
  then printf "\n Repo $PROJECTNAME folder already exist..\n"
  else printf "\n Repo $PROJECTNAME folder does not exist..\n"
fi

# method 2
[ -d $PROJECTNAME/.git ] && rm -rf $PROJECTNAME/.git;
```

<br><br>

## check if file exist (-f)

``` bash
# method 1 (-f)
if [ -f "sample.txt" ]
  then printf "\n sample.txt already exist..\n"
  else printf "\n sample.txt does not exist..\n"
fi


# method 1 (-s) | You should use -s when you work with variables - FILE exists and has a size greater than zero
if [ -s "$file" ]
  then printf "\n sample.txt already exist..\n"
  else printf "\n sample.txt does not exist..\n"
fi





# method 2
[ -f $PROJECTNAME/.sample.txt ] && rm -f $PROJECTNAME/.sample.txt;
```

<br><br>

## check if folder not exist
``` bash
if [ ! -d "$PROJECTNAME" ]
  then printf "\n Repo $PROJECTNAME folder does not exist..\n"
  else printf "\n Repo $PROJECTNAME folder already exist..\n"
fi
```






























<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# Array

<br><br>

## Create array
```bash
# method 1
declare -a FontColor=()

#method 2
FontColor[0]='yellow'
FontColor[1]='white'
FontColor[2]='green'
FontColor[3]='red'
FontColor[4]='blue'
FontColor[5]='purple'
```

<br><br>


## Print array
```bash
printf "\n Names of matched s3 links: ${matchedNamesAR[*]}"
```

<br><br>




## Access specific element from array
```bash
string="1 2 3 4 5"
declare -a array=($string)
printf "\n First element: ${array[0]}" # will print 1
```



<br><br>

## Delete duplicates from array
```bash
clearedarray=( `for i in ${unclearedarray[@]}; do echo $i; done | sort -u` )
```

<br><br>

## Create array from string
```bash
# split by spaces
string="1 2 3 4 5"
declare -a array=($string)

# split by custom character
string="1,2,3,4,5"
delimiter=","
declare -a array=($(echo $string | tr "$delimiter" " "))
```

<br><br>

## Push items into array
```bash
    SRC=/home/usernamehere/Downloads/vidz/*
    for INTRO in $SRC
    do

              introAR+=( "$INTRO" )

    done
```

<br><br>

## get random item from array
```bash
rand=$[$RANDOM % ${#introAR[@]}]
intropath=${introAR[$rand]}
introname=$(basename -- "$intropath")
```
