This repository provides a minimalistic base template for creating a hands-on workshop
following the framework set out in the Bioinformatics Training Platform (BTP). It provides
the "glue" to make life easier for the workshop content developer. Below we have
provided various entry-points, by way of workflows. These demonstrate the various ways in which you
can reuse existing workshops and workshop modules as well how to go about developing your own
modules and workshops.

Table of Contents
=================
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [BTP Components](#btp-components)
  - [Workshop Modules](#workshop-modules)
  - [Workshops](#workshops)
- [What This Template Provides](#what-this-template-provides)
- [Workflow Prerequisites](#workflow-prerequisites)
- [General Workflows](#general-workflows)
  - [Workflow 1: Reusing an Existing BTP Workshop for Self-Directed Learning](#workflow-1-reusing-an-existing-btp-workshop-for-self-directed-learning)
  - [Workflow 2: Reusing an Existing BTP Workshop to Run Your Own Workshop](#workflow-2-reusing-an-existing-btp-workshop-to-run-your-own-workshop)
  - [Workflow 3: Using Existing BTP Modules to Develop Your Own Workshop](#workflow-3-using-existing-btp-modules-to-develop-your-own-workshop)
  - [Workflow 4: Making Changes to an Existing BTP Module or Workshop](#workflow-4-making-changes-to-an-existing-btp-module-or-workshop)
      - [Fork and Pull Collaborative Model](#fork-and-pull-collaborative-model)
      - [Updating a Module](#updating-a-module)
      - [Updating a Workshop](#updating-a-workshop) 
  - [Workflow 5: Developing Your Own BTP Module](#workflow-5-developing-your-own-btp-module)
    - [Specifying Datasets Required for Your Module](#specifying-datasets-required-for-your-module)
    - [Specifying Tools Required for Your Module](#specifying-toolsirequired-for-your-module)
    - [Writing Your Handout Exercises](#writing-your-handout-exercises)
  - [Workflow 6: Developing Your Own BTP Workshop From Scratch](#workflow-6-developing-your-own-btp-workshop-from-scratch)
- [Advanced Workshop Customisations](#advanced-workshop-customisations)
  - [Minting a DOI for your Workshop](#minting-a-doi-for-your-workshop)
  - [Customise the Handout Styling](#customise-the-handout-styling)
  - [Use Travis-CI to automate PDF Building](#use-travis-ci-to-automate-pdf-building)
- [License](#license)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

BTP Components
==============
In order to help create a more reusable, plug-and-play like system for developing workshops we have
developed two key components, [workshop modules](#workshop-modules) and [workshops](#workshops). We
maintain each in their own git repository so as to allow them to be reused and repurposed more easily.

We provide convienient template repositories for both a [module
repository](https://github.com/BPA-CSIRO-Workshops/btp-module-template) as well as this
[workshop repository](https://github.com/BPA-CSIRO-Workshops/btp-workshop-template). Typically, you
will only need to be familiar with these template repositories if you are planning to develop your
own workshop ([Workflow 3](#workflow-3-using-existing-btp-modules-to-develop-your-own-workshop)) or
your own workshop module ([Workflow 5](#workflow-5-developing-your-own-btp-module)).

which should be understood in order to get the most out of the BTP system.

<img src="http://s20.postimg.org/aeqre7ujx/Figure_1.png" width=400>

Workshop Modules
----------------
A workshop module is a major, self-contained component of a hands-on workshop. It can be reused,
in a mix-and-match way, along with other modules to put together a new workshop. Each module has
its own repository and contain all the required information for the teaching of that particular module's
content. They contain, LaTeX source code for the handout, presentation matrials to help introduce
concepts covered in the module as well as metadata describing the data and tools used in the
module excercies.

If you are looking to develop your own workshop content within the BTP framework then most of the work
is done at the module level in a module repository.

Workshops
---------
Workshop repositories pull together 1 or more workshop modules (as git `submodules`) and provide
the necessary files to "glue" them together into a single coherent workshop. We make use of two
types of workshops repositores:

  1. A master workshop repository. This is the workshop which is updated and maintained over time
     and is cloned whenever a trainer runs the workshop at a specific location on a particular date.
  2. A static, workshop repository, cloned from the above master workshop repository. It provides a
     convienient way to capture the state of a workshop at the time it was run. 

What This Template Provides
===========================

  * A barebones [`template.txt`](https://github.com/BPA-CSIRO-Workshops/btp-workshop-template/blob/master/template.tex) LaTeX file which provides the workshop-level information for the exercise handout. For instance, you can change the workshop title, venue, date and authors by some simple modifications of this file.
  * A `Makefile` which is used to build the workshop exercise handout document from all the component module LaTeX files.
  * Scripts for setting up a minimal TeX-Live installation capable of compiling the exercise handout from the LaTeX source.
  * Template files for creating a trainer information page and workshop preamble page in the exercise handout document
  * LaTeX style files for consistent rendering of the exercise handout across all your workshops.

How The Makefile Works
----------------------

The `Makefile` provides several convienient targets for installing a TeX-Live environment, to building both a trainer as well as a trainee version of the workshop exercise handout document.

Installing a minimal TeX-Live environment is as simple as:

```bash
make tex_env
```

We can build the trainer's version of the handout:

```bash
make trainer_handout.pdf
```

We can build the trainee's version of the handout:

```bash
make trainee_handout.pdf
```

For the handout building to work as expected, there are some simple rules to follow:

  * The order in which modules appear in the handout is dictated by the numerical prefix of the
    module's directory name. That means, when you add a workshop module using git `submodules`, use a relevant
    numerical prefix for the submodule name.
  * The LaTeX file containing a module's handhout exercises should be directly under the `handout` directory of the corresponding workshop module repository.

Workflow Prerequisites
======================
To follow along with the examples in the workflows, you'll need to install some software first.
Here is a list of software prerequisites:

- **git**: This tool allows you to use and interact with git repositories.
- [**hub**](https://hub.github.com/): This tool provides command line access to GitHub so you can do
    things like create and fork repositories on GitHub via the command line.

### Installing git
Simply install through the standard Ubuntu repositories:

```bash
apt-get install git -y
```

### Installing Hub
Download the latest release from https://github.com/github/hub/releases/latest. We can download and
install hub using the following commands:

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
The following workflows are to provide guidence on how to achieve particular tasks; from updating
a workshop module to writing your own workshop modules from scratch. There are some software
prerequisits for following the examples under each workflow.

We assume you are working on a Linux OS and have command line experience, or at least you're not
scared by it! The commands provided in these workflows have been written in bash for a 64-bit
Ubuntu OS but should work on other Linux flavours with little modification.

Workflow 1: Reusing an Existing BTP Workshop for Self-Directed Learning
-----------------------------------------------------------------------
A self-directed learner might like to work through the contents of an existing BTP workshop in their
own time and at their own pace.

Ready to roll VirtualBox and VMWare images have been create for the following BTP workshop repositories:

  1. **Intro to NGS** VM Images: [VirtualBox](http://example.com) | [VMWare](http://example.com)
  2. **Another Workshop** VM Images: [VirtualBox](http://example.com) | [VMWare](http://example.com)

These virtual machine image files were created using a process detailed in Revote *et al.* and consists
of the following steps:

  1. Install Packer (https://www.packer.io/)
  2. Install VirtualBox (https://www.virtualbox.org/)
  3. Clone the required BTP Workshop repository from GitHub
  4. Issue a Packer build process
  5. Boot a new VirtualBox virtual machine using the image file just created

<img src="http://s20.postimg.org/pxo7bc2ul/Figure_2.png" width="400">

Workflow 2: Reusing an Existing BTP Workshop to Run Your Own Workshop
---------------------------------------------------------------------
The easiest way to get started is to use an existing Bioinformatics Training Platform (BTP)
workshop. These workshops are like a master template for a given workshop; they are cloned
in order to run a new workshop of the same kind and are maintained and updated over time.

For the purpose of this example workflow we are going to use the [btp-workshop-ngs]
(https://github.com/BPA-CSIRO-Workshops/btp-workshop-ngs) repository. To see a list of available
BTP workshops head over to: https://github.com/BPA-CSIRO-Workshops?query=btp-workshop-

<img src="http://s20.postimg.org/b83d6u0kt/Figure_3.png">

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
    1. Modify `template.tex`. Change `\setWorkshopTitle`, `\setWorkshopVenue`, `\setWorkshopDate` and
    `\setWorkshopAuthor` to reflect the specifics of your workshop. These will be placed into the
    handout.
    2. Modify `010_trainers/` contents. Delete unnecessary trainer photos from `010_trainers/photos/` and add
    photos of your own trainers instead. Modify `010_trainers/trainers.tex` to contain only yor own
    trainers and use the photos you placed into `010_trainers/photos/` or used the
    `010_trainers/generic.jpg` image for camera-shy trainers.
  3. Build your trainee and trainer handout PDFs:

```bash
# Perform a 1-time install of a minimal tex-live so you have everything you need to build the PDFs
# from the LaTeX source
cd ./developers/ && sudo -E ./texlive_install.sh && cd ../

# Build the trainee_handout.pdf and trainer_handout.pdf
PATH=/usr/local/texlive/bin/x86_64-linux:$PATH make
```

Workflow 3: Using Existing BTP Modules to Develop Your Own Workshop
-------------------------------------------------------------------
You would like to create your own workshop by mixing-and-matching [existing BTP workshop modules](https://github.com/BPA-CSIRO-Workshops?query=btp-module-). We'll set up a master workshop
repository for this workshop, as we expect it will be in high demand, and show you how to clone it to
generate a statict workshop-specific repository for each of the workshops you run.

For this workflow, we will be creating an "Introduction to RNA-Seq" workshop, comprising an [NGS QC
module](https://github.com/BPA-CSIRO-Workshops/btp-module-ngs-qc) and an [RNA-Seq
module](https://github.com/BPA-CSIRO-Workshops/btp-module-rna-seq).

We will start with a clone of the 
[btp-workshop-template](https://github.com/BPA-CSIRO-Workshops/btp-workshop-template), as this provides us
with all the "glue" elements to bring our two workshop modules together. We then add our two workshop
modules as git submodules and then we can make customisations.

```bash
# Lets specify a name for our new workshop
WORKSHOP_NAME='my_new_workshop'

# Clone the workshop template as our starting place
git clone --recursive git@github.com:BPA-CSIRO-Workshops/btp-workshop-template.git "${WORKSHOP_NAME}"
cd "${WORKSHOP_NAME}"

# Add the two workshop modules as git submodules
# Use a number prefix to the submodules directories to indicate the order in workshop
# The QC module will come first, followed by the RNA-Seq module, as designated by the 020
# and 030 prefix. Note they come after 010_trainers and 020_preamble which are already
# provided by the workshop template.
git submodule add git@github.com:BPA-CSIRO-Workshops/btp-module-ngs-qc.git 020_qc
git commit -m "Added the QC module"
git submodule add git@github.com:BPA-CSIRO-Workshops/btp-module-rna-seq.git 030_rna-seq
git commit -m "Added the RNA-Seq module"

# Create the master workshop repository in your personal GitHub space using hub
hub create -d "A description of my new workshop"

# Update the origin remote of this local repository to point to our new master workshop
# repository on GitHub and push our new workshop to it. Change GITHUB_USER to your GitHub username
GITHUB_USER='my_github_username'
git remote set-url origin git@github.com:${GITHUB_USER}/${WORKSHOP_NAME}.git
git push
```

An alternative to starting from the [btp-workshop-template](https://github.com/BPA-CSIRO-Workshops/btp-workshop-template)
and then adding the modules you want, is to instead start from an existing [btp-workshop-](https://github.com/BPA-CSIRO-Workshops?query=btp-workshop-)
and then remove (or add) modules as you want. The removal of modules can be achieved
very easily:

```bash
# Lets specify a name for our new workshop
WORKSHOP_NAME='my_new_workshop'

# Clone the btp-workshop-ngs
git clone --recursive git@github.com:BPA-CSIRO-Workshops/btp-workshop-ngs.git "${WORKSHOP_NAME}"
cd "${WORKSHOP_NAME}"

# Delete all but the QC and RNA-Seq submodules
git rm 060_alignment
git commit -m "Deleted 060_alignment submodule"
git rm 070_chip-seq 090_velvet 905_post-workshop
git commit -m "Deleted other submodules"

# Create the master workshop repository in your personal GitHub space using hub
hub create -d "A description of my new workshop"

# Update the origin remote of this local repository to point to our new master workshop
# repository on GitHub and push our new workshop to it. Change GITHUB_USER to your GitHub username
GITHUB_USER='my_github_username'
git remote set-url origin git@github.com:${GITHUB_USER}/${WORKSHOP_NAME}.git
git push
```

Workflow 4: Making Changes to an Existing BTP Module or Workshop
----------------------------------------------------------------

### Fork and Pull Collaborative Model

We will assume that you are using a [fork & pull collaborative model](https://help.github.com/articles/using-pull-requests/#fork--pull)
to getting updates included into a workshop or workshop module. This means that the master
repository of a workshop or module has limited GitHub users which have write access and can thus
OK changes into a particular repository.

To modify who has write access to your GitHub repository, head over to the repository's
Settings >> Collaborators page. Whether the repository is under your personal space or an
organisation will determine exactly how youmake these changes. For full details of both
approaches, see the GitHub Help for
[adding collaborators to a personal repository](https://help.github.com/articles/adding-collaborators-to-a-personal-repository/)
or [permission levels for an organisation repository](https://help.github.com/articles/permission-levels-for-an-organization-repository/).

This provides a convienient way of controlling how changes are vetted before being included into a
module or workshop. Choose wisely which users you give this power to!

### Updating a Module

To demonstrate this workflow we will use the [btp-module-ngs-qc](https://github.com/BPA-CSIRO-Workshops/btp-module-ngs-qc)
repository as the example.

We won't make changes directly in the [btp-module-ngs-qc](https://github.com/BPA-CSIRO-Workshops/btp-module-ngs-qc)
repository because we either don't have permission or our policy doesn't allow it. Instead we will
fork the repository, make our changes there and then [issue a pull request](https://help.github.com/articles/using-pull-requests/#initiating-the-pull-request)

Lets take this one step at a time and use the command line where possible rather than using the
GitHub website:

```bash
# Create a local clone of the repository we want to fork
cd /tmp
git clone --recursive https://github.com/BPA-CSIRO-Workshops/btp-module-ngs-qc.git
cd btp-module-ngs-qc

# Create the fork using hub
hub fork

# Make a change and commit it to your local repository
touch test
git add test
git commit -m "Added test file"

# Push it up to your repository on GitHub - replace GITHUB_USER with your own GitHub username
GITHUB_USER='my_github_username'
git push --set-upstream ${GITHUB_USER} master

# Subsequent pushes will only need a "git push"
touch test2
git add test2
git commit -m "Added test2 file"
git push

# Issue a pull request to request your changes be included into the module repository
hub pull-request -m "Test pull request"
```

Now wait until someone with write access to the module repository has merged in your changes
or otherwise provided a comment on your proposed changes. Once a decision has been made regarding
your merge request, you can delete your fork of the repository. To do this, simply head over to the
repository's settings page. For full details, see GitHub's
[deleting a repository](https://help.github.com/articles/deleting-a-repository/) help page.

To have an existing workshop repository utilise these updates you will need to update the workshop
repository's git submodules. This is detailed in the [Updating a Workshop](#updating-a-workshop)
section.

### Updating a Workshop

A workshop is comprised of 1 or more modules which are included in the repository as git submodules.
A submodule always points to a particular revision of the module repository; usually the revision
when the module was added as a submodule to the workshop repsitory. As such, if a module gets
updated the submodule is still pointing to the same (older) revision of the module repository. We
need to update this if we want the workshop to use the new and improved updates that have been
included in the module.

We assume that some update(s) have been added to the [btp-module-ngs-qc](https://github.com/BPA-CSIRO-Workshops/btp-module-ngs-qc)
module and we now want to have those changes reflected in the [btp-workshop-ngs](https://github.com/BPA-CSIRO-Workshops/btp-workshop-ngs)
workshop repository.

Remember, we're not allowed to make changes directly in the master [btp-workshop-ngs](https://github.com/BPA-CSIRO-Workshops/btp-workshop-ngs)
repository. We either don't have permissions to do that ourself or we want someone else to check
what we're doing! As such, we'll fork the workshop repository and issue a pull request.

```bash
# Create a local clone of the repository we want to fork
cd /tmp
git clone --recursive https://github.com/BPA-CSIRO-Workshops/btp-workshop-ngs.git
cd btp-workshop-ngs

# Create the fork using hub
hub fork

# Update a git submodule so it points to the latest revision of the workshop module
cd 050_ngs-qc
git pull origin master
cd ../
git add 050_ngs-qc
git commit -m "Updated QC submodule to latest revision"

# Push it up to your repository on GitHub - replace GITHUB_USER with your own GitHub username
GITHUB_USER='my_github_username'
git push --set-upstream ${GITHUB_USER} master

# Update all submodules so they all point to the latest revisions in their
# respective repositories
git submodule update --remote
git add .
git commit -m "Updated all submodule to their latest revisions"

# Push it up to your repository on GitHub
git push

# Issue a pull request to request your changes be included into the module repository
hub pull-request -m "Updated all submodules to their latest revisions"
```

Workflow 5: Developing Your Own BTP Module
------------------------------------------
This is where you will spend most of your time in developing content for use in workshops. To be able to
effectively develop new content, you will need to become familiar with the way BTP modules are structured
and how they interface with a BTP workshop repository. All these details are provided in a template repository
([btp-module-template](https://github.com/BPA-CSIRO-Workshops/btp-module-template)) which contains detailed
information about the strucure of a BTP module, inline help and examples.

This workflow helps you to find information on how to specify what tools and data are needed for trainees
to follow along with your handout excersies and how to write your handout document using LaTeX. You will
then be able to follow [Workflow 3](#workflow-3-using-existing-btp-modules-to-develop-your-own-workshop)
for information on how to use/include your newly created BTP module as part of a workshop.

### Specifying Datasets Required for Your Module

You will need to create a `datasets/data.yaml` file in your workshop module. In this file, you need to
specify the public URL from which the dataset will be obtained. Other information specified in this YAML file
pertain to where the data set should reside on the training platform (this should match what you write in your
handout exercises), who should own the file(s) etc.

More specific information can be found in the [`btp-module-template/datasets/README.md`](https://github.com/BPA-CSIRO-Workshops/btp-module-template/datasets/README.md)

### Specifying Tools Required for Your Module

You will need to create a `tools/tools.yaml` file in your workshop module. In this file, you need to
specify the public URL of a shell script to be used to install each of the required tools. "Providers" other than `shell` can be specified. For instance, if the tool has a Debian package, you can provide a link to the `.deb` file
and specify the `provider` as `dpkg`.

More specific information can be found in the [`btp-module-template/tools/README.md`](https://github.com/BPA-CSIRO-Workshops/btp-module-template/tools/README.md)

### Writing Your Handout Exercises

TODO

Workflow 6: Developing Your Own BTP Workshop From Scratch
=========================================================

TODO

Advanced Workshop Customisations
================================
Once you've got the hang of making basic modifications of existing workshops, you'll find there are
other useful and interesting things you might like to customise. This section aims to guide you
though some of these.

Minting a DOI for your Workshop
-------------------------------
So, you've [developed your own BTP workshop](#developing-your-own-btp-workshop-from-scratch) and
you want to get credit for all your hard work or you simply want to have a public record of a
particular workshop so people can refer back to the content used. With a DOI, you can do just that.

In order to generate a DOI for your workshop repository, you first need to login to a service
external to GitHub, called [zenodo](https://zenodo.org/), and enable DOI generation for your
workshop repository. Then, when you create a new "release" of your repository, zenodo will store a
copy of the "release" for posterity and generate a DOI that will point to that copy held by zenodo.
This means that even in the event that GitHub goes away, there will be a copy of your repository as
it stood at the time you created the "release".

  1. Login to the [zenodo](https://zenodo.org/) website using your GitHub account details.
  2. Find the GitHub settings on zenodo; you should see a listing of all your GitHub repositories.
  3. Flick the switch to "ON" next to the repository for which you what zenodo to create DOI's
  whenever you make a "release" for that repository.
  4. Head back over to the repository on GitHub and click "releases" in the header of the repository.
  5. Click "Create a new release" and complete the form.
  6. Once the "release" is made, zenodo will detect this and do it's thing. Head back to the
  GitHub settings page on zenodo and you should find a DOI badge next to the workshop repository
  name containing the DOI. If you click the badge, you will be provided with code for inclusion in
  various file formats, including Markdown, HTML and a URL to an SVG image.

Full details of this process can be found on the [citable code](https://guides.github.com/activities/citable-code/)
GitHub help page.

Customise the Handout Styling
-----------------------------
There are several aspects of the theme which can be customised, some of which are straightforward
and some of which require in-depth knowledge of LaTeX macros and commands. The simplest aspects to
change are the icons placed in the left margin next to each of the paragraph text environments.
Simply clone the `btp-handout-style` repository, replace the relevant icon files in the `./icons`
directory and replace any existing style submodules, in your workshop repositories, with a link to
your cloned `style` repository. To change other aspects such as background colours of paragraph text
environments you need more in depth knowledge of LaTeX macros and commands. This requires that the
`btp.sty` style file be edited directly.

Use Travis-CI to automate PDF Building
--------------------------------------
IN PROGRESS

Ideally, everyone who contributes LaTeX code to your module will test if the PDF can still be built from the source
without error, before the change is comitted to the repository. This of course means that each collaborator has a
working TeX environment such as [TeX Live](http://www.tug.org/texlive/) (multi-platform),
[MiKTeX](http://www.miktex.org/) (MS Windows), [proTeXt](http://www.tug.org/protext/) (MS Windows) or
[MacTeX](http://www.tug.org/mactex/) (Apple Mac). However, what if edits are performed online, the collaborator
couldn't get a TeX environment installed easily simply forgets to test the build before committing their changes? As a
result, you can find yourself with a LaTeX project which no longer compiles and debugging becomes time-consuming.

The Travis Continuous Integration ([Travis-CI](https://travis-ci.org/)) system can be instructed to perform almost any automated task following a commit being pushed to a GitHub repository. By utilising Travis-CI we can:

  1. Automate the PDF build process, thereby removing the requirement for all collaborators to have a working TeX
environment and to check the building before pushing changes.
  2. We are able to catch LaTeX errors early while they are easier to debug.
  3. We can publish the resulting PDF to the repository's [GitHub Pages](https://pages.github.com/).

### Configuring Travis-CI 
All that is required is a a bit of configuration to get Travis-CI and your GitHub repository talking and a special
`.travis.yml` file in the top level of your repository. This file is where you place instructions for Travis-CI to
perform. To enable Travis-CI to automate tasks associated with your GitHub-based LaTeX repository (I assume this already exists) we need to set up a few things first (full details at http://docs.travis-ci.com/user/getting-started/). These are:

  1. Sign in to [Travis-CI](https://travis-ci.org/) using your GitHub account credentials and OK the permissions
required by Travis-CI to access your GitHub account.
  2. Enable your LaTeX project repository on your Travis-CI [profile page](https://travis-ci.org/profile). Once this
is done, Travis-CI is then monitoring the repository for any commit activity.
  3. Create a `.travis.yml` file in the root of your GitHub repository defining the tasks you want Travis-CI to
perform each time a commit is made to your monitored repository.

### Add a `.travis.yml` File to Your Repository
Once you've enabled Travis-CI for your LaTeX repository, it's time to create that `.travis.yml` file which will tell
Travis-CI what automated tasks to perform each time a commit is pushed to your repository. First, a little background:

When Travis-CI detects a commit to your repository it first create a brand new, clean (vanilla) virtual machine (VM)
called the [build environment](http://docs.travis-ci.com/user/ci-environment/) (by default: Ubuntu 12.04 LTS Server
Edition 64bit). Next, Travis-CI clones your repository within that build environment and then runs the tasks defined
in the `.travis.yml` file in the top level of your repository. Now, what do we need to add to the `.travis.yml` file to have Travis-CI perform:

  1. Install TeX Live
  2. Compile our LaTeX document

License
=======
The contents of this repository are released under the Creative Commons
Attribution 3.0 Unported License. For a summary of what this means,
please see:
http://creativecommons.org/licenses/by/3.0/deed.en_GB

