# microPLM

Inspired by [Karpathy's microGPT](https://github.com/karpathy/microGPT), ported to proteins instead of names.

The core architecture (GPT with autograd, attention, RMSNorm, Adam) is unchanged — which is kind of the cool part.

## Key changes from the original

1. **Dataset** — Swapped the text/names dataset for [UniProt Swiss-Prot](https://www.uniprot.org/) protein sequences. The script auto-downloads, decompresses, and parses the FASTA file (~90MB compressed).
2. **Tokenizer vocabulary** — Instead of English characters, the vocab is the 20 standard amino acids (`ACDEFGHIKLMNPQRSTVWY`) + a `<BOS>` token.
3. **Data filtering** — Parses FASTA format, keeps only proteins between 10–60 amino acids with standard residues only.
4. **Block size** — Set to 64 to match short protein/peptide lengths.
5. **Inference output** — Generates novel, "hallucinated" protein sequences instead of names/text.

## Limitations

To be honest, it lowkey sucks right now. It's also using the autoregressive method from natural language models, not the masked token prediction you usually see with protein LMs.

While most generated sequences start with M (methionine — the universal start codon, so that's good), most of these could not possibly exist (validated via AlphaFold). It does get certain parts of proteins right, like alpha helices. My intuition is that for a simple and small architecture like this one, performance could be improved by training on a smaller, more focused subset of proteins.

## Sample output

Generated sequences from run 1:

```
sample  1: MTTTLPVSSLSTLLLVLIIPG
sample  2: MAFSLGVVLIVKFLAVLLAAAGASVQVLGSLFAARFTFLVFIVN
sample  3: MGISVIPPVLAVIAGSYIFLVAVAALGFDPAITGQAALIFFFIIIHEIVIK
sample  4: MKSLIIDAPLFYVLKVKVQFLLKIFYALAVLKRSYFAVKIVVILKHHTKVKQKVI
sample  5: MLVPKTVTLPPGL
sample  6: MKIRLSKVIPKKVIKVVIRLKMVVINSNKLAMYVKEG
sample  7: MKVRKLGVSLCRKCGRRRRKKGKRRNCKVPKARRRRKKKPRVNKKRTACRKCGG
sample  8: MLVAIDEPIFGSLLFFGEPILFVPILALVVPSKIVAGKFP
sample  9: MKTKTLKKPFFYRVAGFRRKLASVIAFGFLLKISQDRKAHGRARKGKVNSKKKGKRRK
sample 10: MSKGINSKRLVVSKRSNSTARIAGRNGREINMNRRTLFRKRIIAKRKNKKKSVPAGVK
sample 11: ALGLLLSMVPF
sample 12: MSKKNRKRKICKVRKKRKRTKRICKRKKRRKRKRRRHGRKRVSKKRRRKKKRSRKKKKA
sample 13: MKKLTFTHPLVPKGRKIRAVTAAKNICKDPAK
sample 14: MEDTLAVSNTQFLSLVPFVALVSGLTIGLPIFSGFFVKLFFALFTLASEKEVSFAAAVP
sample 15: MRSVLRVFSTKRRKRRGRRWRRKVVVVKVIKRKRRERRRKRVLRRKAERKAKKVSKSA
sample 16: MSQSTLVFWFVKRKRSGIRNLLVYANRYTGRRKVLFRKGKR
sample 17: MVSTAPVPVGTFLLIALFFLALLLLGLTGVNQPLLISSFETKLAVVELP
sample 18: MSIGLTVLLFTFILIGSPAAFLMVVTLGGLTLSICYK
sample 19: MPEPTLGSSYDHVQGLVGLFLMLATNLIKKLFLINQEKKY
sample 20: MKVLVLFSLVIVPFLVALFLFFTIATILGVIPNIR
```
