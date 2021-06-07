# shellbee 🐚🐝
A collection of shell utility functions written by a noob in shell scripting. These are tested on Ubuntu 20.04. 

## Purpose
These were written by me when automating a big ass setup. This was my first time ever doing bash scripting and thus the noob thing. Obviously, I took help from StackOverflow as well. These functions came in handy at many places in the whole workflow. Also, there are many ways to implement what these functions provide. Truth be told, there might be many native commands too which can replace or shorten some of the implementations but I decided it's better to have them saved on the Internet for my benefit. 

**Note** - The name of functions might feel verbose to people who know the underlying short native commands well. I personally like function names which can give subtle hints of what they are trying to achieve.

## Functions
In the conditional functions I have chosen to return `"true"` or `"false"` since I like this pattern of comparing function outputs to booleans/boolean strings for further decisions. You can totally customize this as per your preferences.
    
### configureAsAService
A function to configure a service for non-root user.

<details>
  <summary>Click to expand</summary>
  
```sh
function configureAsAService() {
  local service=$1;
  local serviceFilePath=$2;
  if [ $(compgen -G ~/.config/systemd/user) ]
     then
     echo "~/.config/systemd/user already exists, cleaning previous $service file if it exists"
     rm ~/.config/systemd/user/$service.service 2>/dev/null
     else
     echo "Creating ~/.config/systemd/user directory"
     mkdir -p ~/.config/systemd/user
  fi
  cp $serviceFilePath ~/.config/systemd/user
  systemctl --user daemon-reload
  systemctl --user enable $service
}
```
</details>

### getTextBetween
A function to get text between two words in a phrase of a file.

<details>
  <summary>Click to expand</summary>

```sh
function getTextBetween() {
  local start=$1
  local end=$2
  local phrase=$3
  local file=$4
  echo $(grep "$phrase" "$file" | awk -v FS="$start|$end" '{print $2}')
}
```
</details>

### isDebPackageInstalled
A function to check if a deb package is installed or not.

<details>
  <summary>Click to expand</summary>

```sh
function isDebPackageInstalled() {
  local software=$1
  local checkString=$(dpkg -s $software 2>/dev/null | grep Status)
  if [ "$checkString" == "Status: install ok installed" ]
     then echo "true"
     else echo "false"
  fi
}
```
</details> 

### isResourceAvailable
A function to check if a resource, for instance a file/directory is available at the designated path or not.

<details>
  <summary>Click to expand</summary>

```sh
function isResourceAvailable() {
  local path=$1;
  if [ $(compgen -G "$path") ]
     then echo "true"
     else echo "false"
  fi
}
```
</details>

### isServiceActive
A function to check if a service is active or not for non-root user.

<details>
  <summary>Click to expand</summary>

```sh
function isServiceActive() {
    local service=$1
    if [ "$(systemctl --user show -p ActiveState --value $service)" = "active" ]
       then echo "true"
       else echo "false"
    fi
}
```
</details>    

### promptLoop
A function which keeps prompting the user with question until valid options are entered. 

Depends on `promptUser` for implementation.

<details>
  <summary>Click to expand</summary>

```sh
function promptLoop() {
    local promptString=$1
    local option1=$2
    local option2=$3
    local isNotValid="true"
    local promptOutput=""
    while [ "$isNotValid" = "true" ]
    do
    promptOutput="$(promptUser "$promptString" 'r')"
    if [[ "$promptOutput" = "$option1" || "$promptOutput" = "$option2" ]]
       then isNotValid="false"
    fi
    done
    echo "$promptOutput"
}
```
</details>     
    
### promptUser
A function to take input from user.

<details>
  <summary>Click to expand</summary>

```sh
function promptUser() {
   local question=$1;
   local mode=$2;
   # r mode means the user can see what they type
   if [ "$mode" = "r" ]
      then read -p "$question" reply
   # s mode means the user can not see what they type
   elif [ "$mode" = "s" ]
      then read -sp "$question" reply
   else 
      echo "Wrong mode $mode, should be either of r or s"
      exit
   fi

   echo "$reply"
}
```
  
</details>  

### startService
A function to start a service as non-root user.

<details>
  <summary>Click to expand</summary>

```sh
function startService() {
  local service=$1
  systemctl --user start $service
}
```
</details>
  
### stopService
A function to stop a service as non-root user.

<details>
  <summary>Click to expand</summary>

```sh
function stopService() {
  local service=$1
  systemctl --user stop $service
}
```
</details>

### trimQuotes
A function to trim the leading and trailing quotes of a value.

<details>
  <summary>Click to expand</summary>

```sh
function trimQuotes() {
  local trimmed="$1"

  # Strip leading quotes
  trimmed="${trimmed%\"}"

  # Strip trailing quotes
  trimmed="${trimmed#\"}"

  echo "$trimmed";
}
```
  
</details>  

### trimSpaces
A function to trim the leading and trailing spaces of a value.

<details>
  <summary>Click to expand</summary>

```sh
function trimSpaces() {
  local trimmed="$1"

  # Strip leading spaces
  while [ $trimmed = ' '* ]; do
   trimmed="${trimmed## }"
  done

  # Strip trailing spaces
  while [ $trimmed = *' ' ]; do
   trimmed="${trimmed%% }"
  done

  echo "$trimmed";
}
```
</details>

