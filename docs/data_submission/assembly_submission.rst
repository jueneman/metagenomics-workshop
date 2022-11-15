Submitting a metagenome assembly
==================

The submission of the assembly can be done only using ``webin-cli``.

Again, We will need to create a manifest file, which contains the metadata for our read data and references the assembly ``fasta`` file that should be submitted.

First, we create another directory::

  mkdir /mnt/submission/assembly/assembly

Then, we ``gzip`` the assembly file, since it needs to be zipped for submission::
  
  gzip /mnt/WGS-data/assembly_results/megahit_out/final.contigs.fa
  
And change to our submission directory::
  
  cd /mnt/submission/assembly/assembly

Create the manifest file
^^^^^^^^^^

TODO: coverage?
TODO: mingaplength

We will use this short manifest template for our submission::

  STUDY   TODO
  SAMPLE   TODO
  RUN_REF   TODO
  ASSEMBLYNAME   TODO
  ASSEMBLY_TYPE   primary metagenome
  COVERAGE   20
  PROGRAM   MEGAHIT
  PLATFORM   ILLUMINA
  MOLECULETYPE   genomic DNA
  DESCRIPTION   MEGAHIT assembly of the MG course 2022 dataset
  FASTA   /mnt/WGS-data/assembly_results/megahit_out/final.contigs.fa.gz
  
Create a file named ``manifest`` and fill it with the content above - fill the fields marked with TODO with the appropriate content. Then continue with the next step.

Validating the assembly submission
^^^^^^^^^^^^^^^^^^

Before actually submitting, we are validating our manifest file. To do so, we use the option ``-validate`` in our call of ``webin-cli``. Also, make sure, to use the ``-test`` flag to submit to the ENA test server. We also use ``-context=read`` since we are submitting reads. Other options are your ``-username``, ``-password`` and the path to the ``-manifest`` file::

  java -jar /mnt/submission/webin-cli-5.2.0.jar -username=$ENA_USER -password=$ENA_PWD -context=reads -manifest=manifest -validate -test

When everything was successfully validated, you should get a message like::

  INFO : The submission has been validated successfully.


Submit the reads
^^^^^^^^^^^^^^^^

Now, that our read submission is validated successfully, we can go on with the submission. Just replace the ``-validate`` flag by ``-submit`` in the ``webin-cli`` call. Do NOT remove the ``-test`` flag::

  java -jar /mnt/submission/webin-cli-5.2.0.jar -username=$ENA_USER -password=$ENA_PWD -context=reads -manifest=manifest -submit -test
 
If everything works fine, you should receive a message like::

  INFO : The TEST submission has been completed successfully. This was a TEST submission and no data was submitted. The following experiment accession was assigned to  the submission: ERX10008217
  INFO : The TEST submission has been completed successfully. This was a TEST submission and no data was submitted. The following run accession was assigned to the submission: ERR10488906


Now we can go on and submit our assembly.


References
^^^^^^^^^^
**ENA - Submitting A Primary Metagenome Assembly** https://ena-docs.readthedocs.io/en/latest/submit/assembly/metagenome/primary.html
