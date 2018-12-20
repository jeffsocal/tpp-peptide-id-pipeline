# TPP Peptide-Protein ID Pipeline

*Pipeline for feature detection using OpenMS*

## Set-up

1. Install Docker on your system
        [https://docs.docker.com/engine/installation/](https://docs.docker.com/engine/installation/)

2. By default, docker commands require root/sudo.  To configure access without sudo, execute the following command, then logout/login:

        sudo usermod -aG docker

3. Grab the latest TPP container and install them on your system    
    - all operations will be running from the commandline.  
    - An example as of 12/08/2017:

          docker pull biocontainers/tpp:5.0


4. After the container is downloaded, you can verify it's there via the command below.  Also note the full name of the container image under the REPOSITORY heading.  This name needs to be used in the 00_start_openms_container.sh script.

        docker images

5. Review the docker image name variable ```docker_img_name``` to that from the ```docker images``` command.


6. Download and setup UniProt

          wget ftp://ftp.uniprot.org/pub/databases/uniprot/current_release/decoy/uniprot_sprot.decoy.fasta.gz
          wget ftp://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/complete/uniprot_sprot.fasta.gz

          gunzip uniprot_sprot.decoy.fasta.gz
          gunzip uniprot_sprot.fasta.gz

          cat uniprot* > uniprot_sprot.tardec.fasta

 
7. Install and setup R

          sudo apt-get install libxml2-dev  
          R
          > install.packages('XML')
          > q()

8. Checkout the R helper tools repository.

        git clone https://github.com/jeffsocal/tpp-post-id-tools.git


## Running the Pipeline

          vi run.xml
          docker run xantdem -v <path> -w run.xml 
          docker run Xtandem2XML -v <path> -w <xtandem_out>.xml 
          docker run PeptideProphetParser -v <path> -w <xtandem_out>.xml -w <xtandem_out>.pepXML
          docker run ProteinProphet -v <path> -w <xtandem_out>.pepXML -w <xtandem_out>.protXML


          Rscript --slave pepxml2csv.R <path_to.pepXML> <path_to.pepXML.csv>
          Rscript --slave proxml2csv.R <path_to.protXML> <path_to.protXML.csv>
          Rscript --slave mzxml2csv.R <path_to.mzXML> <path_to.mzXML.csv>
          Rscript --slave merge_scans-ids.R <path_to.mzXML.csv> <path_to.pepXML.csv>

