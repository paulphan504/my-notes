##### [**Show hidden files and folders within Mac terminal**](https://stackoverflow.com/questions/71983948/show-hidden-files-and-folders-within-mac-terminal)

Have you tried using the -a flag for ls?

1. To list all files (including hidden files): `$ ls -a`
2. To list all files in a list with more details: `$ ls -al`
3. Alias (shortcut) for 2. `$ ll`

`ls` itself usually ignores entries starting with '.', and the `-a` or `--all` flag ignores that protection.

Use `ls --help` in your terminal to review all available flags.

#### [How to CD to Your iCloud Drive on a Mac](https://www.wikihow.com/CD-to-iCloud-Drive-on-Mac)

```
cd ~/Library/Mobile\ Documents/
```

![[Pasted image 20241008205715.png]]




