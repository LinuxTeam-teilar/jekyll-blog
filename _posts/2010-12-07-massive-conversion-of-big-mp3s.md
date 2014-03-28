---
layout: post
title: Massive conversion of (big) mp3s
created: 1291754792
categories:
- !ruby/string:Sequel::SQL::Blob bash
- !ruby/string:Sequel::SQL::Blob script
---
<p>Πριν λίγες μέρες ξέμεινα από χώρο στο mp3 player μου. Εφόσον δεν ήθελα να διαγράψω αρχεία η μόνη λύση ήταν η συμπίεση των υπάρχοντων αρχείων. Κατέληξα στο παρακάτω script:&nbsp;</p><blockquote>IFS=$'\n'<br>for i in `find . -name *\.mp3`
          do
              k=`file "$i" | sed 's/, /\n/g' | grep kbps | sed 's/ kbps//'`
              if [ "$k" != "" ]
              then
                  if [ "$k" -ge "128" ]
                  then
                      echo "$i" = "$k"
                      mv "$i" input.mp3
                      lame -b 96 input.mp3 "$i"
                      rm input.mp3
                  fι
              fi
          done
         </blockquote><p>Αρχικά ψάχνει στον υπάρχον φάκελο και τους υποφακέλους του για αρχεία mp3, διαβάζει και απομονώνει τα kbps τους και τέλος αν αυτά υπερβαίνουν τα 128 kbps μετατρέπει τα mp3 σε 96kbps.&nbsp;</p><p>Υ.Γ.: special thanks to Digenis για το&nbsp;IFS=$'\n' το οποίο ορίζει το <a href="http://mindspill.net/computing/linux-notes/using-the-bash-ifs-variable-to-make-for-loops-split-with-non-whitespace-characters.html">Internal Field Separator</a> σε κάτι διαφορετικό από το κενό (που είναι η προεπιλογή).</p><p>&nbsp;</p>
