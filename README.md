# Sanger.Deconvolution
Deconvolution of sanger chromatograms to quantify drug resistance alleles in mixed infections

R markdown scripts in support of publication:
Magia H, Morrison R, "Sanger sequencing and deconvolution of polyclonal infections: a quantitative 
approach to monitor drug resistant Plasmodium falciparum"  (eBioMedicine, in submission)

### Expected Inputs:

1. One or more sanger chromatogram .AB1 files

2. A named vector of 11aa character strings, defined explicitly in the body of the R markdown script, that defines
the mutation motifs of interest.

3. A character string of DNA, defined explicitly in the body of the R markdown script, that roughly spans 
the expected nucleotide region of the sanger chromatogram, in the coding strand reading frame, such that 
it correctly translates to the expected portion of the referenece protein.
  
### Running the Workflow:

Run the R markdown script (use `Sanger.Deconvolution.Rmd` for a single chromatogram; or `Batch.Sanger.Deconvolution.Rmd` 
for a folder of chromatograms) using Rstudio's `Run All` command.  Follow the prompts to select the inputs and 
define the protein of interest.

The script will generate an HTML file of results and images, and a CSV file of the same results in tabular form.
  
### Deconvolution Method

Each mutation motif site of interest will be deconvoluted by a 3-step process:  

1. Search for the 11aa mutation motif in all 6 possible reading frames of the chromatogram, to find the correct 
strand orientation and coding reading frame to align the chromatogram with the gene's protein coding sequence. 
This step will be tolerant of both chromatogram noise and field isolate variation, so rather than expecting a 
perfect 11aa match, a similarity scoring method is used to find the one best matching location in all 6 reading 
frames. If no high scoring match is found, the deconvolution quantification fails for this motif.
  
2. Isolate the center 3-nucleotide codon region of the motif, to extract the tiny chromatogram fragment corresponding 
to the single amino acid of interest. This now holds the raw trace matrix that describes the observed peak shapes and 
amplitudes in all 4 base call channels over the full period of 3 nucleotide peaks. As each peak shape is typically 
measured at about 10 data points per nucleotide, this tiny trace matrix will contain roughly 30 rows by 4 columns of 
raw intensity call values. This tiny extracted chromatogram trace matrix becomes the observed data for the final 
peak fitting step.
  
3. Perform the deconvolution model fitting. The observed tiny trace matrix is fitted by non-linear regression, using a simulated 
annealing algorithm (R package GenSA), as a weighted sum of all likely codons, where each possible codon is modeled 
as a tiny trace matrix of 3 gaussian peaks. The fitting process calculates the relative abundance of all model codon 
trace matrices that best fits the observed trace matrix. Lastly, each model codon of non-zero abundance is translated 
to its coding amino acid and proportionally summed, to give the final deconvolution result expressed as the relative 
proportions of one or more amino acids that best describe the observed chromatogram at that protein mutation site.

