# LogCrawler

## Usage
`logcrawler <search - path>[ <search - path> ...][<options>]`

scans recursively the specified search paths for files matching a certain regex. Those files can be copied to a folder or searched for a combination of search strings. In this case only those lines with unique timestamps are copied to the output file.
Unless the option --nozip is used, archives are scanned as well.

## available options:
```

Usage: logcrawler <search - path>[ <search - path> ...][<options>]

scans the specified search paths and archives it finds on its way recursively
for files matching a certain regex.
Those files can be copied to a folder or searched for a combination of search
strings. In this case only those lines with unique timestamps are copied to the
output file.

available options:
  -h [ --help ]                         produce help message
  -p [ --search-path ] arg              search path. Can be a single file,
                                        archive or directory
  -f [ --filespec ] arg                 file specification (regexp). Process
                                        only files matching any of these file
                                        specifications.
  -e [ --exclude ] arg                  file specification (regexp). Do not
                                        process files matching any of these
                                        file specifications.
  --nozip                               do not recurse into archives files
  --nocrc                               Do not calculate crc of files prior to
                                        scanning and do not skipscanning if
                                        file has already been scanned.
  -q [ --quiet ]                        Quiet mode, suppress progress messages.
  -x [ --extract ] arg                  extract files to path, do not scan
                                        them.
  -l [ --file-list ] [=arg(=filelist.txt)]
                                        together with -x: create a file list
                                        indicating the origin of the extracted
                                        files.
  -s [ --search ] arg                   extract only lines containing all of
                                        the specified strings. More than one
                                        search strings can be specified per
                                        option. Several -s options can be used,
                                        which are and all lines are extracted,
                                        which match one of them.
  --parse arg                           parse files according to the specified
                                        regex and dump the result in table form
                                        to the specified output file, with one
                                        column per match-group. Note that the
                                        regex must match the input string
                                        completely to generate an entry in the
                                        output table.
  -o [ --output-file ] arg              writes lines sorted by date to output
                                        file. Outputs to stdout if no file name
                                        is given.
  -m [ --output-matlab ] arg            writes lines sorted by date to binary
                                        output file optimized for use with
                                        matlab.Outputs to stdout if no file
                                        name is given.
  -d [ --date-fmt ] arg                 date format. See below for examples and
                                        syntax.
  -v [ --verbose ]                      displays the options used and all data,
                                        and extracted string while scanning.


Examples:

logarchive c:\logfiles c:\logarchive.zip -f "Log.*\.txt" "Log*.old" -e c:\temp
Scans the folder c:\logfiles and the archive c:\logarchive.zip for files
Log*.txt and Log*.old
All files found are copied to c:\temp\*.*. Duplicate files are skipped, files
with the same name but different content (as determined by a checksum) are
renamed according to the scheme <filename>_<n>.<ext>, with n counting from one
upwards.

filelist.txt:

If the option -l is specified a file filelist.txt is created in the target
folder which allows to identify the source location of each file found.
The format of the file follows the following example:
"Logfile_56.txt","\\?\c:\logfiles\logarchive_20170303.zip\C:/Log/Logfile.txt"

Syntax of date-time-format specification (see date-fmt option) can be found here
:
  http://www.boost.org/doc/libs/1_62_0/doc/html/date_time/date_time_io.html

Examples:
  %d-%b-%Y %H:%M:%S%f : 16-mar-2017 19:05:20.458 (default)
  %Y-%m-%d %H:%M:%S%f : 2017-03-22 08:13:53.686

For more documentation:
https://github.com/mibcoder/LogCrawler/blob/master/README.md
Fork me on GitHub: https://github.com/mibcoder/LogCrawler
```

## Supported archive formats

Logarchive supports natively only the zip format. If 7z.dll is present in the search path, the following other formats are also supported:

- .7z
- .gz
- .tar
- .tgz
- .cab

## Date and time format

For date and time parsing, [boost datetime](http://www.boost.org/doc/libs/1_62_0/doc/html/date_time/date_time_io.html) is used. The Syntax of the specifications (see date-fmt option) can be found in the link.  

Examples:
  - %d-%b-%Y %H:%M:%S%f : 16-mar-2017 19:05:20.458 (default)
  - %Y-%m-%d %H:%M:%S%f : 2017-03-22 08:13:53.686

## Examples:

### Extraction mode

`logarchive c:\logfiles c:\logarchive.zip -f "Log.*\.txt" "Log*.old" -e c:\temp`

Scans the folder `c:\logfiles` and the archive `c:\logarchive.zip` for files `Log*.txt` and `Log*.old`.
All files found are copied to `c:\temp\*.*`. Duplicate files are skipped, files with the same name but different content (as determined by a checksum) are renamed according to the scheme `<filename>_<n>.<ext>`, with n counting from one upwards.

#### filelist.txt:

If the option -l is specified a file filelist.txt is created in the target folder which allows to identify the source location of each file found. The format of the file follows the following example:

`"Logfile_56.txt","\\?\c:\logfiles\logarchive.zip\C:/Log/Logfile.txt"`

### Search mode

If a search string is specified, the files are searched for lines matching the search criteria and written to the output file. If no output file option is specified, it is written to console output.


## ToDo
- In Search Mode only the timestamp decides if a line is a duplicate and skipped. The right way would be to only skip real duplicates (based on text) and also remember the sequence of lines, which have identical timecodes.
- Make use of multi-threading (for instance on file level)
- Provide makefile for compile on linux.
- Add support for other archive formats.
- Make timestamp format configurable.
