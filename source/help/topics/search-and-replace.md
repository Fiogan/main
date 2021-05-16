## Search and Replace

Search and replace functions can be used within a single scene through [keyboard shortcut](topics/keyboard-shortcuts.md "keyboard shortcuts") or within an entire project via the <a href="#" rel="cside" title="settings" onclick="CSIDE(parent.cside.selectTab.bind(null, 'search'));">Search and Replace</a> tab. Search and replace supports regex, case matching, and pattern finding.


### Search and Replace within a Scene

Using any search–replace hot key command brings up a pop-up search box for searching and replacing text within the current scene file. The search box contains fields for search and replace; it also provides controls for navigating through current matches, enabling or disabling regex searching (see below){link here?}, and restricting a search to selected text. Search results are updated in real time.


### Search and Replace within a Project

To find and replace within an entire project, select the <a href="#" rel="cside" title="settings" onclick="CSIDE(parent.cside.selectTab.bind(null, 'search'));">Search and Replace</a> tab. Results are listed by scene and displayed 'in context' with the surrounding text. Typing in the Replace field brings up previews of any changes, also displayed in context.

By default, 'OnType' (results show as you type) and case matching are enabled. These can be disabled via toggles to the right of the find–replace fields.

For performance reasons, Monaco limits the number of results returned for any search to 999. If the number of finds exceeds this amount, CSIDE will display a notification next to the search results. After a certain number of finds, additional results will be collapsed until selected to improve performance.


#### Scope

Each project's search results are separately preserved. When swapping to a different project, results in the Search and Replace tab will change or disappear to reflect the search parameters {is this the right word? I didn't want to use 'settings'} in the currently selected project. The search function only scans {interacts with? what's the correct word to say what it's doing?} scene files that are currently open.


#### "Stale" Results

The search-and-replace tab displays most scene edits in real time. However, some changes or circumstances can lead to "stale" search results (results that may no longer accurately represent your scene files).

A pop-up notification appears if the search results are no longer current. To clear the error, re-submit your search query.

Conditions that may cause stale search results include:
	• Renaming a file
	• Copying or moving a file to another project


### Advanced: Regexes and Replace Patterns

Search and replace supports regex searches and replace patterns, allowing for more powerful queries. To enable regex, press the relevant toggle button {can we link to this or have an image?} to the right of the search fields.

CSIDE's regex and replace-pattern functionality is based on the Monaco Editor (Visual Studio Code); see [vscode-regexes] for additional information.


#### Regex (Regular Expressions)

A regular expression (regex) is a series of characters used to search for text that matches a given pattern. For instance, regex can be used to search all lines starting with the same word and ending with a variable number of any value. {can we do a 'for more on regexs' and provide some kind of informational link here?}


#### Replace Patterns

The contents of replace patterns are more restricted than regular expressions. {why? what is good about this versus regular search or regex? I know we're not reinventing the wheel here, but just for helps docs readers who are poor writers who  have never heard of all this but want to know at least vaguely what it is so they know whether they need it and want to learn more}

Some common examples of replace pattern substitutions:

	`\n` will be replaced with a line break
	`\t` will be replaced with a tab character
	`\\` will be replaced with a single backlash
	`$$` will be replaced with a single dollar ($)
	`$n` (where n is an integer < 100) will be replaced with the nth match group (as specified in the search regex) {I have no idea what this means, but maybe that's fine. or we could put a (see example below for implementation) maybe? not sure here}
	`$&` and `$0` will be replaced with the entirety of the matched string

This is not an exhaustive list but covers what many authors may find useful. For the brave or curious, visit [vscode-replace-patterns] for further implementation details.


### Advanced Example

The following example demonstrates text manipulation using a regex search and corresponding replace pattern.

The example ChoiceScript code is a series of variables meant to map higher to lowercase letters; a single search query converts them to do the opposite.

**Example ChoiceScript**

<pre>
*temp lowercase_B "b"
*temp lowercase_C "c"
*temp lowercase_D "d"
*temp lowercase_E "e"
*temp lowercase_F "f"
</pre>

**Search Regex**

Example regex search term:

<pre>
temp lowercase_([A-Z]) "([a-z])"
</pre>

This search will match text starting with 'temp lowercase_' and followed by a single character A–Z (case sensitive), then a space, then a single character (a–z) wrapped with in double quotes.

**Replace Pattern**

The replace pattern will replace matching text with the string 'temp uppercase_$2 "$1"' {can you show this segment as a code block or something instead of using ' to make the demarcations clearer? using ' so close to " can muddy the waters a bit, especially for the eyes of new coders}. Only $1 and $2, which are special replace-pattern denotations, will be replaced.

	The parenthesis represent "match groups" (a way to capture certain information about the search result) used in the replace pattern. For example:

	results = [
   		{ line: 7, character: 4},
   		{ line: 200, character: 2}
	]
	results = FIND "(MY|YOUR) SEARCH"
	results = [
   		{ line: 7, character: 4, matchgroups: [ "MY" ] },
   		{ line: 200, character: 2, matchgroups: [ "YOUR" ] }
	]

In this example, $1 matches our first group match (remember the parentheses): the capital letter following 'lowercase_'. $2 matches the second set of parentheses and will evaluate to the quoted value.

<pre>
temp uppercase_$2 "$1"
</pre>

**Resulting ChoiceScript**

The end result is the following (note the switched lower- and uppercase values):
<pre>
*temp uppercase_b "B"
*temp uppercase_c "C"
*temp uppercase_d "D"
*temp uppercase_e "E"
*temp uppercase_f "F"
</pre>

[vscode-replace-patterns]: https://github.com/microsoft/vscode/blob/a699ffaee62010c4634d301da2bbdb7646b8d1da/src/vs/editor/contrib/find/replacePattern.ts#L230 "234"
[vscode-regexes]: https://docs.microsoft.com/en-us/visualstudio/ide/using-regular-expressions-in-visual-studio?view=vs-2019 "123"