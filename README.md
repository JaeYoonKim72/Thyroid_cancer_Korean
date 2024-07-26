# Whole genome data of normal-tumor pairs of 459 Korean thyroid cancer individuals consisting of 15 subtypes

This somatic calling process for whole genome data of cancer patients was constructed by Jae-Yoon Kim (jaeyoonkim@kribb.re.kr), and it is based on GATK4, BWA-mem2, samtools, and bcftools softwares.

This pipeline starts with paired-end fastq files and does not cover quality check and control steps (e.g. fastqc or cutadapter).

Specifically, this pipeline uses a total of three somatic callers (Strelka2, Mutect2, and Varscan2), and ultimately provides somatic mutations with minimal false-positives.

Source code was written in bash and python, and only supported on linux platform.


## 1. Abstract

- Thyroid cancer is one of the endocrine malignancies with an incidence rate that ranks within the top ten worldwide. The rate is steadily increasing, particularly in Korea, where the incidence among women is rising due to the widespread adoption of diagnostic technologies. As NGS tech-nology has been introduced, many studies based on SNP-array and whole-exome sequencing have been reported in the thyroid cancer field. However, despite the advantages of whole-genome level research in discovering more clinical clues, not much has been reported on thyroid cancer yet. From this perspective, we generated high-quality whole-genome data from normal-tumor tissue pairs of 459 Korean thyroid cancer individuals, including 15 subtypes. MGI DNBseq-T7 was used with a read length of 150 bp, producing approximately 394M read pairs on average per individual. After quality control, high-quality somatic mutations were obtained, and a comprehensive mutation landscape including the 15 subtypes was presented. Our dataset provides a valuable resource to re-searchers studying various subtypes of thyroid cancer and is expected to contribute to therapy and diagnosis studies.


## 2. Work-flow

 - The work-flow of somatics calling procees is as follows:

![work_flow_new](https://github.com/JaeYoonKim72/CRC_Korean/assets/49300659/bf26dc22-72fc-4408-8d26-a5c01871fdf8)


## 3. Usage

### 3-1. STEP01: Mapping

 - Only paired-end reading is available, and the file name rules are as follows: [sample name]__[T or N ("T" for a tumor sample, "N" for a normal sample)]__[R1 or R2 (“R1” for a forward file, “R2” for a reverse file)].fastq.gz

 - The important thing is that before executing the script, you must open the script (e.g. vi or nano) and type each software path and environment variable appropriately.

 - The usage of the step01 script is as follows:

    Usage : sh step01.Mapping_to_BQSR.sh  [ Sample_T_R1.fastq.gz(=Forward read) ]    [ Sample_T_R2.fastq.gz(=Reverse read)]


![use1](https://github.com/JaeYoonKim72/CRC_Korean/assets/49300659/29469768-4bcb-4099-9094-b2cc343446b8)

    Example: sh step01.Mapping_to_BQSR.sh \
    
                         TEST1_N_R1.fastq.gz   \       # Forward reads

                         TEST2_N_R2.fastq.gz           # Reverse reads


### 3-2. STEP02: Somatic calling

 - BAM files for Normal and Tumor of each sample are required. The file name rules are as follows: [sample name]__[T or N ("T" for a tumor sample, "N" for a normal sample)].otehr.strings.bam

 - The important thing is that before executing the script, you must open the script (e.g. vi or nano) and type each software path and environment variable appropriately.

 - The usage of the step02 script is as follows:

    Usage : sh step02.BAM_to_Somatic_calling.sh   [ Sample_N.mapping.bam(=Normal bam)]     [ Sample_T.mapping.bam(=Tumor bam)]

![use2](https://github.com/JaeYoonKim72/CRC_Korean/assets/49300659/45bf62ef-d787-433b-ac53-c6269fe4da30)

    Example: sh step02.BAM_to_Somatic_calling.sh \
    
                         TEST1_N.markdup.BQSR.bam  \  # Normal bam
                          
                         TEST1_T.markdup.BQSR.bam  \  # Tumor bam
        
        
### 3-3. STEP03: Filtering and Merge mutations (detected in 2 more callers)

 - This STEP03 once again applies consistent filtering to the outputs of strelka2, mutect2, and varscan2, and finally presents somatic mutations detected in two or three callers among these three callers.

 - The important thing is that before executing the script, you must open the script (e.g. vi or nano) and type each software path and environment variable appropriately.
   
 - The usage of the step03 script is as follows:
   
    Usage : sh step03.Somatic_calling_to_Filter_and_Merge.sh   [ Strelka2.out.vcf ]     [ Mutect2.out.vcf ]     [ Varscan2.out.vcf ]

![use3](https://github.com/JaeYoonKim72/CRC_Korean/assets/49300659/814df0e1-271a-451b-8e95-61cb719f595c)

    Example: sh step03.Somatic_calling_to_Filter_and_Merge.sh \
    
                         Strelka2.out.vcf \                       # output of Strelka2 of STEP02
                         
                         Mutect2.out.vcf \                        # output of Mutect2 of STEP02
                         
                         Varscan2.out.vcf                         # output of Varscan2 of STEP02

                         

## 4. Contact

jaeyoonkim@kribb.re.kr


## 5. Citation

- Paper is under review.


## 6. Reference

 - GATK4: Van der Auwera, G. A., & O'Connor, B. D. (2020). Genomics in the cloud: using Docker, GATK, and WDL in Terra. O'Reilly Media.

 - SAMTOOLS: Li, H., Handsaker, B., Wysoker, A., Fennell, T., Ruan, J., Homer, N., ... & Durbin, R. (2009). The sequence alignment/map format and SAMtools. Bioinformatics, 25(16), 2078-2079.

 - BCFTOOLS: Danecek, P., Bonfield, J. K., Liddle, J., Marshall, J., Ohan, V., Pollard, M. O., ... & Li, H. (2021). Twelve years of SAMtools and BCFtools. Gigascience, 10(2), giab008.
 
 - BWA-MEM2: Vasimuddin, M., Misra, S., Li, H., & Aluru, S. (2019, May). Efficient architecture-aware acceleration of BWA-MEM for multicore systems. In 2019 IEEE international parallel and distributed processing symposium (IPDPS) (pp. 314-324). IEEE.
