
Simple branched workflow example: SimpleVariantSelection
==========================================================

有关此脚本的更多信息，请阅读其 `演练指南 <https://software.broadinstitute.org/wdl/userguide/article?id=7614>`_ 。


  :: 

	workflow SimpleVariantSelection {
	  File gatk
	  File refFasta
	  File refIndex
	  File refDict
	  String name
	  call haplotypeCaller {
	    input: 
	      sampleName=name, 
	      RefFasta=refFasta, 
	      GATK=gatk, 
	      RefIndex=refIndex, 
	      RefDict=refDict
	  }
	  call select as selectSNPs {
	    input: 
	      sampleName=name, 
	      RefFasta=refFasta, 
	      GATK=gatk, 
	      RefIndex=refIndex, 
	      RefDict=refDict, 
	      type="SNP", 
	      rawVCF=haplotypeCaller.rawVCF
	  }
	  call select as selectIndels {
	    input: 
	      sampleName=name, 
	      RefFasta=refFasta, 
	      GATK=gatk, 
	      RefIndex=refIndex, 
	      RefDict=refDict, 
	      type="INDEL", 
	      rawVCF=haplotypeCaller.rawVCF
	  }
	}

	#This task calls GATK's tool, HaplotypeCaller in normal mode. This tool takes a pre-processed 
	#bam file and discovers variant sites. These raw variant calls are then written to a vcf file.
	task haplotypeCaller {
	  File GATK
	  File RefFasta
	  File RefIndex
	  File RefDict
	  String sampleName
	  File inputBAM
	  File bamIndex
	  command {
	    java -jar ${GATK} \
	      -T HaplotypeCaller \
	      -R ${RefFasta} \
	      -I ${inputBAM} \
	      -o ${sampleName}.raw.indels.snps.vcf
	  }
	  output {
	    File rawVCF = "${sampleName}.raw.indels.snps.vcf"
	  }
	}

	#This task calls GATK's tool, SelectVariants, in order to separate indel calls from SNPs in
	#the raw variant vcf produced by HaplotypeCaller. The type can be set to "INDEL"
	#or "SNP".
	task select {
	  File GATK
	  File RefFasta
	  File RefIndex
	  File RefDict
	  String sampleName
	  String type
	  File rawVCF
	  command {
	    java -jar ${GATK} \
	      -T SelectVariants \
	      -R ${RefFasta} \
	      -V ${rawVCF} \
	      -selectType ${type} \
	      -o ${sampleName}_raw.${type}.vcf
	  }
	  output {
	    File rawSubset = "${sampleName}_raw.${type}.vcf"
	  }
	}
