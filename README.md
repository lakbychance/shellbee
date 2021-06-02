# shellbee
A collection of shell utility functions written by a noob in shell scripting. These are tested on Ubuntu 20.04. 

## Purpose
These were written by me when automating a big ass setup. This was my first time ever doing bash scripting and thus the noob thing. Obviously, I took help from StackOverflow as well. These functions came in handy at many places in the whole workflow. Also, there are many ways to implement what these functions provide. Truth be told, there might be many native commands too which can replace or shorten some of the implementtions but I decided it's better to have them saved on the Internet for my benefit. 

## Functions
In the conditional functions I have chosen to return `"true"` or `"false"` since I like this pattern of comparing function outputs to booleans for further decisions. You can totally customize this as per your preferences.

### isResourceAvailable
A function to check if a resource, for instance a file/directory is available at the designated path or not.

```
function isResourceAvailable() {
  local path=$1;
  if [ $(compgen -G "$path") ]
     then echo "true"
     else echo "false"
  fi
}
```

### trimSpaces
A function to trim the leading and trailing spaces of a value.

```
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

### trimQuotes
A function to trim the leading and trailing quotes of a value.

```
function trimQuotes() {
  local trimmed="$1"

  # Strip leading spaces
  trimmed="${trimmed%\"}"

  # Strip trailing spaces
  trimmed="${trimmed#\"}"

  echo "$trimmed";
}
```
