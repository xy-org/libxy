# Unicode

## General

 * The entire unicode space is 21-bit large.
 * It is devided into 16-bit planes.
 * The first plane (U+0000-U+FFFF) is called Basic Multilingual Plane (BMP)
 * Each point in the 21-bit unicode space is called a _code point_ (colloquially character).

## Encodings

* UTF-8: Packs (compresses) characters well minimizing memory but at increased cost for processing.
  If text is to be stored on disk or transmitted over a channel this is the encoding to use.
* UTF-32: Stores each character in a 32-bit variable. 11 bits always go unsued i.e. at least 34% of memory is waisted.
  On the flip side each character can be accessed directly.
* UTF-16: IMHO this is the worst of all encodings - it is variable length like UTF-8 i.e. no direct access to
  individual characters; in terms of memory it is very efficient if using characters in BMP but not ascii (U+0080-U+FFFF)
  and wastefull otherwise. If all we need is ascii then 50% is wasted. And in the case of characters beyond BMP it is 34% wasteful.
* Dynamic: Python appears to use a kind of "dynamic" encoding: if only characters  in the range U+0000-U+007F are present then it uses 1 bytes per character i.e. og ascii. The moment a character in BMP appears it switches to UTF-16.
And when a character outside of BMP is needed then it switches to UTF-32\. Thus we always have direct access
to each character. Memory unilization is dynamic i.e. highly dependant on the input. If all we use are characters in BMP it is pretty good. However
the moment we use a character outside (e.g. an emoji) the string bloats to 4 bytes per character.

## Length Variability

This is a "theme" that repeats a couple of times. First we have the encoding - common symbols are in BMP, so we want to use a space saving encoding, but these encoding make processing difficult. Even if we just use 4 bytes per character and ignore the issue of memory utilization then we have _combining characters_. These are code points that modify the preceeding character in a certain way (usually by adding a diacritical mark or similar). Take for example the following sequence "\U+0041\U+0305" (A̅ - A with overlay). Depending on the language this may be a single letter bug it consists of 2 unicode code points, and may get encoded in 8 bytes. Now if we equate letters, characters, and code points (which you shouldn't) then this string should have a length of 2 but most people will say it is a single letter with "stuff" at the top. There can be any number of combining characters modifying a syngle "normal" character.

A sequence of one or more code points that are displayed as a graphical unit that a reader recognizes as a single element of the writing system is called *grapheme* (Colloquially called a letter in western languages).

So this gets very confusing very quickly. E.g. "pɔlʂt͡ʂɨzna". The string has a code-point len of 11\. But is this the number of letter (graphemes) it has? What is that line over the 't' and 's' symbols in the middle? Is that a letter? If we execute `"pɔlʂt͡ʂɨzna"[5]` in python we get some weird symbol?

XY's approach to strings and unicode is to ebrace the chaos of it all instead of trying to control it. And embrace the _variability_ of it all. Both in terms of encodings and representations. But also in terms of interpretation.

## Spec

The "spec" can be found at https://www.unicode.org/Public/UNIDATA/
And description at https://www.unicode.org/reports/tr44/

Each code-point is described in a kind of database (`UnicodeData.txt`). That can be processed to build some kind
of structure needed to process the codes in a real application.

## Other

Could have been better designed but considering the incremental way it was developed and the need for backward compatibility, it is as good we are going to get.

## Resources

https://www.unicode.org/reports/
https://www.compart.com/en/unicode/
