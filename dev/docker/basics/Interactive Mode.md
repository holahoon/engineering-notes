I can make the docker shell interactive. If I have a simple input/output code say with Python:
```python
from random import randint

min_number = int(input('Please enter the min number: '))
max_number = int(input('Please enter the max number: '))

if (max_number < min_number):
	print('Invalid input - shutting down...')
else:
	rnd_number = randint(min_number, max_number)
	print(rnd_number)
```
This code doesn't create any type of server.
If I just build an image and try to run a container, it will just exit out by throwing an error.

I can make this an interactive container by adding:
```bash
$ docker run -it <image-id>
```
Notice how the command shell is not interactive!

## Restart

When done inputing all the fields, it will exit out of the interactive shell and will kill the container. What if I want to restart it? I know I can just grab the container name and run:
```bash
$ docker start <container-name>
```
This is fine... but it behaves really weirdly...
And it will be in detached mode as well.

Here's a niche way of doing so:
```bash
$ docker start -a -i <container-name>
```
`-a` is a command for attached mode, and `-i` is a command for interactive mode!
