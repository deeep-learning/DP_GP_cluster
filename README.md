
## DP_GP_cluster

DP_GP_cluster clusters genes by expression over a time course using a Dirichlet process Gaussian process model.
    
## Motivation

Genes that follow similar expression trajectories in response to stress or stimulus tend to share biological functions.  Thus, it is reasonable and common to cluster genes by expression trajectories.  Two important considerations in this problem are (1) selecting the "correct" or "optimal" number of clusters and (2) modeling the trajectory and time-dependency of gene expression. A [Dirichlet process](http://en.wikipedia.org/wiki/Dirichlet_process) can determine the number of clusters in a nonparametric manner, while a [Gaussian process](http://en.wikipedia.org/wiki/Gaussian_process) can model the trajectory and time-dependency of gene expression in a nonparametric manner.

## Installation and Dependencies

DP_GP_cluster requires the following Python packages:
    
    GPy, pandas, numpy, scipy (>= 0.14), matplotlib.pyplot

It has been tested in linux with Python 2.7 and with Anaconda distributions of the latter four above packages.

Download source code and uncompress, then:

    python setup.py install

## Tests

    cd DP_GP_cluster
    DP_GP_cluster.py -i test/test.txt -o test/test -p png -n 20 --plot

## Code Examples

To cluster genes by expression over time course and create gene-by-gene posterior similarity matrix:
    
    DP_GP_cluster.py -i /path/to/expression.txt -o /path/to/output_prefix [ optional args, e.g. -n 2000 --true_times --criterion MAP --plot ... ]
    
Above, `expression.txt` is of the format:

    gene    1     2    3    ...    time_t
    gene_1  10    20   5    ...    8
    gene_2  3     2    50   ...    8
    gene_3  18    100  10   ...    22
    ...
    gene_n  45    22   15   ...    60

where the first row is a header containing the time points and the first column is an index containing all gene names. Entries are delimited by whitespace (space or tab), and for this reason, do not include spaces in gene names or time point names.

From the above command, the optimal clustering will be saved at `/path/to/output_path_prefix_optimal_clustering.txt` in a simple tab-delimited format:

    cluster	gene
    1	gene_1
    1	gene_23
    2	gene_7
    ...
    k	gene_30
    
Because the optimal clustering is chosen after the entirety of Gibbs sampling, the script can be rerun with alternative clustering optimality criteria to yield different sets of clusters. Also, if `--plot` flag was not indicated when the above script is called, plots can be generated after sampling:

    DP_GP_cluster.py \
    -i /path/to/expression.txt \
    --sim_mat /path/to/output_prefix_posterior_similarity_matrix.txt \
    --clusterings /path/to/output_prefix_clusterings.txt \
    --criterion MPEAR \
    --post_process \
    --plot --plot_types png,pdf \
    --output /path/to/output_prefix_MPEAR_optimal_clustering.txt \
    --output_path_prefix /path/to/output_prefix_MPEAR

When the `--plot` flag is indicated, the script plots (1) gene expression trajectories by cluster along with the Gaussian Process parameters of each cluster and (2) the posterior similarity matrix in the form of a heatmap with dendrogram. For example:

#### Gene expression trajectories by cluster*
![expression](https://github.com/PrincetonUniversity/DP_GP_cluster/blob/master/auxiliary/expression.png)

*from McDowell et al. 2017, A549 dexamethasone exposure RNA-seq data

#### Posterior similarity matrix**
![PSM](https://github.com/PrincetonUniversity/DP_GP_cluster/blob/master/auxiliary/PSM.png)

**from McDowell et al. 2017, _H. salinarum_ hydrogen peroxide exposure microarray data

For more details on particular parameters, see detailed help message in script.

## Citation

citation of publication

## License

[BSD 3-clause](https://github.com/PrincetonUniversity/DP_GP_cluster/blob/master/LICENSE)
    