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
virtualenv --python=”C:\Python3.7\python.exe” myrentenv
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

1. If you plan on following the `Git workflow` then **fork** the following repo: https://github.com/JoinCODED/myrent
2. Clone the repo you forked:

```bash
git clone https://github.com/<your-github-username>/myrent.git
```

### Installing requirments

We need to install the packages required to complete this project:
```bash
cd myrent
pip install -r requirments.txt
```

### Exploring the project

Now let’s go to the newly cloned project, and see what we have there:

```bash
ls -la
```

We can see there are a few folders that we created beforehand for you:
- `accounts`: Will handle the user authentication functionalities
- `main`: Will handle the renter models and views to display/add/delete/list renters
- `transactions`: Will handle the payment gateway that we use, in this case TAP.

You will also notice that in some of these apps, we have already provided you with the `templates` folders.

