# SRTLab
*SubRip subtitle file converter*<br>
by Alexander Thomas, aka Dr. Lex (with contributions by Idiomdrottning)<br>
Current version: 0.97<br>
Contact: visit https://www.dr-lex.be/lexmail.html<br>
&nbsp;&nbsp;&nbsp;&nbsp;or use my gmail address "doctor.lex".


## What is it?
This is a Perl script that can perform certain operations on SubRip (.srt) subtitle files. For instance, it can scale and offset the time stamps of all subtitles based on two pairs of current and expected time values. It can also check files for subtitles that appear too briefly or overly long, and attempt to fix subs that appear too briefly (which is of course not always possible), as well as remove overlap between subtitles. It also has routines that attempt to remove annotations for the hearing impaired, if you want to convert such file to one with only dialog text.

It is a bit of a work in progress and I plan to add some more features, but I released it to the public since it is already useful in its current form.

## Installing
Put it anywhere in a place that is in your executable PATH. Or always run it by specifying its full path.

## Usage
To get extended help, run the script with the switch '-h'.

The normal mode of operation is to run the script as:<br>
`srtlab.pl input.srt > output.srt`<br>
Which uses the standard redirect mechanism to write the output to a file. When running the script without the '>' part, it will simply print output on the console.

BEWARE: do not try this:<br>
`srtlab.pl input.srt > input.srt`<br>
This will destroy input.srt and leave you with an empty file. If you want to overwrite the input directly, instead run the script as follows and be aware that you will not be able to fix any mistakes you made unless you have a back-up of the file:
`srtlab -e input.srt`

The -L option can only fix too short subtitles if there is enough empty time after them. Otherwise more manual work will be required to fix the poorly made subtitle file. This option does not shorten 'sticky' subtitles (i.e. that appear too long) because these can sometimes be intentional. You should check the reported sticky subs yourself and fix them if necessary. In case of overlapping subtitles, -L will cut off the first subtitle in an overlapping pair at the time where the second one starts.

The -H switch will cause the script to remove the most common non-verbal annotations in subtitles for the hearing impaired (like [CLEARS THROAT] and character names). This can be useful for people with normal hearing who want to play a film silently without missing out on any of the dialogue, or if you want to prepare a subtitle file for translation. You should combine -H with -c. If you provide the -H switch twice, it will try a wider range of patterns to strip non-dialogue subtitles. You should only use this if a single -H does not work satisfactorily, because -HH has a higher risk of damaging parts of dialogue.<br>
For instance, the regular -H will not work well if the file has been made with the kind of bad but popular OCR tool that believes every capital I is a lowercase L. In that case your best option is to go through the file and correct all those ‘I’s before running it through SRTLab, but the quickest option is just to use -HH.

A recommended invocation to get a clean file with only dialogue, is:<br>
`srtlab -vLcuHUw` (or with double H if the single one doesn't cut it.)

At this time the script will only work with UTF-8, UTF-16, Windows Latin 1, or Shift-JIS encoded files. It can detect UTF-x files both with and without a starting Unicode BOM character. If you have files in other encodings, you will either need to convert them to a known encoding first (UTF-8 recommended), or modify the last lines of the script to recognise those encodings. My advice is to write all your output srt files in Unicode unless your media player does not support it. The 8-bit encodings are a thing of the past.

## Note to developers
The code uses 'tab indenting'. The rule is very simple: one code indent level == one tab, and this is the *only* thing tabs may be used for. For any other whitespace formatting, use spaces. This makes it trivial to adjust indent width in any editor that has configurable tab width. If you edit the code and want to make a pull request, please try to follow this convention. Also, please try not to write read-only code.

## License
This program is released under the GNU General Public License. See the source file and COPYING for more details.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

