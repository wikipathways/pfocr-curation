# pfocr-curation
A curation tool for fixing and annotating the data associated with Pathway Figure OCR results.

## Background
The Pathway Figure OCR project (PFOCR) aims to identify pathway figures from the published literature and extract biological meaning from them. Our pipeline has already screened over 300,000 figures published over the past 26 years and identified over 70,000 pathway figures. 1.4 million genes and over 100 thousand chemicals have been extracted from the figures using optical character recognition (OCR) and matched to proper database identifiers (i.e., HGNC and MESH). Read more about the project in [Hanspers et al. 2020](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-02181-2), and explore the database at https://gladstone-bioinformatics.shinyapps.io/shiny-25years/.

## Purpose
This repo contains a shiny app that you can run locally to help with curating and annotating the results of the PFOCR project. We extract figure titles and captions from PubMed Central and these are often not ideal (in length or content) as descriptors of the pathways. This tool allows you to rewrite the titles and captions. The relevant organism(s) represented by the pathway figure biology is not consistently supplied by the paper authors or jounral. So, this tool also allows you to annotated the figures with this information.

See our [curated organism list to-date](https://github.com/wikipathways/pfocr-curation/blob/main/curated_organism_list_todate.csv) for a sampling of the diverse species represented in the first set of 2,400 manually curated figures. There is lot of content here!

## New Curator Training
If this is your first time, go ahead and clone the repo and give the tool a try. By default, the tool is in [training mode](https://github.com/wikipathways/pfocr-curation/blob/main/app.R#L11).

#### Installation
 * Install R
 * Install R Studio
 * Clone this repo
 * Run the app

## How It Works
The tool will read in an RDS of figure metadata to be curated (e.g., pfocr_figures_curating.rds) and compare this against the figures already curated (e.g., pfocr_curated.rds). It will then present the next figure to be curated, displaying editable text field and some helpful button operations. The last of buttons will either save the curated fields, reload the original content, or go back to the previous if you have second thoughts.

![Screenshot](screenshot.png?raw=true "Screenshot")

**Curation buttons**
 * Remove Preamble -- "preamble" refers to commonly used phrases at the begining of figure titles that actually aren't at all descriptive and are just conventional English language kruft. The tool learns about these preambles as curation occurs (checking diffs along the way) and if a previously deleted reamble is found, the button will be active and a single click will remove it from the Figure title field. Simply "Reload" if you don't like the change.
 * Remove Word -- simply removes one word at a time from the front of the Figure title. Faster than using mouse and keyboard to highlight and delete.
 * Un-Greek -- Greek characters should not be retained in Figure titles as they can muck up later downstream bioinformatic analyses. This button will scan the Figure title text and replace Greek characters with substitutions, e.g., "alpha"
 * Capilatlize -- will capitalize the first character in the current Figure title. Useful after making other edits and then wanting to save. All titles should start with a capital letter.
 * Relace: Paper Title -- sometimes the Figure title is missing or woefully inadequate. This button will replace it with the Paper title, which sometimes is a perfect Figure title as well (but not always).
 * Replace: ... pathway -- sometimes the pathway title is buried in a bunch of text. This button will look for the pattern of "blahblah pathway" or "blahblah signaling pathway" and simply replace the Figure title field with just this bit.

**Decision buttons**
 * Save -- save the current fields to the local RDS and mark as curated.
 * Reload -- replace all fields with their original values. Kind of like a big undo.
 * Reject -- mark the figure as "not a pathway figure" and skip it. This will result in its removal from the PFOCR collection. Obviously we try to only capture pathway figures in the pipeline, but some others still sneak through. This is one way to mark those.
 * Non-human -- if you're sure the biology represented in the figure is not human, but you're not sure what it is (or can't be bothered at the moment), then use this button and it simply replaced the Organism text with "XXX" to be flagged for exclusion from the human collection and further curation later.
 * Undo -- whoops! You clicked save (or reject) too soon and are having second thoughts. Click this to go back to the previous figure. You will need to start over with the curation of that figure, but it's better than regretting your work.

## Coordinating Curation
Since the app runs locally, each curator will need to manage which sets of figures they are fixing in coordination with other curators. Otherwise, we will all be fixing the same ones over and over again! Please post an Issue in this repo and tag it as a question if you want to join. Curators can specify [assigned ranges](https://github.com/wikipathways/pfocr-curation/blob/main/app.R#L28) in the app to split up the work.
