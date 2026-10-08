# Rice gene and protein ID matching

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/arif202536599-cloud/gene_ids_matching/blob/main/Rice_TBtools_Gene_Matcher.ipynb)

Match a TBtools export to an MSU release 7 japonica rice GFF3 and protein FASTA. The notebook follows exact model IDs and GFF3 Parent links, calculates sequence properties, and retrieves gene symbols from the official Oryzabase gene list.

## Run in Google Colab

1. Click **Open in Colab** above. A CPU runtime is sufficient.
2. Run the cells from top to bottom.
3. Upload one file at each prompt:
   - **Selection file:** `Final_matched_genes.txt`, an ID list, a headered CSV/TSV, or protein FASTA
   - **Original annotation:** `osa1_r7.all_models.gff3` or `.gff3.gz`
   - **Protein FASTA:** `final_protein_ids.txt` with FASTA `>` headers
4. Review the match summary and any reported conflicts.
5. Download the ZIP produced by the last cell.

Use the same organism and annotation release for all inputs. Coordinate exports without headers begin with chromosome, protein/model ID, start, end, and strand. The reader also detects one MSU ID per line, FASTA headers, and CSV/TSV files with a named model/gene ID column. When coordinates are absent, they are looked up in the GFF3. Gene-level IDs without coordinates expand to every annotated isoform; requested IDs are recorded in the audit. Input order is retained.

## Results

- Gene and protein/model IDs, common names, chromosome and strand
- Model coordinates, model size, parent gene size and summed CDS size
- Protein length, theoretical molecular weight in kDa and pI
- Matching audit with gene coordinates, full names, descriptions, ambiguous names and conflicts
- Duplicate FASTA report
- Styled HTML/PDF table and an Excel-readable CSV
- Protein features for a future labelled machine-learning study

Missing or conflicting values are marked unavailable rather than guessed. Common names are looked up in curated annotation. This workflow does **not** train an ML model. ML prediction cannot verify official gene symbols or exact genomic coordinates from an unlabelled protein list.

## Definitions

**Model size** and **gene size** are inclusive genomic spans (`end - start + 1`) and include introns. **CDS size** sums distinct, nonoverlapping coding segments for each transcript. **Protein size** excludes one terminal stop symbol. MW and pI describe an unmodified protein. A CDS/protein length check does not establish translation identity.

## Sources

- [Oryzabase English gene list](https://shigen.nig.ac.jp/rice/oryzabase/download/gene)
- [RAP/MSU ID converter](https://rapdb.dna.naro.go.jp/converter/)
- [Biopython amino-acid masses](https://github.com/biopython/biopython/blob/master/Bio/Data/IUPACData.py)
- [Bjellqvist pI parameters in Biopython](https://github.com/biopython/biopython/blob/master/Bio/SeqUtils/IsoelectricPoint.py)

The matching and calculation core was tested locally against 65 protein models from 56 gene loci. Additional reader tests cover plain/headered ID lists, FASTA headers, CSV/TSV tables, gene-to-isoform expansion and invalid-file handling. Colab upload/download controls must be run inside Colab.
