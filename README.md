# Sanger.Deconvolution
Deconvolution of sanger chromatograms to quantify drug resistance alleles in mixed infections

R markdown scripts in support of publication:
Magia H, Morrison R, "Sanger sequencing and deconvolution of polyclonal infections: a quantitative 
approach to monitor drug resistant Plasmodium falciparum"  (eBioMedicine, in submission)

### Expected Inputs:

1. One or more sanger chromatogram .AB1 files

2. A named vector of 11aa character strings, defined explicitly in the body of the R markdown script, that defines
the mutations of interest.

3. A character string of DNA, defined explicitly in the body of the R markdown script, that roughly spans 
the expected nucleotide region of the sanger chromatogram, in the coding strand reading frame, such that 
it correctly translates to the expected portion of the referenece protein.
  
### Running the Workflow:

Run the R markdown script (use `Sanger.Deconvolution.Rmd` for a single chromatogram; or `Batch.Sanger.Deconvolution.Rmd` 
for a folder of chromatograms) using Rstudio's `Run All` command.  Follow the prompts to select the inputs and 
define the protein of interest.

The script will generate an HTML file of results and images, and a CSV file of the same results in tabular form.

