# RegExp

## Resources:
- [RegExp Tester](http://regexpal.com/):  Simple web page to sanity test your match statements. 
- [text2re: A regex builder](http://txt2re.com/).  Helps you build regular expressions for a specific string
- [Regular Expressions101](http://www.regex101.com/): Another testing site for regex's but with the ability to create permalinks.


## Quick Reference:
| code | description |
|--|--|
`.`	|Any character except newline.
`\.`	|A period (and so on for \*, \(, \\, etc.)
`^`	|The start of the string.
`$`	|The end of the string.
`\d,\w,\s`	|A digit, word character [A-Za-z0-9_], or whitespace.
`\D,\W,\S`	|Anything except a digit, word character, or whitespace.
`[abc]`	|Character a, b, or c.
`[a-z]`	|a through z.
`[^abc]`	|Any character except a, b, or c.
`aa|bb`	|Either aa or bb.
`?`	|Zero or one of the preceding element.
`*`	|Zero or more of the preceding element.
`+`	|One or more of the preceding element.
`{n}`	|Exactly n of the preceding element.
`{n,}`	|n or more of the preceding element.
`{m,n}`	|Between m and n of the preceding element.
`??,*?,+?,`<br>`{n}?, etc.`	|Same as above, but as few as possible.
`(expr)`	|Capture expr for use with \1, etc.
`(?:expr)`	|Non-capturing group.
`(?=expr)`	|Followed by expr.
`(?!expr)`	|Not followed by expr.

