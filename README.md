# Create vector graphic formulae for large print bills

I have found two ways to create vector graphic formulae for large print bills. The first way is to use [LaTeX](https://www.latex-project.org/). The second way is to use the [MathJax](https://www.mathjax.org/) library. I think LaTeX might be best as the method below outpus the formula in a Sans Serif and at 20pt in size. Both methods are described below.

For both methods you must first extract each formula as Math ML from the LawMaker XML. See the example file, `example_mathML_from_LM_2023_HCB13_Leasehold.xml`. This was extracted from the [LawMaker XML](https://publications.parliament.uk/pa/bills/cbill/58-04/0013/230013.xml) from the [Leasehold and Freehold Reform Bill](https://bills.parliament.uk/bills/3523/publications)

## MathJax

MathJax is a JavaScript library that allows you to include mathematical formulae in your web pages but it can also be used to create an SVG of the formula.

### MathJax Installation

Make sure you have [Node.js](https://nodejs.org/en/) installed.
The file `mml2svg.js` is a node script that converts MathML to SVG. It's copied
from the [MathJax Github](https://github.com/mathjax/MathJax-demos-node/blob/master/component/mml2svg). *Optionally*, download the most up to date version from the original github repository using the following command:

```bash
curl -O https://raw.githubusercontent.com/mathjax/MathJax-demos-node/master/component/mml2svg.js
```

Then install MathJax using the following command (in powershell if you are using Windows or in a terminal if you are using Mac or Linux):

```bash
npm install mathjax-full@3 --save-dev esm --save-dev yargs
```

### Use MathJax to create an SVG of the formula

If you are using Mac or Linux use the following bash command in a terminal:

```bash
node -r esm mml2svg.js $( tr -d "\n\r" <  example_mathML_from_LM_2023_HCB13_Leasehold.xml ) > formula.svg
```

If you are using Windows use the following command in powershell:

```powershell
```

## LaTeX
LaTeX is a document preparation system with excellent support for mathematical formulae.

Using this method, first we transform the MathML to LaTeX using [XSLT](https://www.w3.org/Style/XSL/). Then we use [LaTeX](https://www.latex-project.org/) to create a PDF.

### Installation

#### XSLT

Make sure you have [Saxon](http://saxon.sourceforge.net/) installed. I used the [Homebrew](https://brew.sh/) package manager to install Saxon on Mac.

On windows I used installed the .Net version of Saxon from [Saxon dotnet](https://www.saxonica.com/download/dotnet.xml). To check that it is installed correctly use the following command:

```powershell
C:\path\to\saxonCS.exe version
```

Replace `path\to` with the path to the directory where you installed Saxon. This should output something like the following:

`SAXON-CS-EE 11.6 from Saconica`


#### LaTeX

Make sure you have [LaTeX](https://www.latex-project.org/get/) installed. There are several distributions of LaTeX to choose from. I used [TexLive](https://www.tug.org/texlive/), but this is many Gigabytes in size so you may want to consider [MikTex](https://miktex.org/) which is a much smaller install.

To check that it is installed correctly use the following command:

```bash
pdflatex --version
```
You should then see some version info printed out.

### Usage

Transfrom the MathML file to a LaTeX file using a command like following command:

```bash
saxon -s:example_mathML_from_LM_2023_HCB13_Leasehold.xml -xsl:pmml2tex.xsl -o:example.tex
```
Note: the above will transform the example file `example_mathML_from_LM_2023_HCB13_Leasehold.xml` to a LaTeX file called ``example.tex``. If you want to transform a different file replace `example_mathML_from_LM_2023_HCB13_Leasehold.xml` with the path to the file you want to transform.

You should see a new file called `example.tex`

next create a PDF using the following command:

```bash
pdflatex example.tex
```

You should now see a new file called `example.pdf`. You can insert this file into the Large print Bill.
