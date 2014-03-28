---
layout: post
title: Massive rename of subtitles (Remake σε Python)
created: 1273411135
categories:
- !ruby/string:Sequel::SQL::Blob script
- !ruby/string:Sequel::SQL::Blob python
---
<p>Με αφορμή το script του <strong>forfolias</strong>, για μαζική μετονομασία αρχείων βάσει άλλων αρχείων, αποφάσισα να συνεισφέρω κι εγώ, με ένα παρόμοιο script γραμμένο σε python. <!--break-->To script έχει ως εξής:</p><p>&nbsp;</p><pre> <span style="color: rgb(255, 102, 0);">import</span> os
 
 subs, vids = [], []
 
 <span style="color: rgb(128, 0, 128);">print</span>('<span style="color: rgb(51, 153, 102);">Initializing ...</span> ')
 <span style="color: rgb(255, 102, 0);">try</span><span style="color: rgb(0, 0, 128);">:</span>
     <span style="color: rgb(255, 102, 0);">for</span> files <span style="color: rgb(255, 102, 0);">in</span> os.listdir(os.getcwd()):
         <span style="color: rgb(255, 102, 0);">if</span> files.endswith('<span style="color: rgb(51, 153, 102);">.avi</span>'):
             vids.append(files[:-<span style="color: rgb(128, 0, 128);">len</span>('<span style="color: rgb(51, 153, 102);">.avi</span>')])
         <span style="color: rgb(255, 102, 0);">elif</span> files.endswith('<span style="color: rgb(51, 153, 102);">.srt</span>'):
             subs.append(files[:-<span style="color: rgb(128, 0, 128);">len</span>('<span style="color: rgb(51, 153, 102);">.srt</span>')])
     <span style="color: rgb(255, 102, 0);">for</span> vid, sub <span style="color: rgb(255, 102, 0);">in</span> <span style="color: rgb(128, 0, 128);">zip</span>(<span style="color: rgb(128, 0, 128);">sorted</span>(vids), <span style="color: rgb(128, 0, 128);">sorted</span>(subs)):
         <span style="color: rgb(255, 102, 0);">if</span> vid != sub:
             old_name = (sub + '<span style="color: rgb(51, 153, 102);">.srt</span>')
             new_name = (vid + '<span style="color: rgb(51, 153, 102);">.srt</span>')
             os.rename(old_name, new_name)
             <span style="color: rgb(128, 0, 128);">print</span>('<span style="color: rgb(51, 153, 102);">File</span>', old_name, '<span style="color: rgb(51, 153, 102);">has been renamed to</span>', new_name)
 <span style="color: rgb(255, 102, 0);">except</span> <span style="color: rgb(128, 0, 128);">Exception</span> <span style="color: rgb(255, 102, 0);">as</span> e:
     <span style="color: rgb(128, 0, 128);">print</span>('<span style="color: rgb(51, 153, 102);">Error encountered:</span>', e)
 <span style="color: rgb(128, 0, 128);">print</span>('<span style="color: rgb(51, 153, 102);">Terminated</span>')
 </pre><p>&nbsp;</p><p>Επεξήγηση:</p><p><!--break--></p><p>Αρχικά, κι εδώ, δημιουργούνται δυο (άδειες) λίστες. Σε ένα <span style="color: rgb(255, 102, 0);">try</span> εμφωλευμένες βρίσκονται οι εντολές, που σε περίπτωση οποιουδήποτε λάθους (υπερβολικό το <span style="color: rgb(128, 0, 128);">Exception</span>, αλλά η χρήση του <span style="color: rgb(128, 0, 128);">OSError</span> μου φάνηκε λιτή) ενημερώνουμε τον χρήστη για το τι συνέβη.</p><p>Εφόσον όλα πάνε καλά, διαβάζουμε τα ερχεία του τρέχων φακέλου. Ελέγχουμε εάν το εκάστοτε αρχέιο έχει κατάληξη είτε ".avi", είτε ".srt" και το αποθηκεύουμε στην ανάλογη λίστα, χωρίς όμως την κατάληξη του.</p><p>Τέλος, αφού διαβάσουμε όλα τα αρχεία και τελειώσουμε με τις λίστες, δημιουργούμε ενα dictionary (κατά κάποιο τρόπο) από τον συνδυασμό των ταξινομημένων λιστών και αναζητούμε τιμές εκεί. Αν ίδιοι index δείκτες διαφέρουν σε τιμές τότε κάνουμε μετονομασία, ενημερώντας πάντα τον χρήστη για την διαδικασία σε περίπτωση λάθους για αργότερη χειροκίνητη διόρθωση.</p><p>ΥΓ: Κατά την υλοποίηση του script θεωρήσαμε δεδομένο ότι κάθε αρχέιο είναι avi και srt. Επίσης θεωρήσαμε δεδομένο ότι τα αρχεία video είναι ισάριθμα με τους υπότιτλους. Και τέλος θεωρήσαμε δεδομένο ότι και τα video αλλά και οι υπότιτλοι δεν έχουν εντελώς τυχαίες ονομασίες, διότι η πιθανότητα για λάθος μετονομασία είναι υπερβολικά μεγάλη.</p>
