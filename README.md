# .bashrc examples


## ll as cute ls [bash]

```bash
alias ll="ls -lhA"
```

**Usage**

  `ll`


## deploy node.js project [node.js] [pm2]

```bash
dep () {
  info="$@";
  currentBranch=`git rev-parse --abbrev-ref HEAD`;
  echo "$info : $currentBranch -> master -> deploy -> $currentBranch";
  if [ "$currentBranch" == "master" ]; then
    echo "[PRESS ENTER]";
    read
  fi;
  if [[ ! -z $info ]]; then
    cp CHANGELOG.md CHANGELOG.md- && sed "8i\- $info" CHANGELOG.md- > CHANGELOG.md && rm CHANGELOG.md-
  fi;
  git commit -a -m "[$currentBranch] $info" && git push origin $currentBranch;
  git checkout master && git merge $currentBranch && git push origin master;
  git checkout $currentBranch;
  pm2 deploy test
};
```

**Usage**

  Deploy without changelog information

  `dep`

  Deploy with changelog information

  `dep CHANGELOG INFORMATION`

**Some conditions**

- project hasn't to be in master branch
- changelog information adds to 8'th line of CHANGELOG.md file
- project merges to master branch
- this is example to deploy current branch to test environment (read more [how to deploy with pm2](https://keymetrics.io/2014/06/25/ecosystem-json-deploy-and-iterate-faster/))


## create a directory and cd into it [bash]

```bash
mcd () {
    mkdir -p $1
    cd $1
}
```

**Usage**

`mcd new_dir`


## useful ps [bash]

```bash
alias ps="ps auxf"
```

**Usage**

  `ps`


## process search [bash]

```bash
alias psg="ps aux | grep -v grep | grep -i -e VSZ -e"
```

**Usage**

  `psg chrome`


## human-readable df with filesystem type and total [bash]

```bash
alias df="df -Tha --total"
```

**Usage**

  `df`


## sorted du [bash]

```bash
alias du="du -ach | sort -h"
```

**Usage**

  `du`


## extract any archive [bash]

```bash
function e {
 if [ -z "$1" ]; then
    # display usage if no parameters given
    echo "Usage: extract <path/file_name>.<zip|rar|bz2|gz|tar|tbz2|tgz|Z|7z|xz|ex|tar.bz2|tar.gz|tar.xz>"
 else
    if [ -f $1 ] ; then
        # NAME=${1%.*}
        # mkdir $NAME && cd $NAME
        case $1 in
          *.tar.bz2)   tar xvjf ../$1    ;;
          *.tar.gz)    tar xvzf ../$1    ;;
          *.tar.xz)    tar xvJf ../$1    ;;
          *.lzma)      unlzma ../$1      ;;
          *.bz2)       bunzip2 ../$1     ;;
          *.rar)       unrar x -ad ../$1 ;;
          *.gz)        gunzip ../$1      ;;
          *.tar)       tar xvf ../$1     ;;
          *.tbz2)      tar xvjf ../$1    ;;
          *.tgz)       tar xvzf ../$1    ;;
          *.zip)       unzip ../$1       ;;
          *.Z)         uncompress ../$1  ;;
          *.7z)        7z x ../$1        ;;
          *.xz)        unxz ../$1        ;;
          *.exe)       cabextract ../$1  ;;
          *)           echo "extract: '$1' - unknown archive method" ;;
        esac
    else
        echo "$1 - file does not exist"
    fi
fi
}
```

**Usage**

  `e <filename>`


## perl checks every .pl, .pm and .t files recursively [perl]

```bash
alias perlc="find -type f -name '*.t' -exec perl -c {} \; -or -name '*.p[lm]' -exec perl -c {} \;"
```

**Usage**

  `perlc`


## All in common

```bash
alias ll="ls -lhA"
alias ps="ps auxf"
alias df="df -Tha --total"
alias psg="ps aux | grep -v grep | grep -i -e VSZ -e"
alias du="du -ach | sort -h"

alias perlc="find -type f -name '*.t' -exec perl -c {} \; -or -name '*.p[lm]' -exec perl -c {} \;"

mcd () {
    mkdir -p $1
    cd $1
}

dep () {
  info="$@";
  currentBranch=`git rev-parse --abbrev-ref HEAD`;
  echo "$info : $currentBranch -> master -> deploy -> $currentBranch";
  if [ "$currentBranch" == "master" ]; then
    echo "[PRESS ENTER]";
    read
  fi;
  if [[ ! -z $info ]]; then
    cp CHANGELOG.md CHANGELOG.md- && sed "8i\- $info" CHANGELOG.md- > CHANGELOG.md && rm CHANGELOG.md-
  fi;
  git commit -a -m "[$currentBranch] $info" && git push origin $currentBranch;
  git checkout master && git merge $currentBranch && git push origin master;
  git checkout $currentBranch;
  pm2 deploy test
};

function e {
 if [ -z "$1" ]; then
    # display usage if no parameters given
    echo "Usage: extract <path/file_name>.<zip|rar|bz2|gz|tar|tbz2|tgz|Z|7z|xz|ex|tar.bz2|tar.gz|tar.xz>"
 else
    if [ -f $1 ] ; then
        # NAME=${1%.*}
        # mkdir $NAME && cd $NAME
        case $1 in
          *.tar.bz2)   tar xvjf ../$1    ;;
          *.tar.gz)    tar xvzf ../$1    ;;
          *.tar.xz)    tar xvJf ../$1    ;;
          *.lzma)      unlzma ../$1      ;;
          *.bz2)       bunzip2 ../$1     ;;
          *.rar)       unrar x -ad ../$1 ;;
          *.gz)        gunzip ../$1      ;;
          *.tar)       tar xvf ../$1     ;;
          *.tbz2)      tar xvjf ../$1    ;;
          *.tgz)       tar xvzf ../$1    ;;
          *.zip)       unzip ../$1       ;;
          *.Z)         uncompress ../$1  ;;
          *.7z)        7z x ../$1        ;;
          *.xz)        unxz ../$1        ;;
          *.exe)       cabextract ../$1  ;;
          *)           echo "extract: '$1' - unknown archive method" ;;
        esac
    else
        echo "$1 - file does not exist"
    fi
fi
}
```


## Created and copy-pasted by

Dimitry, 2@ivanoff.org.ua # curl -A cv ivanoff.org.ua
