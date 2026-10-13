# Genome JS

A browser-based genomics viewer. Everything runs locally in your browser; no files are uploaded anywhere, and the page loads no fonts, scripts or analytics from other sites, so it works offline.

**Views:** linear and circular maps, whole-genome alignment blocks from Mauve (block map, identity profile, base-level view, pairwise identity), alignment pileup with coverage and consensus, variant tables, sequence comparison (dot plot, identity rings), assembly contigs, sample QC with Pass/Review/Repeat flags, notes and exports.

**Imports:** FASTA, GenBank, GFF3, BED, VCF, FASTQ, SAM, BAM, and Mauve alignments (`.xmfa`, `.backbone`). `.gz` accepted.

**Exports:** consensus FASTA, mutation table (CSV), VCF, QC table, GFF3/BED, SNP distance matrix, Markdown summary.

Click **Load demo data** to try it with synthetic sequences.

**Cursor:** in the Maps (linear), Alignment and Genome alignment views, a vertical cursor follows the mouse and shows the position. In Genome alignment it also marks the matching position in every other genome, through the block's alignment.

**Zoom to a region:** in Alignment and Genome alignment, drag across the view to zoom to that span (in Genome alignment the other genomes follow the matching sequence). Right-drag (or Shift+drag) pans, the wheel zooms, double-click zooms in.

**Genome alignment export:** Save map PNG/SVG exports the block map. Select a block and use Save block PNG/SVG to export its base-level alignment as shown (zoom and pan first to choose the window). Exports have a transparent background.

**Colours on the Maps tab:** the Colours button next to the resistance checkbox opens a panel listing the reference (backbone colour), each sample (coverage colour) and each comparison sequence (ring colour, with the shade still showing identity). Pick a colour to apply it live; ↺ resets it. Colours apply to PNG and SVG exports. Gene colours are set on the Annotations tab.

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
- Reference names must match exactly. The only relaxation is an accession-style name (for example `NC_045512.1` vs `NC_045512.2`), which is matched when unambiguous and reported. SAM/BAM `@SQ` lengths must equal the loaded sequence length, otherwise the reads are rejected.
- Input caps: up to 1,500,000 mapped reads per SAM/BAM and about 1.6 GB of decompressed BAM. If a file is cut short, damaged, or over the cap, the sample is marked **PARTIAL**, is never suggested as Pass, and the QC table, consensus header and exports say so. FASTQ statistics use the first 500,000 reads, and quick mapping the first 100,000 (a prefix, not a random sample).
- Consensus sequences are in reference coordinates and omit insertions (the QC table counts insertion sites that are not represented). Variant calls cover SNPs and deletions only.
- SNP distances count only sites where both consensus bases are A/C/G/T. The matrix shows the comparable fraction, and a long-format CSV lists it per pair.
- Protein effects honour GenBank `codon_start` and GFF phase, and translation tables 1, 11 and 4. Other tables are flagged and use the standard code.
- CSV exports quote commas, quotes and line breaks, and prefix text starting with `=`, `+`, `-` or `@` with an apostrophe so spreadsheets do not run it as a formula.
- Notes are kept in memory only. Tick "Keep notes in this browser" to store them locally, and use "Clear notes and saved data" to remove them. Removing an imported notes file removes its text from the notes.
- Analysis runs on the main thread, so very large files can make the page unresponsive for a while.
