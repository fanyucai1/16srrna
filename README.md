# pre:

    docker pull fanyucai1/16srrna

# method1:

    docker run -v /staging2/fanyucai/16s_rRNA/output/:/project/ \
    -v /staging2/fanyucai/16s_rRNA/test_data/:/opt/ 16srrna \
    sh -c "sh /software/16sPIP-master/bin/16sPIP.sh -i /opt/SRR9072077.1_1.fastq -r /opt/SRR9072077.1_2.fastq -p /software/16sPIP-master/"

https://github.com/jjmiao1314/16sPIP

Miao J, Han N, Qiang Y, et al. 16SPIP: a comprehensive analysis pipeline for rapid pathogen detection in clinical samples based on 16S metagenomic sequencing[J]. BMC bioinformatics, 2017, 18(16): 255-259.


# method2:

## build database

https://github.com/marcomeola/DAIRYdb

Now the database version is 2.0.

download database file named: SSU_DAIRYdb_v2.0_20210401_MTX.zip

    mkdir -p /path/to/metaxa2_db/SSU
    cp SSU_DAIRYdb_v2.0_20210401_MTX.zip /path/to/metaxa2_db/SSU/
    cd  /path/to/metaxa2_db/SSU/ && unzip SSU_DAIRYdb_v2.0_20210401_MTX.zip
    mv /path/to/metaxa2_db/SSU/SSU_DAIRYdb_v2.0_20210401_MTX/* /path/to/metaxa2_db/SSU/
    cd /path/to/metaxa2_db/SSU/ && blast-2.2.14/bin/formatdb -i blast.fasta -p F -n blast

## analysis

    docker run -v /staging2/fanyucai/16s_rRNA/output/:/project/ \
    -v /staging2/fanyucai/16s_rRNA/reference/metaxa2_db/:/software/Metaxa2_2.2.3/metaxa2_db/ \
	-v /staging2/fanyucai/16s_rRNA/test_data/:/opt/ 16srrna \
    /software/Metaxa2_2.2.3/metaxa2 -1 /opt/SRR9072077.1_1.fastq -2 /opt/SRR9072077.1_1.fastq \
	--cpu 8 -o test2 --plus T -t b,a -T 0,75,78.5,82,86.5,94.5,98.65


    docker run -v /staging2/fanyucai/16s_rRNA/output/:/project/ 16srrna \
    /software/Metaxa2_2.2.3/metaxa2_ttt -i /project/test.taxonomy.txt -o prefix

