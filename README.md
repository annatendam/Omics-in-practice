library(Matrix)
library(data.table)

matrix_dir <- "/scratch/Omics_in_practice/Anna/projects/brbseq/results/starsolo/Solo.out/Gene/raw"
output_file <- "/scratch/Omics_in_practice/Anna/projects/brbseq/results/umi_counts.txt"

mat_sparse <- readMM(file.path(matrix_dir, "umiDedup-1MM_Directional.mtx"))

features <- fread(
  file.path(matrix_dir, "features.tsv"),
  header = FALSE,
  data.table = FALSE
)

barcodes <- fread(
  file.path(matrix_dir, "barcodes.tsv"),
  header = FALSE,
  data.table = FALSE
)

stopifnot(nrow(mat_sparse) == nrow(features))
stopifnot(ncol(mat_sparse) == nrow(barcodes))

mat <- as.data.frame(as.matrix(mat_sparse))

colnames(mat) <- c(21, 7, 16, 22, 15, 9, 23, 8, 3, 5, 13, 20, 17, 18, 11, 4, 14, 2, 19, 12, 6, 24, 1, 10)
rownames(mat) <- features[[1]]

fwrite(
  mat,
  file = output_file,
  sep = "\t",
  quote = FALSE,
  row.names = TRUE,
  col.names = TRUE
)

message("Wrote: ", output_file)
