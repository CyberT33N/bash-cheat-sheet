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

# log

## echo

### new line
```shell
echo -e "\nStarting port-forward for $target_context:" 🚀
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


# arguments

## start script with arguments

### Example 1
```bash
#!/bin/bash

# Dieses Skript startet die Docker-Container für die Services, die in der `docker-compose.yml` Datei definiert sind.
# Es kann mit dem `--services` Flag aufgerufen werden, um nur bestimmte Services zu starten.
# Beispiel:
# sudo bash .start.sh # Startet alle Services
# sudo bash ./start.sh --service=mongo     # Startet nur den `mongo` Service
# sudo bash ./start.sh --service=mongo,gitlab   # Startet `mongo` und `gitlab` Services
# sudo bash ./start.sh --service=gitlab-runner    # Startet nur den `gitlab-runner` Service


# Parameter parsen
for arg in "$@"
do
    case $arg in
        --services=*)
        services="${arg#*=}"
        shift # Nach dem Parsen des Parameters eine Position nach vorne schieben
        ;;
        *)
        # Unbekannte Parameter oder Fehlerbehandlung hier einfügen
        ;;
    esac
done

# Wenn kein --services Flag gesetzt ist, alle Services starten
if [ -z "$services" ]; then
    echo 'docker-compose up all..'
    sudo docker-compose up -d
    exit 0
fi

# Services verarbeiten und entsprechende Aktionen ausführen
IFS=',' read -ra services_array <<< "$services"
for service in "${services_array[@]}"
do
    case $service in
        mongo)
          echo 'docker-compose up mongo..'
          sudo docker-compose up -d mongo
        ;;
        gitlab)
          echo 'docker-compose up gitlab..'
          sudo docker-compose up -d gitlab
        ;;
        gitlab-runner)
          echo 'docker-compose up gitlab-runner..'
          sudo docker-compose up -d gitlab-runner
        ;;
        *)
          # Fehlerbehandlung für unbekannte Services
          echo "Unbekannter Service: $service"
        ;;
    esac
done
```

### Example #2
```bash
./test.sh red
```

test.sh:
```
#!/bin/bash

if [ $# -eq 0 ]
  then
    echo "No arguments supplied"
fi

if [ "$1" == "red" ]; then
    createProject "red"
elif [ "$1" == "blue" ]; then
    createProject "blue"
elif [ "$1" == "local" ]; then
    createProject "local"
fi
```




<br><br>
<br><br>

## Use flags
```bash
#!/bin/bash

# filename: commandLine.sh
# author: @theBuzzyCoder

showHelp() {
# `cat << EOF` This means that cat should stop reading when EOF is detected
cat << EOF  
Usage: ./installer -v <espo-version> [-hrV]
Install Pre-requisites for EspoCRM with docker in Development mode

-h, -help,          --help                  Display help

-v, -espo-version,  --espo-version          Set and Download specific version of EspoCRM

-r, -rebuild,       --rebuild               Rebuild php vendor directory using composer and compiled css using grunt

-V, -verbose,       --verbose               Run script in verbose mode. Will print out each step of execution.

EOF
# EOF is found above and hence cat command stops reading. This is equivalent to echo but much neater when printing out.
}


export version=0
export verbose=0
export rebuilt=0

# $@ is all command line parameters passed to the script.
# -o is for short options like -v
# -l is for long options with double dash like --version
# the comma separates different long options
# -a is for long options with single dash like -version
options=$(getopt -l "help,version:,verbose,rebuild,dryrun" -o "hv:Vrd" -a -- "$@")

# set --:
# If no arguments follow this option, then the positional parameters are unset. Otherwise, the positional parameters 
# are set to the arguments, even if some of them begin with a ‘-’.
eval set -- "$options"

while true
do
case "$1" in
-h|--help) 
    showHelp
    exit 0
    ;;
-v|--version) 
    shift
    export version="$1"
    ;;
-V|--verbose)
    export verbose=1
    set -xv  # Set xtrace and verbose mode.
    ;;
-r|--rebuild)
    export rebuild=1
    ;;
--)
    shift
    break;;
esac
shift
done
```

```bash
# With short options grouped together and long option
# With double dash '--version'

bash commandLine.sh --version=1.0 -rV
# With short options grouped together and long option
# With single dash '-version'

bash commandLine.sh -version=1.0 -rV

# OR with short option that takes value, value separated by whitespace
# by key

bash commandLine.sh -v 1.0 -rV

# OR with short option that takes value, value without whitespace
# separation from key.

bash commandLine.sh -v1.0 -rV

# OR Separating individual short options

bash commandLine.sh -v1.0 -r -V
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

#### create multiple folder
```bash
mkdir -p {dev,test,prod}/{backend,frontend}
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

# tail

## real-time monitoring files
Using the tail -f command, you can follow the changes in the log file as they happen. This allows you to see new lines appended to the file without needing to re-run the command.
```shell
tail -f error_file.log
```

If you want to follow a log file in real-time but only care about specific entries, you can combine tail -f with grep. For example:
```shell
tail -f error_file.log | grep "ERROR"
```








<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>

# touch

## Create multiple files number increasing
```shell
touch test{1..10}.txt
```











<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


## cd

## cd current directory
```bash
cd "$(dirname "$0")"; printf "\nCurrent working directory:"; pwd
```

<br><br>


## cd last directory
```bash
cd -
```

























<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>


# regex

<br><br>



## check if variable start with string
```bash
TEST="helm template 123"
echo $TEST

if [[ $TEST =~ ^"helm template" ]]; then
     echo "Trigger mongodb-data backup"
fi
```


<br><br>
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
# method 1 (https://askubuntu.com/questions/484993/run-command-on-anothernew-terminal-window)
gnome-terminal --title="SOME WINDOW TITLE HERE" -- sh -c "any code here"

# method 2 (you can basicly use any terminal emulator for this)
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

## for loop with path
```bash
# example 1
cd test/data

for file in *
do
   # do whatever on $i
done




# example 2
for file in test/data/*
do
   # do whatever on $i
done
```



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
# method #1
REGEX='"log":"([^"]+)"'

for jsonfile in $(find /logs -name '*_0'); do
     resultNames=$(cat $jsonfile | grep -ohE $REGEX)
     printf "\n Links of the matched s3 files: $resultNames"
done;














# method #2
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








# method #3
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

## check if variable exists
``` bash
if [[ "$phone_missing" ]]; then
    echo "phone_missing does exists"
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











































