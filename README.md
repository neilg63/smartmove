# SmartMove Quick Bulk File Management Utility

Smartmove is a command line tool that lists, moves or deletes large numbers of files filtered by age, file extension and/or size range.
It does not seek to replace common utilities such as _ls_, _find_ and _exa_ combined with _mv_ and _rm_, but provides a more transparent streamlined workflow when managing large volumes of files.

NB: This is currently under development in its alpha and should be used with caution. I have now added the move and delete functionaliy, but still need to manage hidden files and test on different file systems.

The following command will give you and overview of all jpeg, gif and png files in the target directories and subdirectories thereof to a max depth of 3 with a minimum file size of 5M and minimum age of 30 days
`smartmove -ext jpg,jpeg,gif,png --size 5M --max-depth 3 --before 30`

The -l flag reveals individual file entries with their age, date, type, and relative path.
`smartmove -ext jpg,jpeg,gif,png --size 5M --max-depth 3 --before 30  -l`

Should you wish to move these files to a target directory, respecting the original nested file structure, add a --move flag.
`smartmove -ext jpg,jpeg,gif,png --size 5M --max-depth 3 --before 30  -l --move /extended-drive/media`

Should you wish to delete these files, add a remove flag
`smartmove -ext jpg,jpeg,gif,png --size 5M --max-depth 3 --before 30  -l --delete`

## Arguments

- --before, -b only files modified before the specified number of days ago, --before 30 older than 30 days
- --after, -a only files modified after the specified number of days ago, --newer 30 newer than 30 days
- --size, -s file size range with k (KB), m (MB) or g (GB) unit suffixes. e.g. 1-2M = 1MB to 2MB. One size alone is assumed to be the minimum. To set only a maximum prefix with a comma ( ,5MB) or use a 0-5M range.
- --ext, -e extensions, omit to allow all extensions
- --not_ext, -n extensions to be excluded, e.g. move or delete all files that do not include these extensions
- --list, -l Flag to show individual file details rather than just the overview
- --groups, -g Flag to show stats by extension groups before the overview
- --max-depth, -d Max depth of subdirectories to scan. Defaults to 255 (pratcically unlimited).
- --pattern, -p Match pattern for the file name
- --move, -m Move to specified new target directory
- --delete, -u Delete files filtered by the above criteria
- --force, -f Bypass prompt for bulk deletion (useful for cron jobs)
