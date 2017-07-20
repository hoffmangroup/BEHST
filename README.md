# BEHST - Biological Enrichment of Hidden Sequence Targets #

BEHST: an advanced tool for gene set enrichment analysis (GSEA) enhanced
through integration of chromatin long-range interactions

## Summary ##
BEHST reads an input dataset of chromosome regions, and intersects them with the chromatin interactions available in the Hi-C dataset. Of these chromosome regions, BEHST selects those that are presentthe regulatory regions of genes of APPRIS, a dataset of principal isoform annotations. We defined these cis-regulatory regions upon the position of their nearest transcription start site of the APPRIS genes' principal transcripts (obtained through GENCODE), plus an upstream and downstream extension. Afterwards, BEHST takes the genes of the resulting partner loci found in gene regulatory regions, and performs a gene set enrichment analysis on them through g:Profiler. BEHST, finally, outputs the list of the most significant Gene Ontology terms detected by g:Profiler.

## Installation ##
To run BEHST, you need to have the following programs and packages installed in your machine:

* **Python** (version 2.7.5)
* Python **Pandas** package
* **Bedtools** (version v2.25.0)
* Python **PyBedtools** package
* **R** (version 3.3.2)
* R **gProfileR** package

If you already have all of them installed on your computer, you can download and run BEHST (without root privileges), following the **Execution instructions** below.

On the contrary, in case you needed to install some of these programs, you need to have root privileges, an internet connection, and at least 8 GB of free space on your hard disk.

We here provide the instructions to install all the needed programs and dependencies on Linux CentOS, Linux Ubuntu, and Mac OS. BEHST was originally developed on a Linux CentOS computer.
 
#### Instructions for Linux CentOS ####
Here are the instructions to install all the programs and libraries needed by BEHST on a Linux CentOS computer, from a shell terminal. We tested these instructions on a Dell Latitude 3540 laptop running Linux CentOS 7.2.1511 operating system, in February 2017. If you are using another operating system version, some instructions might be slightly different.

(Optional) First of all, update your system:

`sudo yum -y update`

Install Python, and Python Pandas package:

`sudo yum -y install python`

`sudo yum -y install python-devel`

`sudo yum -y install epel-release`

`sudo yum -y install python-pip`

`sudo pip install pandas`

Install the development tools, such as gcc:

`sudo yum -y group install "Development Tools"`

`sudo yum -y install zlib-devel`

Install Bedtools:

`sudo yum -y install BEDTools`

`sudo pip install pybedtools`

Install R and its packages RCurl and gProfileR:

`sudo yum -y install R`

`sudo yum -y install curl-devel`

`sudo Rscript -e 'install.packages(c("RCurl","gProfileR"), repos="https://cran.rstudio.com")' `

#### Instructions for Linux Ubuntu ####
Here are the instructions to install all the programs and libraries needed by BEHST on a Linux Ubuntu computer, from a shell terminal. We tested these instructions on a Dell Latitude 3540 laptop running Linux Ubuntu 16.10 operating system, in February 2017. If you are using another operating system version, some instructions might be slightly different.


First of all, update your system:

(Optional) `sudo apt-get -y update`

Install Python, and Python Pandas package:

`sudo apt-get -y install python`

`sudo apt-get -y install python-pip`

`sudo pip install pandas`

Install the development tools, such as gcc:

`sudo apt-get -y install build-essential`

`sudo apt-get -y install libpng-dev`

`sudo apt-get -y install zlib1g-dev`

Install Bedtools:

`sudo apt-get -y install bedtools`

`sudo pip install pybedtools`

Install R and its packages RCurl and gProfileR:

`sudo apt-get -y install r-base`

`sudo apt-get -y install curl`

`sudo apt-get -y install libcurl3`

`sudo apt-get -y install libcurl4-gnutls-dev`

`sudo Rscript -e 'install.packages(c("RCurl", "gProfileR"), repos="https://cran.rstudio.com")' `

#### Instructions for Mac OS ####
Here are the instructions to install all the programs and libraries needed by BEHST on a Mac computer, from a shell terminal. We tested these instructions on an Apple computer running a Mac OS macOS 10.12.2 Sierra operating system, in March 2017. If you are using another operating system version, some instructions might be slightly different.

First of all, update:

(Optional) `sudo softwareupdate -iva`

Install rudix:
`curl -O https://raw.githubusercontent.com/rudix-mac/rpm/2016.12.13/rudix.py`

`sudo python rudix.py install rudix`

`sudo rudix install coreutils`

Install the development tools, such as gcc:

`xcode-select --install`

Install Python Pandas: 

`sudo easy_install pandas`

Install homebrew:

`ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

Install Bedtools:

`brew install homebrew/science/bedtools`

`sudo easy_install pybedtools`

Install R and its packages RCurl and gProfileR:

`brew install r`

`sudo Rscript -e 'install.packages(c("RCurl", "gProfileR"), repos="https://cran.rstudio.com")' `

Since the sed command has a different meaning from Linux to Mac, we have to replace sed with gsed in the main project.sh file:

`brew install gnu-sed`

`cd /behst/bin/`

`gsed -i.bak 's/sed/gsed/g' project.sh`

## Execution instructions ##
To run best, move to the /behst/bin/ folder first: 

`cd /behst/bin/`

Then you have to download the default data files for BEHST. They are genomic regions files containing enhancers of FANTOM5 and VISTA which can be the input of your test, and the files of the data you need to run BEHST: a GENCODE annotation file, a principal transcript APPRIS file, and a Hi-C long-range interaction file.

To download the data files, you have to run the `./download_behst_data.sh` by providing the full path of there folder where you want to download the data files. For example:

`./download_behst_data.sh ~/myBEHSTdataFolder`

This command downloads all the default data files of BEHST into the `~/myBEHSTdataFolder/` folder in your computer.
After having downloaded all the default data files, you can use BEHST by calling the `./behst.py` script providing two mandatory parameters (an input .bed file of genomic regions on which to apply BEHST, and the default data folder full path), and multiple optional parameters.
With the optional parameters, you can specifify alternative values for the query extension (`-Q`) and the target extension (`-T`), and alternative files for the gene annotations (`g`), for the principal transcript file (`-t`), and for the long-range interaction file (`-i`).

For example, to apply BEHST to the FANTOM5 lung enhancers by using the optimized hyper-parameter values QUERY = 24100 and TSS extension = 9400, the default GENCODE annotations (previously downloaded in the `~/myBEHSTdataFolder/` folder), the default APPRIS transcripts (previously downloaded in the `~/myBEHSTdataFolder/` folder), and the default Hi-C long range interactions (previously downloaded in the `~/myBEHSTdataFolder/` folder), run the following commands:

`./behst.py ~/myBEHSTdataFolder/pressto_LUNG_enhancers.bed ~/myBEHSTdataFolder`

The user can decide to use an alternative gene annotation file, an alternative transcript file, and an alternative chromatin loopings file, by specifying them as arguments to the `project.sh` script. The user can read the help by typing:

`usage: behst.py [-h] [-T TARGET_EXTENSION] [-Q QUERY_EXTENSION]`
`                        [-g GENE_ANNOTATION_FILE] [-t TRANSCRIPT_FILE]`
`                        [-i INTERACTION_FILE] [-v]`
`                        INPUT_BED_FILE BEHST_DATA_FILES_FOLDER`

> positional arguments:
>  INPUT_BED_FILE        input BED file of genomic regions
>  BEHST_DATA_FILES_FOLDER
>                        path to the folder where you downloaded the default
>                        BEHST data files with ./download_behst_data.sh
>
> optional arguments:
>  -h, --help            show this help message and exit
>  -T TARGET_EXTENSION, --target-extension TARGET_EXTENSION
>                        target extension basepair integer. Default is 9400.
>  -Q QUERY_EXTENSION, --query-extension QUERY_EXTENSION
>                        query extension basepair integer. Default is 24100.
>  -g GENE_ANNOTATION_FILE, --gene-annotation-file GENE_ANNOTATION_FILE
>                        path of the gene annotation file (.gtf format).
>                        Default is the GENCODE annotation v.19 file
>                        (gencode.v19.annotation_withproteinids.gtf).
>  -t TRANSCRIPT_FILE, --transcript-file TRANSCRIPT_FILE
>                        path to the principal transcript file (.bed format).
>                        Default is APPRIS transcript 2017_01.v20 file
>                        (appris_data_principal.bed)
>  -i INTERACTION_FILE, --interaction-file INTERACTION_FILE
>                        path to the chromatin interactions file (.hiccups
>                        format). Default is the Hi-C HiCCUPS from Lieberman-
>                        Aiden 2014 (hic_8celltypes.hiccups).
>  -v, --version         current BEHST version

>Citation: Chicco D, Bi HS, Reimand J, Hoffman MM. 2017. "BEHST: Genomic set enrichment analysis enhanced through integration of chromatin long-range interactions". In preparation.




## License ##
All the code is licensed under the [GNU General Public License, version 2 (GPLv2)](http://www.gnu.org/licenses/gpl-2.0-standalone.html).

The file of the Hi-C dataset `\data\hic_allCellTypes` was downloaded from the National Center for Biotechnology Information (NCBI) [Gene Expression Omnibus (GEO) website](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE63525) and is available under the [GEO copyright license](https://www.ncbi.nlm.nih.gov/geo/info/disclaimer.html).

The file of the APPRIS dataset `\data\appris_data_principal.txt` was downloaded from the [APPRIS website](http://appris.bioinfo.cnio.es/#/downloads) and is available under the [Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International (CC BY-NC-ND 4.0)](https://creativecommons.org/licenses/by-nc-nd/4.0/).

The file of the GENCODE dataset `\data\gencode.v19.annotation.gtf_withproteinids` was downloaded from the [GENCODE website](http://appris.bioinfo.cnio.es/#/downloads) and is available under the [Creative Commons
Attribution-NonCommercial-NoDerivs 2.5 Generic (CC BY-NC-ND 2.5)](https://creativecommons.org/licenses/by-nc-nd/2.5/).

The files of the VISTA dataset `\data\*vista*` are publically available on the [VISTA Enhancer Browser website](https://enhancer.lbl.gov/cgi-bin/imagedb3.pl?form=search&show=1&search.form=no&search.result=yes).

The files of the PrESSTo dataset `\data\*pressto*` are publically available on the [Promoter Enhancer Slider Selector Tool (PrESSto) website](http://enhancer.binf.ku.dk/presets/#download_view_div).

## Contacts ##

BEHST was developed by Davide Chicco, Haixin Sarah Bi, and Michael M. Hoffman at the [Hoffman Lab](http://www.hoffmanlab.org) of the [Princess Margaret Cancer Centre](http://www.uhn.ca/PrincessMargaret/Research/) (Toronto, Ontario, Canada).

For code questions, write to Davide Chicco: davide.chicco(AT)davidechicco.it

For scientific questions, write to Michael M. Hoffman: michael.hoffman(AT)utoronto.ca