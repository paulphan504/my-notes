##### [**Show hidden files and folders within Mac terminal**](https://stackoverflow.com/questions/71983948/show-hidden-files-and-folders-within-mac-terminal)

Have you tried using the -a flag for ls?

1. To list all files (including hidden files): `$ ls -a`
2. To list all files in a list with more details: `$ ls -al`
3. Alias (shortcut) for 2. `$ ll`

`ls` itself usually ignores entries starting with '.', and the `-a` or `--all` flag ignores that protection.

Use `ls --help` in your terminal to review all available flags.

recovery




