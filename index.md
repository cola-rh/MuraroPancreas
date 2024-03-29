
## Hierarchical consensus partitioning analysis on dataset 'MuraroPancreas'

Runnable code:

```r
library(cola)

process_counts = function(data, column = NULL) {
    mat = assays(data)$counts
    mat = as.matrix(mat)
    s = colSums(mat)
    fa = s/mean(s)
    for(i in 1:ncol(mat)) mat[, i]/fa[i]
    mat = adjust_matrix(log2(mat + 1))

    anno = NULL
    if(!is.null(column)) {
        anno = colData(data)
           anno = as.data.frame(anno)
        anno = anno[, column, drop = FALSE]
    }

    list(mat = mat, anno = anno)
}

library(scRNAseq)
data = MuraroPancreasData()
lt = process_counts(data, c("label"))
rh = hierarchical_partition(lt$mat, subset = 500, cores = 4, anno = lt$anno)
saveRDS(rh, file = "MuraroPancreas_cola_rh.rds")

cola_report(rh, output = "MuraroPancreas_cola_rh_report", title = "cola Report for Hierarchical Partitioning - 'MuraroPancreas'")
```

[The HTML report is here.](https://cola-rh.github.io/MuraroPancreas/MuraroPancreas_cola_rh_report/cola_hc.html) (generated by __cola__ version 2.0.0, R 4.1.0.)

