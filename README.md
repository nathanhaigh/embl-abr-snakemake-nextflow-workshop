This repository provides a minimalistic base template for creating a hands-on workshop
following the framework set out in the Bioinformatics Training Platform (BTP). Below we have
provided various entry-points, by way of workflows, to demonstrate the various ways in which you
can utilise existing workshops and workshop modules. We also provide information on how to go about
developing your own modules and workshops.

Table of Contents
=================
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Prerequisites](#prerequisites)
- [General Workflows](#general-workflows)
  - [Resuing an Existing BTP Workshop](#resuing-an-existing-btp-workshop)
  - [Cutting Your Own BTP Workshop from Existing Modules](#cutting-your-own-btp-workshop-from-existing-modules)
  - [Cutting Your Own BTP Modules](#cutting-your-own-btp-modules)
  - [Cutting Your Own BTP Workshop from Scratch](#cutting-your-own-btp-workshop-from-scratch)
- [Advanced Workshop Customisations](#advanced-workshop-customisations)
  - [Minting a DOI for your Workshop](#minting-a-doi-for-your-workshop)
  - [Customise the Handout Styling](#customise-the-handout-styling)
  - [Use Travis-CI to automate PDF Building](#use-travis-ci-to-automate-pdf-building)
- [License](#license)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

Prerequisites
=============
We assume you are working on a Linux OS and have command line experience, or at least you're not
scared by it! The commands provided in the [General Workflows](#general-workflows) sections have
been written in bash for a 64-bit Ubuntu OS but should work on other Linux flavours with little
modification.

Here is a list of software prerequisites:

- [**hub**](https://hub.github.com/): This tool provides command line access to GitHub so you can do
    things like create and fork repositories on GitHub via the command line.

Installing Hub
--------------
Download the latest release from https://github.com/github/hub/releases/latest. As at this time,
the lastest version is v2.2.1 and we can download and install hub using the following commands:

```bash
# Set the version we want to install
HUB_VERSION='2.2.1'
cd /tmp
# Download
wget "https://github.com/github/hub/releases/download/v${HUB_VERSION}/hub-linux-amd64-${HUB_VERSION}.tar.gz"
# Extract
tar xzf "hub-linux-amd64-${HUB_VERSION}.tar.gz"
# Copy the binary onto the path
sudo cp "hub-linux-amd64-${HUB_VERSION}/hub" /usr/local/bin/
```

General Workflows
=================

Resuing an Existing BTP Workshop
--------------------------------
The easiest way to get started is to use an existing Bioinformatics Training Platform (BTP)
workshop. These workshops are like a master template for a given workshop; they are cloned
in order to run a new workshop of the same kind and are maintained and updated over time.

For the purpose of this example workflow we are going to use the [btp-workshop-ngs]
(https://github.com/BPA-CSIRO-Workshops/btp-workshop-ngs) repository. To see a list of available
BTP workshops head over to: https://github.com/BPA-CSIRO-Workshops?query=btp-workshop-

  1. Clone the [btp-workshop-ngs](https://github.com/BPA-CSIRO-Workshops/btp-workshop-ngs)
  repository into a new repository on GitHub. We'll create a repository under our username called
  `ngs-workshop_SYD-2015-05` so we know exactly to which workshop the repository pertains. The idea
  is that once the workshop has been run, this repository will remain unchanged. As such it will
  always provide a snapshot of what was covered during a particular workshop.

  ```bash
  NEW_REPO_NAME='nathanhaigh/ngs-workshop_SYD-2015-05'
  NEW_REPO_DESC='NGS Workshop: Sydney May 2015'
  
  cd /tmp/test
  git clone --recursive "https://github.com/BPA-CSIRO-Workshops/btp-workshop-ngs.git" ngs-workshop_SYD-2015-05
  cd ngs-workshop_SYD-2015-05
  
  hub create -d "${NEW_REPO_DESC}" "${NEW_REPO_NAME}"
  git remote set-url origin git@github.com:nathanhaigh/ngs-workshop_SYD-2015-05.git
  git push
  ```

  2. You should now customise the repository to reflect your workshop-specific details.
    1. `README.md` - Travis and zenodo badges, workshop info content
    2. `template.tex` - Modify `\setWorkshopTitle`, `\setWorkshopVenue`, `\setWorkshopDate` and
    `\setWorkshopAuthor` to reflext the specific of your workshop. These will be placed into the
    handout.
    3. `010_trainers/` - Delete unnecessary trainer photos from `010_trainers/photos/` and add
    photos of your own trainers instead. Modify `010_trainers/trainers.tex` to contain only yor own
    trainers and use the photos you placed into `010_trainers/photos/` or used the
    `010_trainers/generic.jpg` image for camera-shy trainers.
  3. Build your trainee and trainer handout PDFs.

```bash
# Perform a 1-time install of a minimal tex-live so you have everything you need to build the PDFs
# from the LaTeX source
cd ./developers/ && sudo -E ./texlive_install.sh && cd ../

# Build the trainee_handout.pdf and trainer_handout.pdf
PATH=/usr/local/texlive/bin/x86_64-linux:$PATH make
```

Developing Your Own BTP Workshop from Existing Modules
------------------------------------------------------
TODO

Developing Your Own BTP Modules
-------------------------------
TODO

Developing Your Own BTP Workshop from Scratch
---------------------------------------------
TODO

Updating an Existing Workshop
=============================
Once you have created your own workshop repository, either by [reusing an existing workshop](#resuing-an-existing-btp-workshop)
or by [developing your own BTP workshop from scratch](#developing-your-own-btp-workshop-from-scratch),
there may come a time when you want to run another workshop of the same type. We will be setting up
what GitHub calls the [fork & pull collaborative model](https://help.github.com/articles/using-pull-requests/#fork--pull).


For this purpose we
advise that you set up a repository which will act as the master template from which you will clone
other repositories for running another workshop. We recommend following the same steps for cloning a workshop
repository as detailed in [reusing an existing workshop](#resuing-an-existing-btp-workshop), except
when it comes to naming the repository, you omit the location and date information from the
repository name. This way you can easily distinguish your master workshop template repository,
from the workshop repositories linked to a particular workshop on a particular date in a particular
location.

The master template repository may require updating over time as you will be cloning from it for
running some future workshops. For the sake of simplisity, we assume you already have this
repository in place and we will use the [btp-workshop-ngs](https://github.com/BPA-CSIRO-Workshops/btp-workshop-ngs)
repository as our example.

Lock Down Write Access
----------------------
Firstly, this repository will be your master copy of the workshop and will be cloned whenever you
run a new workshop. You should provide a mechanism through which changes can be vetted before being
allowed into your master workshop repository. Luckily, GitHub allows these things to be done easily.

The first thing to do is to add GitHub users who you trust and want to allow write access to this
repository. Head over to the repository's Settings >> Collaborators page for this. Depending on
whether the repository is under your personal space or an organisation will change how you add
users with write permissions. For full details of both approaches, see the GitHub Help for [adding
collaborators to a personal repository](https://help.github.com/articles/adding-collaborators-to-a-personal-repository/)
or [permission levels for an organisation repository](https://help.github.com/articles/permission-levels-for-an-organization-repository/).

How To Provide Updates
----------------------
The GitHub way is to simply [fork the repository](https://help.github.com/articles/fork-a-repo/),
on GitHub. You will then work on this repository for making your changes. Once those changes have
been push up to the forked repository, you then need to
[issue a pull request](https://help.github.com/articles/using-pull-requests/#initiating-the-pull-request)
to get your modifications added into the original master workshop repository. Once the changes have
been accepted, you can delete your fork of that repository.

So, lets take this one step at a time and use the command line where possible rather than using the
GitHub website.

```bash
# Create a local clone of the repository we want to fork
cd /tmp
git clone --recursive https://github.com/BPA-CSIRO-Workshops/btp-workshop-ngs.git
cd btp-workshop-ngs

# Create the fork using hub
hub fork

# Make a change and commit it to your local repository
touch test
git add test
git commit -m "Added test file"

# Push it up to your repository on GitHub
git push --set-upstream <USER> master

# Subsequent pushes will only need a "git push"
touch test2
git add test2
git commit -m "Added test2 file"
git push

# Issue a pull request to have your changes included into the master workshop repository
hub pull-request -m "Test pull request"

# Now wait until someone with write access to the master workshop repository merges in
# your changes or otherwise provides a comment on your proposed changes
```

Advanced Workshop Customisations
================================
Once you've got the hang of making basic modifications of existing workshops, you'll find there are
other useful and interesting things you might like to customise. This section aims to guide you
though some of these.

Minting a DOI for your Workshop
-------------------------------
TODO

Customise the Handout Styling
-----------------------------
TODO

Use Travis-CI to automate PDF Building
--------------------------------------
TODO

License
=======
The contents of this repository are released under the Creative Commons
Attribution 3.0 Unported License. For a summary of what this means,
please see:
http://creativecommons.org/licenses/by/3.0/deed.en_GB

