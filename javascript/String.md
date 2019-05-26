# String

#### String.prototype.localeCompare

Used when comparing string with accents.

| Character | Encoding | hex   | dec (bytes) | dec   | binary            |
|-----------|----------|-------|-------------|-------|-------------------|
| z         | UTF-8    | 7A    | 122         | 122   | 01111010          |
| é         | UTF-8    | C3 A9 | 195 169     | 50089 | 11000011 10101001 |

#### String.prototype.normalize

Say we need to search for a string `Côte et Ciel`, but using `indexOf("Cote") > -1;` won't return us any results. We need to “decompose” the string so that any characters with diacritical marks are represented by their two-byte surrogate pair.

So we do `normalize('NFD').replace(/[\u0300-\u036f]/g, '').indexOf("Cote") > -1;`.

The replace function takes a range of Unicode points from U+0300 to U+036F, covering any diacritic bytes we might have in the string.

[normalize Unicode strings](https://withblue.ink/2019/03/11/why-you-need-to-normalize-unicode-strings.html?utm_source=ponyfoo+weekly&utm_medium=email&utm_campaign=159)




