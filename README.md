# Soundex Programming Exercise
The goal of this programming exercise is to write code that implements the Soundex algorithm. I pulled the instructions below from Wikipedia and left them unmodified because reading them and deciding on an implementation brings up questions that have similarities to a real-world story that a developer might be asked to implement. You can see solutions to the exercise (and submit PRs with your own solutions!) in the `solution` branch.

# Soundex
Soundex is a phonetic algorithm for indexing names by sound, as pronounced in English. The goal is for homophones to be encoded to the same representation so that they can be matched despite minor differences in spelling. The algorithm mainly encodes consonants; a vowel will not be encoded unless it is the first letter. Soundex is the most widely known of all phonetic algorithms (in part because it is a standard feature of popular database software such as DB2, PostgreSQL, MySQL, SQLite, Ingres, MS SQL Server and Oracle.) Improvements to Soundex are the basis for many modern phonetic algorithms.

## History
Soundex was developed by Robert C. Russell and Margaret King Odell and patented in 1918 and 1922. A variation, American Soundex, was used in the 1930s for a retrospective analysis of the US censuses from 1890 through 1920. The Soundex code came to prominence in the 1960s when it was the subject of several articles in the Communications and Journal of the Association for Computing Machinery, and especially when described in Donald Knuth's The Art of Computer Programming.

The National Archives and Records Administration (NARA) maintains the current rule set for the official implementation of Soundex used by the U.S. government. These encoding rules are available from NARA, upon request, in the form of General Information Leaflet 55, "Using the Census Soundex".

## American Soundex
The Soundex code for a name consists of a letter followed by three numerical digits: the letter is the first letter of the name, and the digits encode the remaining consonants. Consonants at a similar place of articulation share the same digit so, for example, the labial consonants B, F, P, and V are each encoded as the number 1.

The correct value can be found as follows:

1. Retain the first letter of the name and drop all other occurrences of a, e, i, o, u, y, h, w.
2. Replace consonants with digits as follows (after the first letter):  
* b, f, p, v → 1  
* c, g, j, k, q, s, x, z → 2  
* d, t → 3  
* l → 4  
* m, n → 5  
* r → 6  

3. If two or more letters with the same number are adjacent in the original name (before step 1), only retain the first letter; also two letters with the same number separated by 'h' or 'w' are coded as a single number, whereas such letters separated by a vowel are coded twice. This rule also applies to the first letter.
4. If you have too few letters in your word that you can't assign three numbers, append with zeros until there are three numbers. If you have four or more numbers, retain only the first three.  

Using this algorithm, both "Robert" and "Rupert" return the same string "R163" while "Rubin" yields "R150". "Ashcraft" and "Ashcroft" both yield "A261". "Tymczak" yields "T522" not "T520" (the chars 'z' and 'k' in the name are coded as 2 twice since a vowel lies in between them). "Pfister" yields "P236" not "P123" (the first two letters have the same number and are coded once as 'P'), and "Honeyman" yields "H555".

The following algorithm is followed by most SQL languages (excluding PostgreSQL):

1. Save the first letter. Map all occurrences of a, e, i, o, u, y, h, w. to zero(0)
2. Replace all consonants (include the first letter) with digits as in [2.] above.
3. Replace all adjacent same digits with one digit, and then remove all the zero (0) digits
4. If the saved letter's digit is the same as the resulting first digit, remove the digit (keep the letter).
5. Append 3 zeros if result contains less than 3 digits. Remove all except first letter and 3 digits after it (This step same as [4.] in explanation above).

The two algorithms above do not return the same results in all cases primarily because of the difference between when the vowels are removed. The first algorithm is used by most programming languages and the second is used by SQL. As examples, both "Robert" and "Rupert" yield "R163", while "Tymczak" yields "T520" and "Honeyman" yields "H555". In designing an application, which combines SQL and a programming language, the architect must decide whether to do all of the Soundex encoding in the SQL server or all in the programming language. The MySQL implementation can return more than 4 characters.

From https://en.wikipedia.org/wiki/Soundex, retrieved 2/22/2020.
