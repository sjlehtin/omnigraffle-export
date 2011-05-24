# OmniGraffle Export tool

A command line tool that allows to export one or more canvases from [OmniGraffle](http://www.omnigroup.com/products/omnigraffle/) into various formats using [OmniGraffle AppleScript interface](http://www.omnigroup.com/mailman/archive/omnigraffle-users/2008/004785.html).
It is compatible with [rubber](https://launchpad.net/rubber) and can be configured to automatically export figures out of .graffle file.


## Installation

In order to have it successfully installed and working, following is required:

* OmniGraffle 5
* python >= 2.6
* [appscript](http://appscript.sourceforge.net/py-appscript/index.html) >= 0.22

You can either clone the repository and use the setup tool:

    setup.py install
    
Or using the PIP:

    pip install omnigraffle_export

## Usage

	Usage: omnigraffle-export [options] <source> <target>
	
	Options:
	  -h, --help  show this help message and exit
	  -f FMT      format (one of: pdf, pnf, eps), defualt pdf
	  --force     force the export
	  -v          verbose
	  -c NAME     canvas name (if not given -t must point to a directory)

## LaTeX support via rubber

### Rubber

Rubber is a program whose purpose is to handle all tasks related to the compilation of LaTeX documents. This includes compiling the document itself, of course, enough times so that all references are defined, and running BibTeX to manage bibliographic references. Automatic execution of dvips to produce PostScript documents is also included, as well as usage of pdfLaTeX to produce PDF documents.

### Setup

Rubber uses `rules.ini` file to configure conversion options. In order to allow to use OmniGraffle Export, following needs to be added to the rules.ini (with the appropriate path alteration):

	[convert-omnigraffle]
	target = (.*):(.*)\.(eps|pdf)
	source = \1.graffle
	cost = 0
	rule = shell
	command = /opt/local/Library/Frameworks/Python.framework/Versions/2.6/bin/omnigraffle-export-rubber $target
	message = converting omigraffle $source into $target

This can be done automatically by executing the `setup-rubber.sh` script as root

### Usage

Following is an example how to include a canvas named `Canvas1` from file `figures/schema.graffle`:

	\begin{figure}
	  \centering
	  \includegraphics[width=\textwidth]{figures/schema:Canvas1.pdf}
	  \caption{Example of a OmniGraffle figure}
	\end{figure}

The notation is `<path_to_graffle_file_without_dot_graffle_suffix>:<canvas_name>`