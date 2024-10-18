import pandas as pd
import allel

# Load the reference genome (FASTA)
reference_genome_path = 'path/to/reference_genome.fasta'
reference_genome = allel.read_fasta(reference_genome_path)

# Load the VCF file containing variant calls
vcf_path = 'path/to/variants.vcf'
callset = allel.read_vcf(vcf_path)

# Extract variant data
variants = callset['variants']
# Example mutations associated with drug resistance
# These are hypothetical; replace with actual mutations for M. tuberculosis
resistance_genes = ['rpoB', 'katG', 'inhA', 'embB']

# Create a DataFrame to store results
mutation_data = []

# Analyze variants
for gene in resistance_genes:
    gene_variants = variants[variants['ID'].str.contains(gene, na=False)]
    for index, variant in gene_variants.iterrows():
        mutation_data.append({
            'gene': gene,
            'position': variant['POS'],
            'ref': variant['REF'],
            'alt': variant['ALT'],
            'qual': variant['QUAL']
        })

# Convert to DataFrame
mutation_df = pd.DataFrame(mutation_data)
print(mutation_df)
# Summary of mutations
mutation_summary = mutation_df.groupby('gene').size().reset_index(name='mutation_count')
print(mutation_summary)

# Basic visualization
import matplotlib.pyplot as plt

mutation_summary.plot(kind='bar', x='gene', y='mutation_count', legend=False)
plt.title('Mutation Counts by Gene Associated with Drug Resistance')
plt.ylabel('Mutation Count')
plt.xlabel('Gene')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

