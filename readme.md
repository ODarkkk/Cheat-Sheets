# Cheatsheets

Just my Cheatsheets

## Installation

To get the command working on Linux, follow these steps:

1. Place the script inside the directory:
```bash
   ~/bin/
```

2. You'll need sudo permissions.

3. Rename the file to whatever command name you'd like to use — in my case, `cheatsheets`.

4. Run this command to give the file read and execute permissions:
```bash
   sudo chmod +xr ~/bin/cheatsheets
```

5. Run this command to reload your shell:
```bash
   echo 'export PATH="$HOME/bin:$PATH"' >> ~/.<shell>
   source ~/.<shell>
```
   The default shell is bash, so replace `<shell>` with `bashrc` (or your shell's config file if you use a different one).

6. Place the `.md` file in your user's home directory, e.g. for Kali:
```bash
   /home/kali/
```

> **Note:** Make sure your `.md` file uses Linux-compatible line endings (LF), not Windows-style (CRLF). If the file was created or edited on Windows, it may fail to be read correctly on Linux. You can convert it using:
> ```bash
> dos2unix ~/cheatsheets.md
> ```
> or, if `dos2unix` isn't installed:
> ```bash
> sed -i 's/\r$//' ~/cheatsheets.md
> ```

Your command should now work.

### How it works

Running:
```bash
cheatsheets
```
will display the entire `.md` file.

Running:
```bash
cheatsheets <search>
```
will display the content of matching headers and sub-headers.

It also works with `grep`:
```bash
cheatsheets <search> | grep "<search>"
```
