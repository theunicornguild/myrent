### Creating the environment

We will start by creating our development environment, in the terminal let's create a Development folder:

```bash
mkdir ~/Development
cd ~/Development
```

### Installing virtualenv

Let’s install virtualenv (you can skip this step if you have virutalenv pre-installed):

```bash
pip install virtualenv
```

Then we can create our virtualenv by typing

```bash
virtualenv --python=python3 myrentenv
```

or for windows

```bash
Virtualenv --python=”C:\Python3.7\python.exe” myrentenv
```

### Activate the virtualenv

For MacOS/Linux

```bash
source myrentenv/bin/activate
```

For Windows

```bash
myrent\Scripts\activate
```

### Cloning the repo

```bash
git clone https://github.com/JoinCODED/myrent.git
```

Now let’s go to the newly cloned project and see what we have there

```bash
cd myrent
ls -la
```
