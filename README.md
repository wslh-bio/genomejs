# Genome JS

A browser-based genomics viewer. Everything runs locally in your browser; no files are uploaded anywhere.

**Views:** linear and circular maps, whole-genome alignment blocks from Mauve (block map, identity profile, base-level view, pairwise identity), alignment pileup with coverage and consensus, variant tables, sequence comparison (dot plot, identity rings), assembly contigs, sample QC with Pass/Review/Repeat flags, notes and exports.

**Imports:** FASTA, GenBank, GFF3, BED, VCF, FASTQ, SAM, BAM, and Mauve alignments (`.xmfa`, `.backbone`). `.gz` accepted.

**Exports:** consensus FASTA, mutation table (CSV), VCF, QC table, GFF3/BED, SNP distance matrix, Markdown summary.

Click **Load demo data** to try it with synthetic sequences.

**Cursor:** in the Maps (linear), Alignment and Genome alignment views, a vertical cursor follows the mouse and shows the position. In Genome alignment it also marks the matching position in every other genome, through the block's alignment.

## Run locally

Open `index.html` in a browser. No build step.

## Publish with GitHub Pages

The workflow in `.github/workflows/pages.yml` deploys the site automatically.

1. Push this folder to a GitHub repository, with the default branch named `main`.
2. In the repository, open Settings, then Pages.
3. Under Build and deployment, set Source to **GitHub Actions**.
4. Push to `main` (or open the Actions tab and run "Deploy to GitHub Pages" by hand).
5. The site appears at `https://<your-user>.github.io/<repository>/` once the workflow finishes.

Every later push to `main` redeploys the site.

## Limits

- Shared-identity values in the Compare view are estimated from shared 16-mers, not a base-level alignment.
- The FASTQ mapper is a quick ungapped screen of the first 100,000 reads. Use minimap2 or BWA for reportable results.
- Variant calling uses depth and fraction thresholds, not a statistical caller.
- BAM files are loaded whole in memory, so subset very large files first.
- Mauve: export from Mauve with File > Export Alignment to get `.xmfa`. `.backbone` files hold coordinates only. `.mauve` project files are not read. Identity is computed from the aligned columns in the XMFA.
