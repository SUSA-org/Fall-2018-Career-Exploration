# Fall 2018 Career Exploration Materials
Collection of materials to bring new members of SUSA @ Berkeley up to speed. Hosted by and maintained by the [Statistics Undergrad Student Association (SUSA)](https://susa.berkeley.edu).

# Contributing to This Repository

Assistance is welcome. In order to modify this repo, you first need to know your organization status. You can be
1) In the susa-org organization
2) In susa-org and a member of the crashcourse team w/i the org
3) Not in susa-org

#### If you are in the susa-org **and** in the crashcourse team **(2)**

Your steps for contributing are:
1) Clone this repo
```bash
# Navigate to your directory to store SUSA repositories.
git clone https://github.com/SUSA-org/crash-course.git
```

2) Create a new branch for your work
```bash
git checkout -b yourbranchnamehere
```

3) Upon making your edits, push onto working branch
```bash
git add changedfile1 [changedfile2 ...]
git commt -am "Description of change here"
git merge  
```

2)  directly pull/push to this repo

If you are having trouble pulling, pushing to the repo, then it may be because the upstreams are set to be the actual repo. To confirm this, you can run ``` git remote -v ``` and look for (upstream) listed somewhere. Then you just run ```git pull upstream [working_branch]``` and ```git push upstream [working_branch]``` (from your master branch). 

#### If you are not in the susa-org organization, or you are just a general member **(1 or 3)**

 Your steps for contributing are:
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
