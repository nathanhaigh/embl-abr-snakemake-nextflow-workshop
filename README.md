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
workshop. For the purpose of this example workflow we are going to use the [btp-workshop-ngs]
(https://github.com/BPA-CSIRO-Workshops/btp-workshop-ngs) repository.

To see a list of available BTP workshops head over to:
https://github.com/BPA-CSIRO-Workshops?query=btp-workshop-

  1. Clone the [btp-workshop-ngs](https://github.com/BPA-CSIRO-Workshops/btp-workshop-ngs)
  repository into a new repository on GitHub. We'll create a repository under our username called
  `ngs-workshop_SYD-2015-05` so we know exactly to which workshop the repository pertains:

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

