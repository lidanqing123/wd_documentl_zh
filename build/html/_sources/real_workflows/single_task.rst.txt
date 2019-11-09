

Single-task workflow example: helloHaplotypeCaller
===================================================

有关此脚本的更多信息，请阅读其 `演练指南 <https://software.broadinstitute.org/wdl/userguide/article?id=7614>`_ 。


  :: 

	workflow helloHaplotypeCaller {
	  call haplotypeCaller
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


