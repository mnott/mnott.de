---

title: Capture new Hook Note in Obsidian
menu_order: 1
post_status: publish
post_excerpt: Using Obsidian and Hook together so as to capture a Hook Note in Obsidian.
taxonomy:
    category:
        - Computer
        - Obsidian
        - Hook
    post_tag:
        - Computer
        - Obsidian
        - Hook
---

I've been using Obsidian for a while, and also [Hookmark](https://hookproductivity.com/). Hookmark, previously just "Hook", gives you the ability to link about everything to everything else on your computer. What I wanted to do was to use Hook's note taking ability to post right into Obsidian.

Basically, for this to work you don't need to do anything - it is already built into Hook. But I wanted to add something on top:

1. Not post into Obsidian's Root folder
2. Add some content (essentially, a timestamp), to each note content.

This is still very raw, and I feel I just haven't understood how Hook would be supposed to use the template functionality it has built in; I wasn't able to make that work. So instead, I copied the "New Item" script for Obsidian, and modified it a bit (see below).

Note the `obsidianFolder` variable at the top. Interestingly, if that folder does not exist, Obsidian will create it.

Also, towards the bottom, look for `defContent`. That's my default content that I want to append to each note. I encoded it simply using [urlencoder.org](https://urlencoder.org).


```python
use framework "Foundation"
use scripting additions

-- Set your Obsidian Folder here:
set obsidianFolder to "Z - Zettelkasten/"

property NSString : a reference to current application's NSString
property NSMutableCharacterSet : a reference to current application's NSMutableCharacterSet

set sysinfo to system info
set osver to system version of sysinfo

considering numeric strings
	set isBigSur to osver ≥ "10.16"
end considering

if not isBigSur then
	display dialog "Hook to New > Obsidian requires macOS 11 or more recent. You have macOS " & osver & "."
	return
end if

set fileType to ".md"

set prefUrl to ""
try
	set prefUrl to (do shell script "defaults read com.cogsciapps.hook integration.obsidian.URL.scheme")
on error errMsg
end try

if prefUrl is not "" and prefUrl is not "obsidian-default" and prefUrl is not "hook-file" and prefUrl is not "obsidian-advanced-URI" then
	-- An invalid value for com.cogsciapps.hook integration.obsidian.URL.scheme  has been set. There, we present the following options and set the default here.
	set thePrefChoices to {"obsidian-default (obsidian://)", "obsidian-advanced-URI (obsidian://advanced-uri)", "hook-file (hook://file/)"}
	set thePrefChoice to choose from list thePrefChoices with prompt "Please select one of the following URL schemes with which to interact with Obsidian:" default items {"obsidian-default (obsidian://)"}
	if thePrefChoice is not false then
		set x to thePrefChoice as text
		set AppleScript's text item delimiters to {" "}
		set prefUrl to text item 1 of x
		do shell script "defaults write com.cogsciapps.hook integration.obsidian.URL.scheme  " & prefUrl
	else
		return
	end if
end if



set callbackURL to "hook://x-callback-url/link-to-new"
set encodedSrc to "$encoded_link"
set callbackURLError to "hook://x-callback-url/error"

set encodedTitle to "$encoded_title"
set encodedLink to "$user_link"

set theString to NSString's stringWithString:encodedLink
set charset to NSMutableCharacterSet's URLQueryAllowedCharacterSet's mutableCopy
charset's removeCharactersInString:"&=?"
set encodedLink to theString's stringByAddingPercentEncodingWithAllowedCharacters:charset

if encodedTitle ends with fileType then
	set fileType to ""
end if

set theString to NSString's stringWithString:encodedTitle

--remove / \ :  because Obsidian would not create a file if the file name contains those characters
set theString to theString's stringByReplacingOccurrencesOfString:"/" withString:""
set theString to theString's stringByReplacingOccurrencesOfString:"%5C" withString:""
set theString to theString's stringByReplacingOccurrencesOfString:":" withString:""

--remove | ^ because they will cause file existence validation problem
set theString to theString's stringByReplacingOccurrencesOfString:"%5E" withString:""
set theString to theString's stringByReplacingOccurrencesOfString:"%7C" withString:""

set encodedTitle to theString as string

-- Set your note content here
set defContent to "%0A%0A---%0A%3Cmark%20style%3D%22margin-top%3A%20100%3B%20background-color%3A%20%233B3836%3B%20color%3A%20%23494942%22%3ECreated%3A%20%60%24%3Ddv.span%28dv.current%28%29.file.ctime%29%60%3C%2Fmark%3E"


if prefUrl is "obsidian-advanced-URI" then
	set urlKey to "advanceduri"
	-- An invalid value for com.cogsciapps.hook integration.obsidian.URL.scheme  has been set. There, we present the following options and set the default here.
	set encodedTitle to theString's stringByAddingPercentEncodingWithAllowedCharacters:charset
	set callbackURL to callbackURL & "?src=" & encodedSrc & "&urlKey=advanceduri&plusencoded=yes"
	set theString to NSString's stringWithString:callbackURL
	set callbackURL to theString's stringByAddingPercentEncodingWithAllowedCharacters:charset
	set myURL to "obsidian://advanced-uri?filename=" & obsidianFolder & encodedTitle & fileType & "&data=[" & encodedTitle & "](" & encodedLink & ")"&defContent&"&mode=new&x-success=" & callbackURL & "&x-error=" & callbackURLError
	set myScript to "open " & quoted form of myURL
	do shell script myScript
	return "hook://link-to-new"
end if

if prefUrl is "" or prefUrl is "obsidian-default" then
	set urlKey to ""
else
	set urlKey to "%26urlKey%3Dfile"
end if

set callbackURL to callbackURL & "%3Fsrc%3D" & encodedSrc & "%26titleKey%3Dname" & urlKey
set myURL to "obsidian://new?name=" & obsidianFolder & encodedTitle & fileType & "&content=[" & encodedTitle & "](" & encodedLink & ")"&defContent&"&x-success=" & callbackURL & "&x-error=" & callbackURLError
set myScript to "open " & quoted form of myURL

do shell script myScript

return "hook://link-to-new"
```


---
Related: [[Computer/MacOS/- -|MacOS]]
