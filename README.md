# powershell-tips-tricks


Summary of powershell command-line combinations to achieve different actions.


#Grep

###### Basic grep

> dir -r -include "Some Regex to include" -exclude "*.obj, *.bin" | select-string "SomePattern"

###### Sort loglines

Helps to sort multiple loglines in the following format (based on [stackoverflow](http://stackoverflow.com/questions/36976757/powershell-sort-loglines-by-date/36977633#36977633)):

SOMETHING.txt:4838:2016-04-29T14:11:36,279000+0100; NOTICE  ; Unknown HostId ; bla bla bla

> dir -r -include logdirectory* -exclude logdirectory*.zip, *internal* | select-string "SomePattern" Sort-Object { $time = [regex]::Matches($_, "[0-9]{1,2}:[0-9]{1,2}:[0-9]{1,2}");  [datetime]::ParseExact($time.Value, "HH:mm:ss",[cultureinfo]::InvariantCulture)   } 