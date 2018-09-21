# Bash

Copy a folder from machine **A** to machin **B**, while remotely logged into machine **A** through ``ssh``.
```
scp -rp /folder user@ip:/folder
```
To copy a file from **B** to **A** while logged into **B**:
```
scp /path/to/file username@a:/path/to/destination
```

To copy a file from **B** to **A** while logged into **A**:
```
scp username@b:/path/to/file /path/to/destination
```