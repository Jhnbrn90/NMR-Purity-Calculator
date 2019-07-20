# NMRPurityCalculator
A chemistry tool for calculating impurities within NMR spectra

## Introduction
Determining the actual yield from an NMR spectrum is tedious and can be automated. 
Therefore, I created this tool which allows chemists - like myself - to analyze their purity of NMR spectra fast and easy.

The website provides a collection of preselected impurities, but is flexible to allow for custom impurities like starting materials.

## Basic Usage

1. The first step in using this tool is to be sure 1 product proton (H) in the NMR spectrum correspond to an integral of 1.00.

1. Next, on the website the user supplies the product mass and a question will appear asking for any impurities.

1. Add impurities, the modal will present the user with a drop-down menu of options to choose from preselected compounds.
The user is also prompted to supply the integral of the found impurity. 

1. Lastly, the product and impurities are displayed together with their corresponding mass- and mol%.

## How it's made
This project is made with VueJS and the Vue-CLI tool, available from https://vuejs.org/.

## Demo
A running demo can be found at https://www.nmrpurity.com
