# Regular Expression Tutorial: Matching a URL

All high-level programming languages come with built-in methods that can check if a string contains a certain character or any combination of characters. For example, the following JavaScript statement will return true:
    /^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/
These methods are great; however, you need to know the exact string you are searching for. Is there a way we can do this dynamically? For instance, we would like to search the word "fast" but not "fasting". Let's introduce **regular expression**.

## Summary

A **regular expression** (shortened as **regex**) is a sequance of characters that specifies a match pattern in text. Take the following example of a regular expression, which we'll call "Matching a Url":
    /^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/
This sereis of characters looks complicated and unreadable; but it's actually a search pattern for basic url validation. In short, here's how it breakdowns (we will explain each one later):
- The string (group 1) must begin with "http(s)://"
- The sub-sequent (group 2) string can contain one or more of any digit (0-9), any character (a-z), any character ".", any character "-", followed by "."
- The sub-sequent string (group 3) can contain any character (a-z), any character ".",the string must be between 2-6 characters long
- The sub-sequent string (group 4) can contain 0 or more of any character "/", any word character, space character, any character ".", any character "-"
- the string can contain 0 or more of group 3 or group 4 sub-string.
- the string either ends with or without character "/"

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [OR Operator](#or-operator)
- [Character Classes](#character-classes)
- [Flags](#flags)
- [Grouping and Capturing](#grouping-and-capturing)
- [Bracket Expressions](#bracket-expressions)
- [Greedy and Lazy Match](#greedy-and-lazy-match)
- [Boundaries](#boundaries)
- [Back-references](#back-references)
- [Look-ahead and Look-behind](#look-ahead-and-look-behind)

## Regex Components
A regex is considered a literal, so the pattern must be wrapped in slash characters (/). Refer to [MDN Web Docs on the RegExp object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp)

### Anchors
Anchors do not match any character at all. Instead, they match a position before, after, or between characters. They can be used to “anchor” the regex match at a certain position. The caret ^ matches the position before the first character in the string. Applying ^1 to 1ab matches 1. ^b does not match 1ab at all, because the b cannot be matched right after the start of the string, matched by ^. Similarly, $ matches right after the last character in the string. b$ matches b in 1ab, while a$ does not match at all.

### Quantifiers
**Quantifiers** set the limits of the string that your regex matches (or an individual section of the string). They frequently include the minimum and maximum number of characters that your regex is looking for.

Quantifiers are inherently greedy, meaning they match as many occurrences of particular patterns as possible. They include the following:

    *—Matches the pattern zero or more times

    +—Matches the pattern one or more times

    ?—Matches the pattern zero or one time

    {}—Curly brackets can provide three different ways to set limits for a match:

    { n }—Matches the pattern exactly n number of times

    { n, }—Matches the pattern at least n number of times

    { n, x }—Matches the pattern from a minimum of n number of times to a maximum of x number of times

Each of these quantifiers can be made lazy by adding the ? symbol after it, meaning it will match as few occurrences as possible.

In our example:
    /^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/
The string contains quantifier {2, 6}, which means it is looking for the preceding sting patterns minimum 2 times, and maximum 6 times.  

### OR Operator
Using the **OR operator (|)**, the expression [1ab] could be written as (1|a|b). Now, all of the following strings: "1ab", "ab1", "ba1" would match; as well as "a" or "1b".

### Character Classes
A **character class** in a regex defines a set of characters, any one of which can occur in an input string to fulfill a match. We've actually already discussed some character classes. The bracket expressions outlined previously, including positive and negative character groups, are considered character classes.

Here are some of the other common character classes:

    .—Matches any character except the newline character (\n)

    \d—Matches any Arabic numeral digit. This class is equivalent to the bracket expression [0-9].

    \w—Matches any alphanumeric character from the basic Latin alphabet, including the underscore (_). This class is equivalent to the bracket expression [A-Za-z0-9_].

    \s—Matches a single whitespace character, including tabs and line breaks

Each of the last three character classes can be changed to perform an inverse match by capitalizing the letter character. For example, \D matches a non-digit character.

### Flags
**Flags** are placed at the end of a regex, after the second slash, and they define additional functionality or limits for the regex. There are six optional flags that can be used, either separately or together and in any order, but these are the three you're most likely to encounter:

    g—Global search: the regex should be tested against all possible matches in a string.

    i—Case-insensitive search: case should be ignored while attempting a match in a string

    m—Multi-line search: a multi-line input string should be treated as multiple lines

### Grouping and Capturing
**Grouping** constructs are the primary way you group a section of a regex is by using parentheses (()). Each section within parentheses is known as a subexpression.

The following example contains two grouping constructs or subexpressions:

    (abc):(xyz)

The first subexpression is looking for a part of the string that matches the string "abc" exactly. Similarly, the second subexpression is looking for "xyz". In between the subexpressions, we have a colon (:). Thus, the string "abc:xyz" would match, but the string "acb:xyz" would not. Unlike bracket expressions, subexpressions look for an exact match unless they're told to do otherwise.

Grouping constructs have two primary categories: **capturing** and **non-capturing**. capturing groups capture the matched character sequences for possible re-use (including a numbered **back-reference**) while non-capturing groups do not. A grouping construct can be made non-capturing by adding the characters ?: at the beginning of an expression inside the parentheses.

### Bracket Expressions
Anything inside a set of square brackets ([]) represents a range of characters that we want to match. These patterns are known as **bracket expressions**, but they are also known as a positive character group, because they outline the characters we want to include. We can write these expressions to include all of the characters we want to match. For example, [abc] will look for a string that includes a or b or c, regardless of the length of the string. So all of the following examples would match: "aaa", "bin" "court", "abracadabra", and "bca".

You'll more commonly see a hyphen (-) used between alphanumeric characters (letters and numbers only) to represent a range of those possible characters. This means that [b-f] and [bcdef] will look for the exact same thing.

In our “Matching a Username” regex example, we can break down the bracket expressions as follows:

[a-z]—The string can contain any lowercase letter between a–z. Keep in mind that this looks for lowercase characters only. If we wanted to include uppercase characters, we would need to change the expression to [a-zA-Z].

[0-9]—The string can contain any number between 0–9

[_-]—The string can contain an underscore or hyphen. Both the underscore and the hyphen are called special characters. Special characters include any non-alphanumeric characters, such as punctuation or symbols. In this case, we only want a string that includes _ or -. It's important to note that the hyphen here is not the same hyphen that we used in our alphanumeric ranges. In bracket expressions, special characters that we want to include follow alphanumeric character ranges within the brackets.

If we put all of these expressions together so that our pattern is [a-z0-9_-], this will match any string that includes any combination of lowercase letters between a and z, any number between 0 and 9, and the special characters of an underscore or a hyphen. Keep in mind that these characters can be in any order. It's also important to note that this pattern does not require the string to meet all of these requirements; it can meet any of them.

### Greedy and Lazy Match
**Greedy: As Many As Possible (longest match)**
By default, a quantifier tells the engine to match as many instances of its quantified token or subpattern as possible. This behavior is called greedy.

For instance, take the + quantifier. It allows the engine to match one or more of the token it quantifies: \d+ can therefore match one or more digits. But "one or more" is rather vague: in the string 123, "one or more digits" (starting from the left) could be 1, 12 or 123. Which of these does \d+ match?

Because by default quantifiers are greedy, \d+ matches as many digits as possible, i.e. 123. For the match attempt that starts at a given position, a greedy quantifier gives you the longest match.

**Lazy: As Few As Possible (shortest match)**
In contrast to the standard greedy quantifier, which eats up as many instances of the quantified token as possible, a lazy (sometimes called reluctant) quantifier tells the engine to match as few of the quantified tokens as needed. As you'll see in the table below, a regular quantifier is made lazy by appending a ? question mark to it.

Since the * quantifier allows the engine to match zero or more characters, \w*?E tells the engine to match zero or more word characters, but as few as needed—which might be none at all—then to match an E. In the string 123EEE, starting from the very left, "zero or more word characters then an E" could be 123E, 123EE or 123EEE. Which of these does \w*?E match?

Because the *? quantifier is lazy, \w*? matches as few characters as possible to allow the overall match attempt to succeed, i.e. 123—and the overall match is 123E. For the match attempt that starts at a given position, a lazy quantifier gives you the shortest match.

### Boundaries
The word **boundary** \b matches positions where one side is a word character (usually a letter, digit or underscore) and the other side is not a word character (for instance, it may be the beginning of the string or a space character).

The regex \bcat\b would therefore match cat in a black cat, but it wouldn't match it in catatonic, tomcat or certificate. Removing one of the boundaries, \bcat would match cat in catfish, and cat\b would match cat in tomcat, but not vice-versa. Both, of course, would match cat on its own.

Word boundaries are useful when you want to match a sequence of letters (or digits) on their own, or to ensure that they occur at the beginning or the end of a sequence of characters.

### Back-references
You place a sub-expression in parentheses, you access the capture with \1. This is called **back-reference**.

For instance, the regex \b(\w+)\b\s+\1\b matches repeated words, such as regex regex, because the parentheses in (\w+) capture a word to Group 1 then the back-reference \1 tells the engine to match the characters that were captured by Group 1.

In a regex pattern, every set of capturing parentheses from left to right as you read the pattern gets assigned a number, whether or not the engine uses these parentheses when it evaluates the match.

### Look-ahead and Look-behind
Lookahead and lookbehind, collectively called “lookaround”, are zero-length assertions just like the start and end of line, and start and end of word anchors explained earlier in this tutorial. The difference is that lookaround actually matches characters, but then gives up the match, returning only the result: match or no match. That is why they are called “assertions”. They do not consume characters in the string, but only assert whether a match is possible or not. Lookaround allows you to create regular expressions that are impossible to create without them, or that would get very longwinded without them.

**Lookahead**: when we look for A(?=B), the regular expression engine finds A and then checks if there’s B immediately after it. If it’s not so, then the potential match is skipped, and the search continues.

More complex tests are possible, e.g. A(?=B)(?=C) means:
Find A.
Check if B is immediately after A (skip if isn’t).
Check if C is also immediately after A (skip if isn’t).
If both tests passed, then the A is a match, otherwise continue searching.

**Lookbehind** is similar, but it looks behind. That is, it allows to match a pattern only if there’s something before it.

The syntax is:

Positive lookbehind: (?<=B)A, matches A, but only if there’s B before it.
Negative lookbehind: (?<!B)A, matches A, but only if there’s no B before it.

## Author

An inspired rookie web developer, you can find Jesse's repos [here](https://github.com/JesseCh3n?tab=repositories).
