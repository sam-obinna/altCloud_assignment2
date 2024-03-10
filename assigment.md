## This is a markdown file containing the commands to the assignment

#!/bin/bash
show_files=false
entries=5
optstrings="dn:"
help() {
  echo "Usage: script $0 [-d -n N] directory"
  echo "where N is an integer"
}
while getopts "${optstrings}" opt; do
        case $opt in
                d)
                  show_files=true
                  ;;
                n)
                  entries=$OPTARG
                  ;;
                \?)
                  help
                  ;;
  esac
done



shift "$((OPTIND - 1))"
for dir in "$@"; do
	echo "****************************"
        if [ ! -d "$dir" ]; then
                echo "$dir is not a valid directory"
                continue
        fi
        if [ "$show_files" = true ]; then
                top_entry=$(sudo find "$dir" -type f -exec du -Sh {} + | sort -rh | head -n "$entries")
        else
                top_entry=$(du -h "$dir" 2>/dev/null | sort -rh | head -n "$entries")
        fi
        echo "$top_entry"
        echo "******************************"
done