# crash-course
Collection of materials to bring new members of SUSA @ Berkeley up to speed. Hosted by and maintained by the [Statistics Undergrad Student Association (SUSA)](https://susa.berkeley.edu).

# Basics

All the code in this repository is expected to run on Python 3.6+

Various libraries needed to execute code samples and jupyter notebooks are listed in requirements.txt

If using conda, one can also use conda.txt to create the corresponding conda env. Then install the pip requirements.

```sh
conda create --name NAME --file conda.txt
```

To then download the libraries efficiently using pip, download this repository, and run

```sh
pip3 install -r requirements.txt
```

# Todo

These are a few core topics that we could use help writing tutorials on. In general, feel free to pull request any material you believe is relevant. Worst-case scenario, we just work with you to change a few lines or something.


## Notes:

We assume basic Python 3.5/3.6+ proficiency because so many people know it as a result of Data 8 & CS 61a. However, we do not expect any R proficiency.


## Areas of Contribution

* Basics
   * R
   * Bash (Shell scripting)
* Python
   * Web Scraping (BeatifulSoup4)
   * OS Scripting (Managing packages, working with directories)
* Data Science
   * __Python__:
        * Numpy
        * Pandas
        * Sci-Kit Learn
        * Matplotlib (and related imaging libraries)
   * __R__:
        * Literally I have no clue
* Deep Learning
   * __Python__:
        * Tensorflow
        * Keras
        * And models: CNNs, LSTMs, etc
   * __R__:
        * Again I have _no clue_

# Contributing to This Repository

Assistance is welcome. In order to modify this repo, you first need to know your organization status. You can be
1) In the susa-org organization
2) In susa-org and a member of the crashcourse team w/i the org
3) Not in susa-org

If you are in the susa-org **and** in the crashcourse team **(2)**, you're steps for contributing are:
1) Clone this repo
2) Just directly pull/push to this repo

If you are having trouble pulling, pushing to the repo, then it may be because the upstreams are set to be the actual repo. To confirm this, you can run ``` git remote -v ``` and look for (upstream) listed somewhere. Then you just run ```git pull upstream [working_branch]``` and ```git push upstream [working_branch]``` (from your master branch). 

If you are not in the susa-org organization, or you are just a general member **(1 or 3)**, your steps for contributing are:
1) Fork this repo
2) Clone your fork (www.github.com/youracct/crash-course)
3) Set your remotes

This means setting up your local version of the repo to be able to pull directly from the actual repo. This can be done by running the following command from within your local git repo:

```bash
git remote add upstream https://github.com/SUSA-org/crash-course.git
```

The link may be wrong, but otherwise it should work. Then, upon running
```bash
git remote -v
```

You should see "upstream https://...." listed.

4) Pull from the upstream, push to origin

```bash
git pull upstream BRANCH # BRANCH is usually master
git push # This will push to your local version, and will push to whatever branch you're working on
```

5) Create a pull request (go to your github account's fork, click pull request, compare forks)
