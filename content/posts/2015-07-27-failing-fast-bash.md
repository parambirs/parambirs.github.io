---
title: 'Failing fast in bash scripts'
date: 2015-07-27T00:00:00-07:00
tags: ["unix", "shell"]
summary: "Use the -e option or set -e in Bash scripts to halt execution upon encountering any non-zero exit status, ensuring immediate termination upon failure."
---

If you want your bash script to stop execution as soon as any of the commands returns a non-zero exit status, invoke bash with the `-e` option.

For example, the following script will continue to run, even though the sub-shell exited with the status 1.

```sh
#!/bin/bash 
( exit 1 ) # subshell that returns non-zero exit status
echo "This will be echoed"
```

However, if the script invokes bash with the `-e` option, the script ends as soon as the subshell returns 1.

```sh
#!/bin/bash -e
( exit 1 ) # subshell that returns non-zero exit status
echo "This will not be echoed"
```

Note: Although I’ve used the subshell as an example here, but the script will stop executing for any command failure e.g. `wget`, `curl`, etc.

Another way to enable bash scripts to stop execution on failure is by using the `set -e` option. This can be set at any point in the script.

```sh
#!/bin/bash
( exit 1 ) # subshell that returns non-zero exit status
echo "This will be echoed"
set -e # enable fail fast mode
( exit 1 ) # script will not execute beyond this
echo "This will NOT be echoed"
```

Lastly, the fail fast mode can be disabled by using `set +e` in the script.

```sh
#!/bin/bash
( exit 1 )
echo "This will be echoed"
set -e # enable fail fast mode
echo "Just echoing something"
set +e # disable fail fast mode
( exit 1 )
echo "This will STILL be echoed"
```
