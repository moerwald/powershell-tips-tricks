# powershell-tips-tricks


Summary of powershell command-line combinations to achieve different actions.


#Grep

###### Basic grep

> dir -r -include "Some Regex to include" -exclude "*.obj, *.bin" | select-string "SomePattern"

###### Sort loglines

Helps to sort multiple loglines in the following format (based on [stackoverflow](http://stackoverflow.com/questions/36976757/powershell-sort-loglines-by-date/36977633#36977633)):

SOMETHING.txt:4838:2016-04-29T14:11:36,279000+0100; NOTICE  ; Unknown HostId ; bla bla bla

> dir -r -include logdirectory* -exclude logdirectory*.zip, *internal* | select-string "SomePattern" | Sort-Object { $time = [regex]::Matches($_, "[0-9]{4}-[0-9]{2}-[0-9]{2}T[0-9]{1,2}:[0-9]{1,2}:[0-9]{1,2},[0-9]{6}"); [datetime]
::ParseExact($time, 'yyyy-MM-ddTHH:mm:ss,ffffffK', [cultureinfo]::InvariantCulture)   } 


###### Get lines between specific dates

> $text | where { $time = [regex]::Matches($_, "[0-9]{4}-[0-9]{2}-[0-9]{2}T[0-9]{1,2}:[0-9]{1,2}:[0-9]{1,2}"); $timeParsed = [datetime]::Parse($time); [datetime]::Parse("2016-05-03T10:00:00") -lt $tim
eParsed -and $timeParsed -lt [datetime]::Parse("2016-05-03T10:05:00") }

# Remote commands

###### Create a new remote session towards server

> $sess = New-PSSession -ComputerName computername.domain -Credential username

From now on you can use the sess object to perform remote commands:

>Invoke-Command -Session $sess -ScriptBlock { Get-Service }

At the end you can remove the session:

>Remove-PSSession -Session $sess
