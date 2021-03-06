# Description

This repository contains:
* Full phd report, written in Latex
* IPython notebooks, for reading data, processing it and saving it

Together, they make for a nice stress-free workflow.
IPython notebooks are particularly good at telling a story more than just storing code.
As such, they are particularly well suited to collaborate with a latex document.

# Python

You will need to install anaconda or miniconda.

Then, start the notebook explorer.

```
jupyter notebook
```

# Latex
## Prepare
Install Latex, then type in a terminal
```
pdflatex
```
to check latex properly installed.

Then, make sure you have npm installed and run

```
npm install -g grunt-cli
grunt latex
```

Also install [Perl interpreter](https://www.perl.org/get.html) (required for generating the glossary).
For code highlighting with `minted`, you will need Python 2.6+ and the `pygments` package installed.


## Generate

To let grunt monitor changes in files and regenerate the output, run
```
grunt
```

Run the `latex` task to generate the pdf file manually. You should run it twice to ensure that all references are properly defined (this is due to how Latex works and is not specific to this repo).
```
grunt latex
```

## Resources

Figures use the Roboto font. Available [here](https://fonts.google.com/specimen/Roboto).

* Latex quick reference: https://latex-project.org/guides/usrguide.tex
* Latex cheat sheet: https://wch.github.io/latexsheet/latexsheet.pdf
* Bibliography management: https://fr.sharelatex.com/learn/Bibliography_management_with_bibtex
* Interesting info about Grunt and automation: https://jonsuh.com/blog/take-grunt-to-the-next-level/

## Figures

In the current workflow, figures are drawn with inkscape in vector format.
Figures should preferably be imported from `.pdf` files to get the best render, rather than direct svg import which is broken at the moment.
To generate `.pdf` from `.svg`, the inkscape command line can be used.

With Inkscape, to get a clean output of the figure, it is required to set the page size and position properly before converting to `.pdf`.
Once a figure drawing is done:

1. Unselect everything
2. Go to `File`->`Document Properties`.
3. In the custom size tab, click on `resize page to content`.
4. Eventually set some margin, then click `resize page to drawing or selection`.

At this point the drawing should be properly surrounded by the page.

The `.svg` file can now be converted manually to `.pdf` with the following command.

```
inkscape --file=myImage.svg --export-pdf=image.pdf
```

To regenerate at once all `.pdf` from all `.svg`

```
node figs.js
```
or
```
grunt execute:figs
```
Grunt will watch for changes and new files and will regenerate automatically `.pdf` for modified files.

## Gotchas

When changing glossary related stuff, glossary generation can fail if the makeglossaries executable is not found.

## Cheat-sheet

### Cross references

For chapters, sections, declare a cross-reference like this (prefixing with `sec` is not mandatory, but a good practice).

```
\section{foo}
\label{sec:foo}
```

For bibliography, declare in `src/refs.bib`, then use the reference like this:

```
\cite{biblioEntryName}
```

You can reference multiple bibliography entries like this:

```
\cite{biblioEntry1, biblioEntry2}
```

### Figures

To leave figure as-is (works well with pdf)

```
\begin{figure}[!h]
  \centering
  \includegraphics{src/1/figures/iso10605_waveform.pdf}
  \caption{All waveforms defined in ISO 10605 standard}
  \label{iso_pulse}
\end{figure}
```

To clamp figure width to text width use `[width=\textwidth]`

```
\begin{figure}[!h]
  \centering
  \includegraphics[width=\textwidth]{src/1/figures/iso10605_waveform.pdf}
  \caption{All waveforms defined in ISO 10605 standard}
  \label{iso_pulse}
\end{figure}
```

To clamp width to text width with some ratio use `[width=0.9\textwidth]`

```
\begin{figure}[!h]
  \centering
  \includegraphics[width=0.9\textwidth]{src/1/figures/iso10605_waveform.pdf}
  \caption{All waveforms defined in ISO 10605 standard}
  \label{iso_pulse}
\end{figure}
```

Using the `[!h]` qualifier tells Latex to place the figure *right here*, as much as possible.
The `!` is enforcing further the placement rules given in the next table.

qualifier | name | comment
----------|------|--------
h | here | Place the figure where the `figure` environment was called (if the available page space permits it)
t | top | Place the figure at the top of a page
b | bottom | Place the figure at the bottom of a page
p | page |  Place the figure in its own page, separated from text

### Number and units

Instead of manually writing math symbols for units, a better approach is to use the [`siunitx`](https://www.ctan.org/pkg/siunitx) package.
The `\SI{value}{units}` command enables to write properly formatted values with units without having to bother about greek letters or formatting.

```
\SI{2}{\kilo\ohm} // 2 kΩ
\SI{15}{\pico\farad} // 15 pF
\SI{10}{\nano\second} // 10 ns
```

### Equations

```
\begin{equation}
a = b + c
\label{eq:foobar}
\end{equation}
```

* Resources for formatting ?

### Code snippets

Code files can be included inside the document with the following snippet

```
\begin{code}
\inputminted[frame=single]{VHDL}{path/to/code.c}
\caption{My foo code}
\label{lst:label}
\end{code}
```

### External tools
#### Viewing pdf files

For viewing the generated `phd.pdf`, consider using the amazing [Sumatra pdf reader](http://www.sumatrapdfreader.org/free-pdf-reader.html).
It is free, extremely fast, portable, small and is packed with good features.

#### Generating code for LaTeX tables using an online WYSIWYG editor

This [great little tool](http://www.tablesgenerator.com/) enables for quick code generation for latex tables. Doesn't work very well with large tables, doesn't support tabularx package.
This [one](http://truben.no/table/) exists too, but has less features (no support for merging cells for instance).

## Spell check

On Windows, run

```
spellcheck.cmd
```

On Unix systems, just run the commands listed in this file.

Also, the personal dictionary `personnal_dict_spellcheck` should be copied over your HOME directory if you don't want to redefine all false positives in this document.

# How did you do that ?

The document was scaffolded with the [Latex Yeoman generator](https://github.com/LeoColomb/generator-latex).

```
npm install -g yo
npm install -g generator-latex
yo latex
```

Then follow the instructions of the generator.
I have made some modifications to the `Gruntfile.js` for my own taste.
Feel free to do the same.
