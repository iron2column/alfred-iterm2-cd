# alfred-iterm2-cd

This is a workflow script of Alfred based on Applescript.

----

## 1. Introduce
You can call Alfred, and then enter `cd "your target file or directory"`. After entering, it will create a new iterm2 window and switch to the target file or directory.


## 2. Script
```
set thePath to "{query}"

set result to do shell script "file -b " & thePath

if result is not equal to "directory" then
	set q to characters 1 thru -((offset of "/" in (reverse of items of thePath as string)) + 1) of thePath as string
else
	set q to thePath
end if

set q to "cd " &q &" && clear"

if application "iTerm" is running then
	run script "
			on run {q}
				tell application \"iTerm\"
					tell the first window
						create window with default profile
						tell current session to write text q 
					end tell
				end tell
			end run
		" with parameters {q}
else
	run script "
			on run {q}
				tell application \"iTerm\"
					activate
					delay 0.5
					tell application \"iTerm\"
					tell the first window
						tell current session to write text q 
					end tell
					end tell
				end tell
			end run
		" with parameters {q}
end if
```
