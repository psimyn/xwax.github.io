---
layout: default
title: Default xwax Import Script
---
# Default xwax Import Script


## xwax ? - Current

<code bash import>#!/bin/sh
#
# Audio import handler for xwax
#
# This script takes an output sample rate and filename as arguments,
# and outputs signed, little-endian, 16-bit, 2 channel audio on
# standard output. Errors to standard error.
#
# You can adjust this script yourself to customise the support for
# different file formats and codecs.
#

FILE="$1"
RATE="$2"

case "$FILE" in

*.cdaudio)
    echo "Calling CD extract..." >&2
    exec cdparanoia -r `cat "$FILE"` -
    ;;

*.mp3)
    echo "Calling MP3 decoder..." >&2
    exec mpg123 -q -s --rate "$RATE" --stereo "$FILE"
    ;;

*)
    echo "Calling fallback decoder..." >&2
    exec ffmpeg -v 0 -i "$FILE" -f s16le -ar "$RATE" -
    ;;

esac
</code>
