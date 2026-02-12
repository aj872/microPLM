Inspired by Karpathy's microGPT, in this repo I port this to the proteins instead of names.
 
 Key changes from the original:
                                                                                                    
  1. Dataset — Swapped the text/names dataset for UniProt Swiss-Prot protein sequences. The script
  auto-downloads, decompresses, and parses the FASTA file (~90MB compressed)
  2. Tokenizer vocabulary — Instead of English characters, the vocab is the 20 standard amino acids
  (ACDEFGHIKLMNPQRSTVWY) + BOS token
  3. Data filtering — Parses FASTA format, keeps only proteins between 10–60 amino acids with
  standard residues only
  4. Block size — Set to 64 to match short protein/peptide lengths. 
  5. Inference output — Generates novel, "hallucinated" protein sequences instead of names/text

  The core architecture (GPT with autograd, attention, RMSNorm, Adam) is unchanged — which is kind
  of the cool part.

  To be honest, it lowkey sucks right now. Also its using autoregressive method in natural language models, not the masked token prediction that you usually see with protein lms.
  Some output from run 1: 

  --- inference (new, hallucinated proteins) ---                                
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

  While at least most of the start with M, most of these could not possibly exist (validated via alphafold). it does certain parts of protiens well, like alpha helixes. My intuition is, for a simple and small architechture, like this one derived from Karpathy, perhaps it would be improved with
  a smaller subset of proteins. 
