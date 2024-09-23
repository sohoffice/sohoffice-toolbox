## Get the directory where the script is located

Use readlink, not always available on all systems.

```bash
# Absolute path to this script, e.g. /home/user/bin/foo.csh
SCRIPT=$(readlink -f "$0")
# Absolute path this script is in, thus /home/user/bin
SCRIPTPATH=$(dirname "$SCRIPT")
```
