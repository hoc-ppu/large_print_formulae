# Create vector graphic formulae for large print bills

I have found two ways to create vector graphic formulae for large print bills. The first way is to use [LaTeX](https://www.latex-project.org/). The second way is to use the [MathJax](https://www.mathjax.org/) library. I think the LaTeX method might be better as it outpus the formula in a font that is closer to aria and at 20pt in size, but the MathJax method is probably a bit easier. Both methods are described below.

For both methods you must first extract each formula as Math ML from the LawMaker XML. See the example file, `example_mathML_from_LM_2023_HCB13_Leasehold.xml`. This was extracted from the [LawMaker XML](https://publications.parliament.uk/pa/bills/cbill/58-04/0013/230013.xml) from the [Leasehold and Freehold Reform Bill](https://bills.parliament.uk/bills/3523/publications)

## MathJax

MathJax is a JavaScript library that allows you to include mathematical formulae in your web pages but it can also be used to create an SVG of the formula.

### MathJax Installation

Make sure you have [Node.js](https://nodejs.org/en/) installed.

Then install MathJax using the following command (in powershell if you are using Windows or in a terminal if you are using Mac or Linux):

```bash
npm install mathjax-full@3 --save-dev esm --save-dev yargs
```

### Use MathJax to create an SVG of the formula

Run the following command to create an SVG of the formula (in powershell or terminal):

```bash
node -r esm mml2svg.js example_mathML_from_LM_2023_HCB13_Leasehold.xml > formula.svg
```

## LaTeX
LaTeX is a document preparation system with excellent support for mathematical formulae.

Using this method, first we transform the MathML to LaTeX using [XSLT](https://www.w3.org/Style/XSL/). Then we use [LaTeX](https://www.latex-project.org/) to create a PDF.

### Installation

#### XSLT

If you do not already have an XSLT processor installed, you will need to install one. I used [Saxon](https://www.saxonica.com/download/download_page.xml).

There are a few different versions of Saxon to choose from. See the list below. You only need to use one method:

##### Install Saxon method 1 Node.js (Mac, Linux, Windows)

If you have [Node.js](https://nodejs.org/en/) installed, run the following (in powershell if you are using Windows or in a terminal if you are using Mac or Linux):

```bash
npm install -g saxon-js xslt3
```

Test that it is installed correctly by running the following command  (in powershell or terminal):

```bash
xslt3 -?
```

If saxon has been installed correctly you should see some help text printed out.

##### Install Saxon method 2 Homebrew (Mac)

On Mac you can install saxon via the [Homebrew](https://brew.sh/) package manager. Follow the homebrew install instructions and then run `brew install saxon` in a terminal. To check that it is installed correctly run `saxon -versionmsg`, you some help text printed out.

##### Install Saxon method 3 .Net version (Windows)

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

Transfrom the MathML (with pmml2tex.xsl) file to a LaTeX file using either your favourite method, or if you installed saxon (see above) with a command like one of the following command:

**On Mac or Linux**
```bash
saxon -s:example_mathML_from_LM_2023_HCB13_Leasehold.xml -xsl:pmml2tex.xsl -o:example.tex
```

**On Windows**
```powershell
saxon "-s:example_mathML_from_LM_2023_HCB13_Leasehold.xml" "-xsl:pmml2tex.xsl" "-o:example.tex"
```

Note: the above will transform the example file `example_mathML_from_LM_2023_HCB13_Leasehold.xml` to a LaTeX file called `example.tex`. If you want to transform a different file replace `example_mathML_from_LM_2023_HCB13_Leasehold.xml` with the path to the file you want to transform.

You should see a new file called `example.tex`

next create a PDF using the following command:

```bash
pdflatex example.tex
```

You should now see a new file called `example.pdf`. You can insert this file into the large print bill in InDesign.
