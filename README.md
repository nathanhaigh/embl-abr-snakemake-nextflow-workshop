[![Build Status](https://travis-ci.org/BPA-CSIRO-Workshops/btp-workshop-template.png?branch=master)](https://travis-ci.org/BPA-CSIRO-Workshops/btp-workshop-template)

This repository provides a base template for creating a hands-on workshop module for use in the
Bioinformatics Training Platform (BTP). These types of workshop modules are designed to be self
contained. That is they contain all the required information to permit them to ne reused quickly
and easily. They contain the following 4 major elements:

 * Handout documentation written in LaTeX. These often included background information,
   step-by-step excersises, questions and answers as well as bonus excecises for those who progress
   rapidly.
 * Metadata describing where the data sets, used in the above handout, can be retrieved and where
   on the computer system they should be located.
 * Metadata describing which tools are required for the above handout excercises, and how to
   install them.
 * Presentations for introducing concepts which will be explored in the handout excercises.

You will find instructions below on how to use this template to build your own hands-on workshop
module.




Table of Contents
=================
<!-- toc -->
* [Homepage and PDFs](#homepage-and-pdfs)
* [Purpose](#purpose)
* [Building PDFs from LaTeX Source](#building-pdfs-from-latex-source)
* [Credits](#credits)
* [License](#license)

<!-- toc stop -->
Homepage and PDFs
=================
You can access the homepage, and up-to-date PDFs, for this project at
http://bpa-csiro-workshops.github.io/ngs-handout/

Purpose
=======
This repository contains style information for use with LaTeX to generate
consistently styled handout documents for use in hands-on bioinformatics
training workshops.

You can find full details of what this repository is all about how to use it at:
http://BPA-CSIRO-Workshops.github.io/handout-template/

Building PDFs from LaTeX Source
===============================
First you need to install texlive and a bunch of LaTeX packages. To make this
easy we have provided a script to do this:
```bash
cd developers
sudo ./texlive_install.sh
cd ../
```

Secondly, you need to add the location of the texlive binary's to your PATH. If
you installed texlive using the supplied script, the following should work:
```bash
export PATH=/usr/local/texlive/2013/bin/x86_64-linux:$PATH
```

Now you're ready to build the PDFs using the supplied Makefile:
```bash
make
```

Credits
=======
This style was originally designed for the Next Generation Sequencing (NGS)
hands-on workshop developed by Bioplatforms Australia (BPA), CSIRO
Bioinformatics Core and the EBI. It was written predominantly by Nathan S.
Watson-Haigh.

License
=======
The contents of this repository are released under the Creative Commons
Attribution 3.0 Unported License. For a summary of what this means,
please see:
http://creativecommons.org/licenses/by/3.0/deed.en_GB

