# LogCrawler

##Usage: 
`logcrawler <search - path>[ <search - path> ...][<options>]`

scans the specified search paths and archives it finds on its way recursively for files matching a certain regex. Those files can be copied to a folder or searched for a combination of search strings. In this case only those lines with unique timestamps are copied to the output file.

##available options:
```
  -h [ --help ]                         produce help message
  -p [ --search-path ] arg              search path. Can be a single file,
                                        archive or directory
  -f [ --filespec ] arg                 file specification (regexp). Process
                                        only files matching any of these file
                                        specifications.
  --nozip                               do not recurse into archives files
  -q [ --quiet ]                        Quiet mode, suppress progress messages.
  -e [ --extract ] arg                  extract files to path, do not scan
                                        them.
  -l [ --file-list ] [=arg(=filelist.txt)]
                                        together with -e: create a file list
                                        indicating the origin of the extracted
                                        files.
  -s [ --search ] arg                   extract only lines containing all of
                                        the specified strings. More than one
                                        search strings can be specified per
                                        option. Several -s options can be used,
                                        which are and all lines are extracted,
                                        which match one of them.
  -o [ --output-file ] arg              writes lines sorted by date to output
                                        file. Outputs to stdout if no file name
                                        is given.
  -m [ --output-matlab ] arg            writes lines sorted by date to binary
                                        output file optimized for use with
                                        matlab.Outputs to stdout if no file
                                        name is given.
```

##Examples:

###Extraction mode

`logarchive c:\logfiles c:\logarchive.zip -f "Log.*\.txt" "Log*.old" -e c:\temp`

Scans the folder `c:\logfiles` and the archive `c:\logarchive.zip` for files `Log*.txt` and `Log*.old`.
All files found are copied to `c:\temp\*.*`. Duplicate files are skipped, files with the same name but different content (as determined by a checksum) are renamed according to the scheme `<filename>_<n>.<ext>`, with n counting from one upwards.

####filelist.txt:

If the option -l is specified a file filelist.txt is created in the target folder which allows to identify the source location of each file found. The format of the file follows the following example:

`"Logfile_56.txt","\\?\c:\logfiles\logarchive.zip\C:/Log/Logfile.txt"`

###Search mode

If a search string is specified, the files are searched for lines matching the search criteria and written to the output file. If no output file option is specified, it is written to console output.
