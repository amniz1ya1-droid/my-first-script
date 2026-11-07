## Я написал скрипт для машины [The Server From Hell](https://tryhackme.com/room/theserverfromhell) который использовал в своём [writeup](https://github.com/amniz1ya1-droid/tryhackme-writeups/blob/main/medium-rooms/linux/the-server-from-hell/README.md). А вот и он:

```bash
#!/bin/bash

if [ -z "$1" ]; then
    echo -e "Error! \n\n Valid Use: $0 <IP> [start_port] [end_port] <what_port_search> \n\n if [start_port] and [end_port] nothing, scan all 65535 ports. If [what_port_search] nothing, searching every port \n\n Example: <path/to/tool> 10.114.51.234 100 500 http"

    exit 1
fi

TARGET_IP=$1

seq1=${2:-1}

seq2=${3:-65535}

whatsearch=$4

export TARGET_IP
export whatsearch

echo "Start scan $TARGET_IP on ports $seq1-$seq2, search port $whatsearch..."

echo " "

seq $seq1 $seq2 | xargs -P 50 -I {} sh -c '
    OUTPUT=$(nc -v -n -w 1 '$TARGET_IP' {} < /dev/null 2>&1 | grep -i "$whatsearch")
    if [ -n "$OUTPUT" ]; then
        echo "[+] Port {}: $OUTPUT"
    fi
'

echo "\n Script end work!"
```
