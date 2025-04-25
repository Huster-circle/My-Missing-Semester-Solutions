- ssh myserver journalctl
- ssh myserver journalctl | grep sshd
- ssh myserver 'journalctl | grep sshd | grep "Disconnected from"' | less
- ssh myserver 'journalctl | grep sshd | grep "Disconnected from"' > ssh.log
- less ssh.log
- ssh myserver journalctl | grep sshd | grep "Disconnected from" | sed 's/.*Disconnected from //'
- /.*Disconnected from /
- perl -pe 's/.*?Disconnected from //'
- | sed -E 's/.*Disconnected from (invalid |authenticating )?user .* [^ ]+ port [0-9]+( \ [preauth\])?$//'
- | sed -E 's/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]+ port [0-9]+( \ [preauth\])?$/\2/'
- ssh myserver journalctl | grep sshd | grep "Disconnected from" | sed -E 's/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]+ port [0-9]+( \ [preauth\])?$/\2/'
- ssh myserver journalctl
 | grep sshd
 | grep "Disconnected from"
 | sed -E 's/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]+ port [0-9]+( \ [preauth\])?$/\2/'
 | sort | uniq -c
- ssh myserver journalctl
 | grep sshd
 | grep "Disconnected from"
 | sed -E 's/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]+ port [0-9]+( \ [preauth\])?$/\2/'
 | sort | uniq -c
 | sort -nk1,1 | tail -n10
- ssh myserver journalctl
 | grep sshd
 | grep "Disconnected from"
 | sed -E 's/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]+ port [0-9]+( \ [preauth\])?$/\2/'
 | sort | uniq -c
 | sort -nk1,1 | tail -n10
 | awk '{print $2}' | paste -sd
- | awk '$1 == 1 && $2 ~ /^c[^ ]*e$/ { print $2 }' | wc -l
-  | paste -sd+ | bc -l
- echo "2*($(data | paste -sd+))" | bc -l
- ssh myserver journalctl
 | grep sshd
 | grep "Disconnected from"
 | sed -E 's/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]+ port [0-9]+( \ [preauth\])?$/\2/'
 | sort | uniq -c
 | awk '{print $1}' | R --slave -e 'x <- scan(file="stdin", quiet=TRUE); summary(x)'
- ssh myserver journalctl
 | grep sshd
 | grep "Disconnected from"
 | sed -E 's/.*Disconnected from (invalid |authenticating )?user (.*) [^ ]+ port [0-9]+( \ [preauth\])?$/\2/'
 | sort | uniq -c
 | sort -nk1,1 | tail -n10
 | gnuplot -p -e 'set boxwidth 0.5; plot "-" using 1:xtic(2) with boxes'
- rustup toolchain list | grep nightly | grep -vE "nightly-x86" | sed 's/-x86.*//' | xargs rustup toolchain uninstall
- ffmpeg -loglevel panic -i /dev/video0 -frames 1 -f image2 -
 | convert - -colorspace gray -
 | gzip
 | ssh mymachine 'gzip -d | tee copy.jpg | env DISPLAY=:0 feh -'


