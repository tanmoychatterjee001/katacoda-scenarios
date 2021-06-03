# Override defaults
## Task
Based on what Ross Rossum's docker image `r-py:latest`, a smart team member named Ada Lovelace wants to prove that the 
difference in precision is noticed only because of the difference in the default print function in both the languages
Can you help Ada to demonstrate a different print of 'pi' in R so that it matches the output from python?

## Solution
create a separate directory in `/root` for this task in your terminal and change to that dir
for e.g. (you can also use the graphical right click options to do the same)

```
cd /root
mkdir similarpi
cd similarpi
```

Create the following files in that folder

precisepi.R
``` 
print("hello from R")
sprintf("%.15f",pi)
```

test.py
``` 
import math
print("hello from python")
print(math.pi)
```

testpi.sh
```
#!/bin/bash
Rscript precisepi.R
python3 test.py
```

Make sure the testpi.sh has executable permissions
```
chmod +x testpi.sh
```

``` 
docker run -it --rm -v /root:/ r-py:latest --entrypoint /testpi.sh
```

## Questions
* why did we need to mount?
* what is the difference between `docker exec` and `docker run`

