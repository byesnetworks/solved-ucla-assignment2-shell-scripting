Download Link: https://assignmentchef.com/product/solved-ucla-assignment2-shell-scripting
<br>
<h2>Laboratory: Spell-checking Hawaiian</h2>

Keep a log in the file <samp>lab2.log</samp> of what you do in the lab so that you can reproduce the results later. This should not merely be a transcript of what you typed: it should be more like a true lab notebook, in which you briefly note down what you did and what happened.

For this laboratory we assume you’re in the standard C or <a href="https://en.wikipedia.org/wiki/POSIX">POSIX</a> <a href="https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap07.html#tag_07">locale</a>. The shell command <a href="https://pubs.opengroup.org/onlinepubs/9699919799/utilities/locale.html"><samp>locale</samp></a> should output <samp>LC_CTYPE=”C”</samp> or <samp>LC_CTYPE=”POSIX”</samp>. If it doesn’t, use the following shell command:

<pre><samp><a href="https://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html#export">export</a> LC_ALL='C'</samp></pre>

and make sure <samp>locale</samp> outputs the right thing afterwards.

We also assume the file <samp>words</samp> contains a sorted list of English words. Create such a file by sorting the contents of the file <samp>/usr/share/dict/words</samp> on the SEASnet GNU/Linux hosts, and putting the result into a file named <samp>words</samp> in your working directory. To do that, you can use the <samp><a href="https://pubs.opengroup.org/onlinepubs/9699919799/utilities/sort.html">sort</a></samp> command.

Then, take a text file containing the HTML in this assignment’s web page, and run the following commands with that text file being standard input. Describe generally what each command outputs (in particular, how its output differs from that of the previous command), and why.

<pre><samp>tr -c 'A-Za-z' '[
*]'tr -cs 'A-Za-z' '[
*]'tr -cs 'A-Za-z' '[
*]' | sorttr -cs 'A-Za-z' '[
*]' | sort -utr -cs 'A-Za-z' '[
*]' | sort -u | comm - wordstr -cs 'A-Za-z' '[
*]' | sort -u | comm -23 - words</samp></pre>

Let’s take the last command as the crude implementation of an English spelling checker. Suppose we want to change it to be a spelling checker for <a href="https://en.wikipedia.org/wiki/Hawaiian_language">Hawaiian</a>, a language whose traditional orthography has only the following letters (or their capitalized equivalents):

<pre><samp>p k ' m n w l h a e i o u</samp></pre>

In this lab for convenience we use ASCII apostrophe (‘) to represent the Hawaiian ‘okina (‘); it has no capitalized equivalent.

Create in the file <samp>hwords</samp> a simple Hawaiian dictionary containing a copy of all the Hawaiian words in the tables in “<a href="http://mauimapp.com/moolelo/hwnwdseng.htm">English to Hawaiian</a>“, an introductory list of words. Use <a href="https://www.gnu.org/software/wget/">Wget</a> to obtain your copy of that web page. Extract these words systematically from the tables in “English to Hawaiian”. Assume that each occurrence of “<samp>&lt;tr&gt; &lt;td&gt;<var>Eword</var>&lt;/td&gt; &lt;td&gt;<var>Hword</var>&lt;/td&gt;</samp>” contains a Hawaiian word in the <samp><var>Hword</var></samp> position. Treat upper case letters as if they were lower case; treat “<samp>&lt;u&gt;a&lt;/u&gt;</samp>” as if it were “<samp>a</samp>“, and similarly for other letters; and treat <samp>`</samp> (ASCII grave accent) as if it were <samp>‘</samp> (ASCII apostrophe, which we use to represent ‘okina). Some entries, for example “<samp>H&lt;u&gt;a&lt;/u&gt;lau, kula</samp>“, contain spaces or commas; treat them as multiple words (in this case, as “<samp>halau</samp>” and “<samp>kula</samp>“). You may find that some of the entries are improperly formatted and contain English rather than Hawaiian; to fix this problem reject any entries that contain non-Hawaiian letters after the abovementioned substitutions are performed. Sort the resulting list of words, removing any duplicates. Do not attempt to repair any remaining problems by hand; just use the systematic rules mentioned above. Automate the systematic rules into a shell script <samp>buildwords</samp>, which you should copy into your log; it should read the HTML from standard input and write a sorted list of unique words to standard output. For example, we should be able to run this script with a command like this:

<pre><samp>cat foo.html bar.html | ./buildwords | less</samp></pre>

If the shell script has bugs and doesn’t do all the work, your log should record in detail each bug it has.

Modify the last shell command shown above so that it checks the spelling of Hawaiian rather than English, under the assumption that <samp>hwords</samp> is a Hawaiian dictionary. Input that is upper case should be lower-cased before it is checked against the dictionary, since the dictionary is in all lower case.

Check your work by running your Hawaiian spelling checker on this web page (which you should also fetch with Wget), and on the Hawaiian dictionary <samp>hwords</samp> itself. Count the number of “misspelled” English and Hawaiian words on this web page, using your spelling checkers. Are there any words that are “misspelled” as English, but not as Hawaiian? or “misspelled” as Hawaiian but not as English? If so, give examples.

<h2>Homework: Find improperly marked files</h2>

<em>Warning: it will be difficult to do this homework without attending the lab session for hints.</em>

You’re working in a project that has lots of text files. Some of them are in plain <a href="https://en.wikipedia.org/wiki/ASCII">ASCII</a>; others are in <a href="https://en.wikipedia.org/wiki/UTF-8">UTF-8</a>, which is a superset of ASCII. None of the files are supposed to contain NUL bytes (all bits zero). The UTF-8 files that contain non-ASCII characters are supposed to have a first line containing the string “<samp>-*- coding: utf-8 -*-</samp>” (without the quotation marks).

<a href="https://en.wikipedia.org/wiki/POSIX">POSIX</a> systems typically support various <a href="https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap07.html#tag_07">locales</a> to let applications operate well in various countries, languages, and character encodings. The shell command <samp><a href="https://pubs.opengroup.org/onlinepubs/9699919799/utilities/locale.html">locale -a</a></samp> outputs all the locales available on your system, and should list among others the <samp>C</samp>, <samp>es_MX.utf8</samp>, and <samp>ja_JP.utf8</samp> locales. You can select a locale by setting the <samp>LC_ALL</samp> <a href="https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap08.html">environment variable</a> with a shell command like the following:

<pre><samp><a href="https://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html#export">export</a> LC_ALL=en_US.utf8</samp></pre>

This setting takes effect for all programs later invoked. You can see your current locale settings by running the <samp>locale</samp> command without any arguments. You can see how the locale affects the output of some standard utilities by running the <samp>date</samp> and <samp>ls -l</samp> commands, using the abovementioned locales.

Associated with each locale is a <a href="https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap06.html#tag_06_02">character encoding</a> that specifies how characters in that locale are represented as bytes. In the <samp>C</samp> locale on GNU/Linux systems, each character represents a single byte using the <a href="https://en.wikipedia.org/wiki/ASCII">ASCII</a> encoding for bytes whose values are in the range 0–127; bytes outside of that range do not represent any characters, and are considered to be encoding errors. The <samp>en_US.utf8</samp> locale is like the C locale, except that some (but not all) short sequences of bytes in the range 128–255 represent non-ASCII characters; bytes that are not in such a sequence are considered to be encoding errors.

<ol>

 <li>Write a shell script <samp>find-ascii-text</samp> that accepts one or more arguments, and outputs a line for each argument that names an ASCII text file (i.e., an ASCII file containing no NUL bytes). If an argument names a directory, your script should recursively look at all files under that directory or its subdirectories.</li>

 <li>Write a similar shell script <samp>find-utf-8-text</samp> that works like <samp>find-ascii-text</samp>, except that it outputs a line only for UTF-8 text files (i.e., UTF-8 files containing no NUL bytes) that are not ASCII text files.</li>

 <li>Write a shell script <samp>find-missing-utf-8-header</samp> that accepts one or more arguments, and outputs a line for each argument naming a UTF-8 text file that lacks the “<samp>-*- coding: utf-8 -*-</samp>” string in the first line. If an argument names a directory, the command should search the directory recursively.</li>

 <li>Write a shell script <samp>find-extra-utf-8-header</samp> that is like <samp>find-missing-utf-8-header</samp> except it outputs a line for each ASCII text file that has the “<samp>-*- coding: utf-8 -*-</samp>” string in the first line.</li>

</ol>

You need not worry about the cases where your scripts are given no arguments. However, be prepared to handle files whose names contain special characters like spaces, “*”, and leading “–”. You need not worry about file names containing newlines.

Your script should be runnable as an ordinary user, and should be portable to any system that supports <a href="https://www.gnu.org/software/grep/">GNU <samp>grep</samp></a> along with the other <a href="https://pubs.opengroup.org/onlinepubs/9699919799/utilities/toc.html">standard POSIX shell and utilities</a>; please see its <a href="https://pubs.opengroup.org/onlinepubs/9699919799/idx/utilities.html">list of utilities</a> for the commands that your script may use. (Hint: see the <samp>find</samp>, <samp>head</samp> and <samp>tr</samp> utilities.) With GNU <samp>grep</samp>, the pattern <samp>.</samp> (period) matches only individual characters in the current locale; it does not match encoding errors. GNU <samp>grep</samp> has special treatment of files with encoding errors, or files containing NUL characters; see its <samp>–binary-files</samp> option. You may also want to look at GNU <samp>grep</samp>‘s <samp>-H</samp>, <samp>-m</samp>, <samp>-n</samp>, <samp>-l</samp>, <samp>-L</samp>, and <samp>-v</samp> options.

When testing your script, it is a good idea to do the testing in a subdirectory devoted just to testing. This will reduce the likelihood of messing up your home directory or main development directory if your script goes haywire.

<h2>Submit</h2>

Submit the following files. <em>Warning: You should edit your files with Emacs or with other editors that end lines with <samp>‘
’</samp>.</em> Do not use Notepad or similar tools that may convert line endings to <a href="https://en.wikipedia.org/wiki/CRLF">CRLF</a> form.

<ul>

 <li>The script <samp>buildwords</samp> as described in the lab.</li>

 <li>The file <samp>lab2.log</samp> as described in the lab.</li>

 <li>The four scripts described in the homework.</li>

</ul>

All files should be ASCII text files, with no carriage returns, and with no more than 80 columns per line. The shell command:

<pre><samp>awk '/r/ || 80 &lt; length' lab2.log find-ascii-text find-utf-8-text find-missing-utf-8-header find-extra-utf-8-header</samp></pre>

should output nothing.

<address> </address>