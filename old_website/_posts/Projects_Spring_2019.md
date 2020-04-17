## Understanding the nature of (P)arsimonious (U)mi (G)raphs and related problems. 

During a typical single cell RNA-seq (scRNA-seq) experiment, short RNA molecules are tagged with UMI (unique molecule identifier sequences) and cell barcodes (again sequences). The cell barcodes help to identify the cells, the UMI sequence are used to identify individual molecules. We would always abreviate them as UMI and CB for rest of the discussion. 

Computational scRNA-seq experiment can be simplified as a simple generative process, and formalized as such. In this simple formulation we would follow the call set of genes as $$g_i$$ 