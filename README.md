# SmartMove Quick Bulk File Management Utility

Smartmove is a command line file management tool. It lets you list, move or delete large numbers of files in nested folders filtered by age, file extension, file name pattern and/or size range.
It does not seek to replace common utilities such as _ls_, (_dir_) and _find_ combined with _mv_ and _rm_ (_move_ or _del_), but provides a more transparent overview and streamlined workflow when managing large volumes of files.

This utility is still under development and should be used with caution. I have added the _move_ and _delete_ functionaliy, but still need to improve feedback and test on different file systems and operating systems. The application leverages contributed packages which are all cross-platform and should work on recent versions of Linux, Mac and Windows.

## Primary use cases

- Summarise file directory contents by size, age and extensions (-g flag)
- Filter file listings by age, size, extension(s) and/or file name pattern
- Move filtered files to another directory
- Delete filtered files (prompted without -f flag)

## Drawbacks

- Reading deeply nested directories with large numbers of files can be slow. The default max depth is thus set to 5. If you just want to find out the total disk usage, use `du -ch --max-depth 1` instead.
- If the target path ends in a filename with a wildcard, the command line interpreter will expand it internally into an array all matching file names. This is inefficient for 100 or more matching file names. Instead use the `-e jpeg,jpg` extension or `-p file_name_pattern` options when filtering by name or extension on thousands of files.

The following command will give you an overview of all jpeg, gif and png files in the target directories and subdirectories thereof to a max depth of 3 with a minimum file size of 5M and minimum age of 30 days

`smartmove -e jpg,jpeg,gif,png --size 5M --max-depth 3 --before 30`

The -l flag reveals individual file entries with their age, date, type and relative path.

`smartmove -e jpg,jpeg,gif,png --size 5M --max-depth 3 --before 30  -l`

Should you wish to move these files to a target directory, respecting the original nested file structure, add a --move flag.

`smartmove -e jpg,jpeg,gif,png --size 5M --max-depth 3 --before 30  -l --move /extended-drive/media`

Should you wish to delete these files, add a `--delete` or `-u` flag (`-d` stands for max depth)

`smartmove -e jpg,jpeg,gif,png --size 5M --max-depth 3 --before 30  -l --delete`

## Arguments

- **--before, -b** only files modified before the specified number of days ago, `--before 30` _older than 30 days_. For other periods, you may use the suffixes `s` for seconds, `m` for minutes, `h` for hours, `w` for weeks or `y` for years, e.g. `5m` _5 minutes_ . You may add a range either via -a (--after) or simply with a dash, e.g. `-b 7-14` means between 7 and 14 days old while `-b 30m-12h` means between 30 minutes and 12 hours old.
- **--after, -a** only files modified after the specified number of days ago, `--after 30` _newer than 30 days_ . This may be combined with -b (--before) for an age range.
- **--size, -s** file size range with k (KB), m (MB) or g (GB) unit suffixes. e.g. 1-2M = 1MB to 2MB. One size alone is assumed to be the minimum. To set only a maximum prefix with a comma ( ,5MB) or use a 0-5M range.
- **--ext, -e** extensions, omit to allow all extensions
- **--not-ext, -n** extensions to be excluded, e.g. move or delete all files that do not include these extensions
- **--exclude-dirs, -q** directories to be excluded. These are relative to the target directory. If prefixed by your system's directory separator (`/` on Linux and Mac and `\` on Windows), it will exclude all subdirectories starting from the parent directory, otherwise it will exclude all subdirectories at any nesting level. You may exclude multiple subdirectory path with comma-separated lists e.g. `/node_modules,/dist` will exclude all files nested in these subdirectories.
- **--list, -l** Flag to show individual file details rather than just the overview
- **--groups, -g** Flag to show stats by extension groups before the main overview
- **--max-depth, -d** Max depth of subdirectories to scan. Defaults to 5 to limit overhead of parsing deeply nested directories. Max value is 255.
- **--pattern, -p** Match pattern for the file name. Add the `-x` flag to use full regular expressions in quotes.
- **--omit-pattern, -o** Omit file names matching this pattern. This may be combined with `--pattern, -p` or `--ext, -e` for more advanced pattern matching.
- **--starts-with** Match pattern from the start of the file name
- **--ends-with** Match pattern from the end of the file name, with or without the extension
- **--regex-mode, -x** Flag to interpret the above pattern as a full regular expression, e.g. where `a*` means any number of the preceding character, otherwise _\*_ is a wildcard for any characters, which in full regex mode is `.*`. For simple pattern matches `.` is interpreted literally, while in full regex mode it means any character and must be escaped to match a dot.
- **--copy, -c** Copy to specified new target directory. Takes precedence over `--move, -m`;
- **--move, -m** Move to specified new target directory
- **--delete, -u** Delete files filtered by the above criteria
- **--force, -f** Bypass prompt for bulk deletion (useful for cron jobs)
- **--hidden, -y** Match hidden files and directories, e.g. `.git` as folder or `.gitignore` as a file

## Installation

- First ensure you have installed the [Rust Cargo compiler](https://doc.rust-lang.org/cargo/getting-started/installation.html) for your operating system
- checkout out the repository and change into its directory
- Run `cargo build --release`
- The executable will be at `target/release/smartmove`
- Add an alias to the file or ideally move it into a directory already in your system's export path. On Linux and Mac, this may be `/usr/local/bin`.
