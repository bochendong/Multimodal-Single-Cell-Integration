# Multimodal-Single-Cell-Integration

This repositories is for kaggle competition Multimodal Single Cell Integration

https://www.kaggle.com/competitions/open-problems-multimodal

* [Data overview](./metadata/)
* [Multiome data Analyze](./multi/)
* [CITEseq data Analyze](./cite/)
* [Evaluation data Analyze](./submission/)

## Context
In the past decade, the advent of single-cell genomics has enabled the measurement of DNA, RNA, and proteins in single cells. These technologies allow the study of biology at an unprecedented scale and resolution. Among the outcomes have been detailed maps of early human embryonic development, the discovery of new disease-associated cell types, and cell-targeted therapeutic interventions. Moreover, with recent advances in experimental techniques it is now possible to measure multiple genomic modalities in the same cell.

While multimodal single-cell data is increasingly available, data analysis methods are still scarce. Due to the small volume of a single cell, measurements are sparse and noisy. Differences in molecular sampling depths between cells (sequencing depth) and technical effects from handling cells in batches (batch effects) can often overwhelm biological differences. When analyzing multimodal data, one must account for different feature spaces, as well as shared and unique variation between modalities and between batches. Furthermore, current pipelines for single-cell data analysis treat cells as static snapshots, even when there is an underlying dynamical biological process. Accounting for temporal dynamics alongside state changes over time is an open challenge in single-cell data science.

Generally, genetic information flows from DNA to RNA to proteins. DNA must be accessible (ATAC data) to produce RNA (GEX data), and RNA in turn is used as a template to produce protein (ADT data). These processes are regulated by feedback: for example, a protein may bind DNA to prevent the production of more RNA. This genetic regulation is the foundation for dynamic cellular processes that allow organisms to develop and adapt to changing environments. In single-cell data science, dynamic processes have been modeled by so-called pseudotime algorithms that capture the progression of the biological process. Yet, generalizing these algorithms to account for both pseudotime and real time is still an open problem.

Competition host Open Problems in Single-Cell Analysis is an open-source, community-driven effort to standardize benchmarking of single-cell methods. The core efforts of Open Problems include the formalization of existing challenges into measurable tasks, a collection of high-quality datasets, centralized benchmarking of community-contributed methods, and community-focused events that bring together diverse method developers to improve single-cell algorithms. They're excited to be partnering with Cellarity, Chan Zuckerbeg Biohub, the Chan Zuckerberg Initiative, Helmholtz Munich, and Yale to see what progress can be made in predicting changes in genetic dynamics over time through interdisciplinary collaboration.

There are approximately 37 trillion cells in the human body, all with different behaviors and functions. Understanding how a single genome gives rise to a diversity of cellular states is the key to gaining mechanistic insight into how tissues function or malfunction in health and disease. You can help solve this fundamental challenge for single-cell biology. Being able to solve the prediction problems over time may yield new insights into how gene regulation influences differentiation as blood and immune cells mature.

## Goal of the Competition
The goal of this competition is to predict how DNA, RNA, and protein measurements co-vary in single cells as bone marrow stem cells develop into more mature blood cells. You will develop a model trained on a subset of 300,000-cell time course dataset of CD34+ hematopoietic stem and progenitor cells (HSPC) from four human donors at five time points generated for this competition by Cellarity, a cell-centric drug creation company.

In the test set, taken from an unseen later time point in the dataset, competitors will be provided with one modality and be tasked with predicting a paired modality measured in the same cell. The added challenge of this competition is that the test data will be from a later time point than any time point in the training data.

Your work will help accelerate innovation in methods of mapping genetic information across layers of cellular state. If we can predict one modality from another, we may expand our understanding of the rules governing these complex regulatory processes.

<div align=center>
    <img src ="./image/predict.png"/>  
</div>

# Dataset Info
从四个人身上采集8种不同的细胞，在第2，3，4，7，10天采集，并应用两种不同的技术来分析

* 四人的编号: 13176, 27678, 31800, 32606
* 八种细胞分别为
    * HSC: 造血干细胞
    * EryP: 巨核細胞（红细胞的祖细胞）
    * BP: B淋巴球（白血球中一種淋巴細胞的亞型）
    * NeuP: 造血干/祖细胞
    * MkP: 巨核-红系祖细胞（分化为巨核祖细胞与红系祖细胞）
    * MasP: 肥大细胞（机体侵入门户的哨兵）
    * MoP: 单核细胞前体
    * Hidden: 不可见数据
* 两种技术
    * CITEseq: measures gene expression (RNA) and surface protein levels.
    * Multiome: measures gene expression (RNA) and chromatin accessibility
    <div align=center>
        <img src ="./image/whole_process.jpg" width="800" height ="400"/>  
    </div>
* 数据采集
    * 训练数据（Training Set）
        * 训练数据只包含从13176， 31800和32606号实验体上采集的数据
        * 对于使用Multiome技术的数据，只从第2，3，4，7天采集
        * 对于使用CITEseq技术的数据，只从第2，3，4天采集
    * 可见测试数据（public test set）
        * 只包含从276786号实验体上采集的数据
        * 对于使用Multiome技术的数据，只从第2，3，7天采集
        * 对于使用CITEseq技术的数据，只从第2，3，4天采集
    * 不可见测试数据（private test set）
        * 包含从13176， 276786，31800和32606号实验体上采集的数据
        * 对于使用Multiome技术的数据，只从第10天采集
        * 对于使用CITEseq技术的数据，只从第7天采集
    * Note: There are no day 10 CITEseq samples in any split.
        
* 任务

    * Your task is to predict the labels corresponding to the inputs in the test set. To facilitate submission scoring, we only require predictions on a subset of the Multiome data. This subset was created by sampling 30% of the Multiome rows, and for each row, 15% of the columns. The sample of columns varies from row-to-row. All of the CITEseq labels are scored.
