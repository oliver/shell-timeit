# shell-timeit

`timeit` runs a shell command multiple times, measures the execution time of each run, and prints the min/max/average times.
It can also collect results in a file and create a diagram of all collected results.

Shell Timeit was inspired by the Python Timeit module.

## Example ##
Run `dd` for in-memory copying with different block sizes:
* each size is tested five times (`-r 5`) instead of the default of three times
* the command is executed as shell command to allow redirecting output to `/dev/null` (`-s`)
* results are written to dd.json (`-f dd.json`)
* a comment is added for every test (`-m ...`)
* results are afterwards turned into a diagram (`-o dd.png`)

```shell
for s in 1 2 5 10 20 50 100 200 500 1000 2000 5000 10000 20000 50000; do
    echo "testing with bs="$s"kB"
    timeit -r 5 -s -f dd.json -m "block size: $s kB" "dd if=/dev/zero of=/dev/null bs=$s"kB" count=$((1000000/$s)) >/dev/null 2>&1"
done
timeit -f dd.json -o dd.png
```

Result:

![dd.json diagram](dd.png)

The bars show the lowest execution time; the small dots are the results of each single run.

The diagram displays lowest time prominently because this is the fastest time that was achievable with the specified settings; the higher values just indicate that something interfered with the run.
