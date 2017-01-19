#!/bin/bash

green=$(tput setaf 2)
gold=$(tput setaf 3)
magenta=$(tput setaf 5)
cyan=$(tput setaf 6)
red=$(tput setaf 1)
default=$(tput sgr0)

array=( $(git tag -l --sort=-v:refname) )

version=( ${array[0]//./ } )

usage() { echo "Usage: $0 [ -t major|minor|patch ]" 1>&2; exit 1; }

inc_major() {
  #echo "MAJOR UP"
  ((version[0]=version[0] + 1))
  ((version[1]=0))
  ((version[2]=0))
}

inc_minor() {
  #echo "MINOR UP"
  ((version[1]=version[1] + 1))
  ((version[2]=0))
}

inc_patch() {
  #echo "PATCH UP"
  ((version[2]=version[2] + 1))
}

push_it() {
  git push; git push --tags
}

create_tag() {
  git tag -a $1 -m $2
}

while getopts ":t:" o; do
    case "${o}" in
        t)
            segment=${OPTARG}
            ((segment == "major" || segment == "minor" || segment == "patch")) || usage
            ;;
        *)
            usage
            ;;
    esac
done

case "$segment" in

  minor)
      inc_minor
      ;;

  major)
      inc_major
      ;;

  patch)
      inc_patch
      ;;

  *)
      inc_patch
      ;;

esac

echo "${cyan}Enter Tag Message:${default}"
read message

newtag=${version[0]}.${version[1]}.${version[2]}
create_tag ${newtag} ${message}

echo "${default}Tag ${gold}${newtag}${default} created"

read -p "${cyan}Do you want to push to remote: [y/n]" answer
  case "${answer}" in
      [yY])
          push_it
  esac


