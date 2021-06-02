# shellbee 🐚🐝
A collection of shell utility functions written by a noob in shell scripting. These are tested on Ubuntu 20.04. 

## Purpose
These were written by me when automating a big ass setup. This was my first time ever doing bash scripting and thus the noob thing. Obviously, I took help from StackOverflow as well. These functions came in handy at many places in the whole workflow. Also, there are many ways to implement what these functions provide. Truth be told, there might be many native commands too which can replace or shorten some of the implementations but I decided it's better to have them saved on the Internet for my benefit. 

**Note** - The name of functions might feel verbose to people who know the underlying short native commands well. I personally like function names which can give subtle hints of what they are trying to achieve.

## Functions
In the conditional functions I have chosen to return `"true"` or `"false"` since I like this pattern of comparing function outputs to booleans/boolean strings for further decisions. You can totally customize this as per your preferences.

### configureAsAService
A function to configure a service for non-root user.

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

### getTextBetween
A function to get text between two words in a phrase of a file.

```sh
function getTextBetween() {
  local start=$1
  local end=$2
  local phrase=$3
  local file=$4
  echo $(grep "$phrase" "$file" | awk -v FS="$start|$end" '{print $2}')
}
```

### isResourceAvailable
A function to check if a resource, for instance a file/directory is available at the designated path or not.

```sh
function isResourceAvailable() {
  local path=$1;
  if [ $(compgen -G "$path") ]
     then echo "true"
     else echo "false"
  fi
}
```

### promptUser
A function to take input from user.

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

### startService
A function to start a service as non-root user.

```sh
function startService() {
  local service=$1
  systemctl --user start $service
}
```

### stopService
A function to stop a service as non-root user.

```sh
function stopService() {
  local service=$1
  systemctl --user stop $service
}
```

### trimQuotes
A function to trim the leading and trailing quotes of a value.

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

### trimSpaces
A function to trim the leading and trailing spaces of a value.

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

