# Create your own Docker Image
## Task
In P361 a team member by the name Ross Rossum recently joined. 
He is an expert in both R and Python programming language.

He thinks that 'pi' gives different precision results in R and Python. 
Unfortunately all other p361 team-members are either R or Python experts 
and do not have both R and Python installed on their systems. Hence they cannot test it out themselves
Ross Rossum is also quite new to the Docker world.

Can you help Ross Rossum in creating a docker image containing both R and Python so that he can show his findings without 
each of the team-members having to fiddle with installation of R / Python on their laptops.

## Solution

create a separate directory in `/root`for this task in your terminal and change to that dir
for e.g. (you can also use the graphical right click options to do the same)
```
mkdir testdocker
cd testdocker
```
Create the following files in that folder

test.R
``` 
print("hello from R")
print(pi)
```

test.py
``` 
import math
print("hello from python")
print(math.pi)
```

test.sh
```
#!/bin/bash
Rscript test.R
python3 test.py
```

Dockerfile
```
FROM r-base

RUN apt update
RUN apt -yq install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libsqlite3-dev libreadline-dev libffi-dev curl libbz2-dev
RUN wget https://www.python.org/ftp/python/3.9.1/Python-3.9.1.tgz
RUN tar -xf Python-3.9.1.tgz
RUN cd Python-3.9.1 && ./configure && make -j8 build_all && make -j8 install

COPY ./test.R ./test.py ./start.sh /
RUN chmod +x /start.sh
CMD ["/start.sh"]
```

`docker build -t r-py:latest .`

`docker run r-py:latest`


## Questions
* What is happening when docker build command is executed? - concept of layers
* What is happening when you run docker run <image> without specifying any other arguments - default cmd mentioned in the dockerfile is executed
* Any guesses what will happen if the Dockerfile instead had the following code (try it out yourself)
```
FROM r-base
COPY ./test.R /
CMD ["Rscript", "test.R"]

FROM python:3
COPY ./test.py /
CMD ["python3", "test.py"]
```

Multi-stage builds does not combine but overwrites so the above will only end up having python
