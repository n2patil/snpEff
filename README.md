snpEff with HGVS
=================

The snpEff make script assumes a specific Eclipsey directory structure. Let's not muck with that.

```
mkdir ~/workspace ~/snpEff
cd ~/workspace
git clone git@github.com:CBMi-BiG/snpEff.git SnpEff
cd SnpEff
```

Maven/Ant/Ivy have a hard time getting Picard and Samtools jars, but they are in the Picard distribution. Alter the pom.xml versions as necessary.

```
wget http://downloads.sourceforge.net/project/picard/picard-tools/1.84/picard-tools-1.84.zip
unzip picard-tools-1.84.zip
mvn install:install-file -DgroupId=net.sf.samtools -DartifactId=Sam -Dversion=1.84 \
-Dpackaging=jar -Dfile=$PWD/picard-tools-1.84/sam-1.84.jar
mvn install:install-file -DgroupId=net.sf.picard -DartifactId=Picard -Dversion=1.84 \
-Dpackaging=jar -Dfile=$PWD/picard-tools-1.84/picard-1.84.jar
```



To build snpEff-3.1-jar-with-dependencies.jar in the ~/workspace/SnpEff/target directory:


```
./scripts/make.sh
```

This is copied to ~/workplace/SnpEff/snpEff.jar per the config file

To get GRCh37 annotations:
```
mkdir -p data/GRCh37.64
java -jar snpEff.jar download GRCh37.64
```

To test (assuming you have downloaded all the test cases):

```
java -cp snpEff.jar \
ca.mcgill.mcb.pcingola.snpEffect.testCases.TestSuiteAll
```


HGVS annotations can be added to a given input file (i.e. ```tests/hgvs_test_in.vcf```) using ```-hgvsonly``` input parameter of snpEff. In this case, the input vcf MUST have snpEff annotation already and running snpEff again with this parameter, will add HGVS nomenclatures at the end of EFF field. The correct output should be the same as ```tests/hgvs_test_out.vcf```.

Running snpEff with ```-hgvs``` should run snpEff as a normal run (to add all annotations) with HGVS nomenclatures included in annotations.

```
java -Xmx4g -jar snpEff.jar hg19 -i vcf -o vcf tests/hgvs_test_in.vcf
```
