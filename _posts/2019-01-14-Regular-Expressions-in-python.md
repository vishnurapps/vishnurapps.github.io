---
title:  "Regular Expressions in python"
date:   2019-01-14
layout: single
author_profile: true
comments: true
tags: 
---
.       - Any Character Except New Line
\d      - Digit (0-9)
\D      - Not a Digit (0-9)
\w      - Word Character (a-z, A-Z, 0-9, _)
\W      - Not a Word Character
\s      - Whitespace (space, tab, newline)
\S      - Not Whitespace (space, tab, newline)

\b      - Word Boundary
\B      - Not a Word Boundary
^       - Beginning of a String
$       - End of a String

[]      - Matches Characters in brackets
[^ ]    - Matches Characters NOT in brackets
|       - Either Or
( )     - Group

Quantifiers:
*       - 0 or More
+       - 1 or More
?       - 0 or One
{3}     - Exact Number
{3,4}   - Range of Numbers (Minimum, Maximum)

import re

text_to_search = '''
abcdefghijklmnopqurtuvwxyz
ABCDEFGHIJKLMNOPQRSTUVWXYZ
1234567890
Ha HaHa
MetaCharacters (Need to be escaped):
. ^ $ * + ? { } [ ] \ | ( )
coreyms.com
321-555-4321
123.555.1234
123*555*1234
800-555-1234
900-555-1234
Mr. Schafer
Mr Smith
Ms Davis
Mrs. Robinson
Mr. T

cat
mat
pat
bat
'''

sentence = 'Start a sentence and then bring it to an end'


#print('\tTab')
#print(r'\tTab') #This is raw string. Python dont do anything to raw string. Its printed like that#
#	Tab
#\tTab

# pattern = re.compile(r'\bHa')
# matches = pattern.finditer(text_to_search)
# for match in matches:
# 	print(match.span(), match.group())

#The Ha and the Ha in the first are matched because we searched with \b

# pattern = re.compile(r'\BHa')
# matches = pattern.finditer(text_to_search)
# for match in matches:
# 	print(match.span(), match.group())

# Here it matches the second Ha in HaHa as there is no word boundary before it.

# pattern = re.compile(r'^Start')
# matches = pattern.finditer(sentence)
# for match in matches:
# 	print(match.span(), match.group())
#Here the Start at the begining is found
#If we use ^a insted, we wont get any output as there is not a at the start of the sentence

# pattern = re.compile(r'end$')
# matches = pattern.finditer(sentence)
# for match in matches:
# 	print(match.span(), match.group())
#Here end is matched. If we give and$, this wont work eventhough and is present. It is not at the end.

# pattern = re.compile(r'\d\d\d.\d\d\d.\d\d\d\d') 
# matches = pattern.finditer(text_to_search)
# for match in matches:
# 	print(match.span(), match.group())
#If we use . in between numbers, it will match anything. If we are specific to what all should be matched, we can give that in the square bracket.

# pattern = re.compile(r'\d\d\d[-.]\d\d\d[-.]\d\d\d\d')
# matches = pattern.finditer(text_to_search)
# for match in matches:
# 	print(match.span(), match.group())


#Suppose we want to match 800 and 900 we can do like this
# pattern = re.compile(r'[89]00[-.]\d\d\d[-.]\d\d\d\d')
# matches = pattern.finditer(text_to_search)
# for match in matches:
# 	print(match.span(), match.group())

#[1-5] matches if the first number is between 1 and 5, [a-f] means the letter between a and f. [a-fA-F] this matches both upper and lower
# pattern = re.compile(r'[1-5]')
# matches = pattern.finditer(text_to_search)
# for match in matches:
# 	print(match.span(), match.group())

# re.compile('r[^a-fA-F') -> means everything other than whats is inside
#re.compile(r'[^b]at') -> matches everything except bat
# Instead of re.compile(r'\d\d\d.\d\d\d.\d\d\d\d') , I can use re.compile(r'\d{3}.\d{3}.d\{4}') 

# pattern = re.compile(r'M(r|s|rs)\.?\s[A-Z]\w*')
# matches = pattern.finditer(text_to_search)
# for match in matches:
# 	print(match.span(), match.group())

#Mr
# ((216, 218), 'Mr')
# ((228, 230), 'Mr')
# ((246, 248), 'Mr')
# ((260, 262), 'Mr')

# Mr.
# ((216, 219), 'Mr.')
# ((228, 231), 'Mr ')
# ((246, 249), 'Mrs')
# ((260, 263), 'Mr.')
# Mr\.?
# ((216, 219), 'Mr.')
# ((228, 230), 'Mr')
# ((246, 248), 'Mr')
# ((260, 263), 'Mr.')
# Mr\.?\s[A-Z]\w+
# ((216, 227), 'Mr. Schafer')
# ((228, 236), 'Mr Smith')
# Mr\.?\s[A-Z]\w*
# ((216, 227), 'Mr. Schafer')
# ((228, 236), 'Mr Smith')
# ((260, 265), 'Mr. T')
# M(r|s|rs)\.?\s[A-Z]\w*
# ((216, 227), 'Mr. Schafer')
# ((228, 236), 'Mr Smith')
# ((237, 245), 'Ms Davis')
# ((246, 259), 'Mrs. Robinson')
# ((260, 265), 'Mr. T')

# emails = '''
# CoreyMSchafer@gmail.com
# corey.schafer@university.edu
# corey-321-schafer@my-work.net
# '''

# pattern = re.compile(r'[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+')

# matches = pattern.finditer(emails)

# for match in matches:
#     print(match.span(), match.group())

urls = '''
https://www.google.com
http://coreyms.com
https://youtube.com
https://www.nasa.gov
'''

#pattern = re.compile(r'https?://(www\.)?(\w+)(\.\w+)')
# matches = pattern.finditer(urls)
# for match in matches:
#     print(match.span(), match.group(), match.group(1), match.group(2), match.group(3))

# ((1, 23), 'https://www.google.com', 'www.', 'google', '.com')
# ((24, 42), 'http://coreyms.com', None, 'coreyms', '.com')
# ((43, 62), 'https://youtube.com', None, 'youtube', '.com')
# ((63, 83), 'https://www.nasa.gov', 'www.', 'nasa', '.gov')


# subbed_urls = pattern.sub(r'\2\3', urls)
# print(subbed_urls)
#Here the second and third group is taken and substituted it with group 2 and 3. Groups are things inside ()
# google.com
# coreyms.com
# youtube.com
# nasa.gov

pattern = re.compile(r'start', re.IGNORECASE)  #Here we are want to do ignore case.
matches = pattern.finditer(sentence)
for match in matches:
	print(match.span(), match.group())
