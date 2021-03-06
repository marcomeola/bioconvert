.. _formats:

Formats
==========

Here below, we provide a list of formats used in bioinformatics or computational
biology. Most of these formats are used in **Bioconvert** and available for
conversion to another formats. Some are available for book-keeping. 

We hope that this page will be useful to all developers and scientists. Would
you like to contribute, please edit the file in our github **doc/formats.rst**.


.. _format_abi:

ABI
---
:Type: sequence

traces files, including the PHRED quality scores for the base calls.
This allows ABI to FASTQ conversion. Note that each ABI file contains one and only one sequence (no need for indexing the file). The trace data contains probablities of the four nucleotide bases along the sequencing run together with the sequence deduced from that data. ABI trace is a binary format.

File format produced by ABI sequencing machine. It produces ABI "Sanger" capillary sequence



.. admonition:: Bioconvert conversions:

    - :class:`~bioconvert.abi2qual.ABI2QUAL`
    - :class:`~bioconvert.abi2fastq.ABI2FASTQ`
    - :class:`~bioconvert.abi2fasta.ABI2FASTA`

.. seealso:: :ref:`scf`, :class:`~bioconvert.scf2fasta.SCF2Fasta`, 
    :class:`~bioconvert.scf2fastq.SCF2Fastq`, 


.. admonition::  References

    - ftp://saf.bio.caltech.edu/pub/software/molbio/abitools.zip
    - https://github.com/jkbonfield/io_lib/
    - http://www6.appliedbiosystems.com/support/software_community/ABIF_File_Format.pdf


.. _format_asqg:

ASQG
----

:Type: assembly
:status: deprecated format not included in **Bioconvert**

The ASQG format describes an assembly graph. Each line is a tab-delimited
record. The first field in each record describes the record type. The three
types are:

- HT: Header record. This record contains metadata tags for the file version
  (VN tag) and parameters associated with the graph (for example the minimum
  overlap length).
- VT: Vertex records. The second field contains the vertex identifier, the
  third field contains the sequence. Subsequent fields contain optional tags.
- ED: Edge description records. Fields are:
    - sequence 1 name
    - sequence 2 name
    - sequence 1 overlap start (0 based)
    - sequence 1 overlap end (inclusive)
    - sequence 1 length
    - sequence 2 overlap start (0 based)
    - sequence 2 overlap end (inclusive)
    - sequence 2 length
    - sequence 2 orientation (1 for reversed with respect to sequence 1)
    - number of differences in overlap (0 for perfect overlaps, which is the default).

Example::

    HT  VN:i:1  ER:f:0  OL:i:45 IN:Z:reads.fa   CN:i:1  TE:i:0
    VT  read1   GATCGATCTAGCTAGCTAGCTAGCTAGTTAGATGCATGCATGCTAGCTGG
    VT  read2   CGATCTAGCTAGCTAGCTAGCTAGTTAGATGCATGCATGCTAGCTGGATA
    VT  read3   ATCTAGCTAGCTAGCTAGCTAGTTAGATGCATGCATGCTAGCTGGATATT
    ED  read2 read1 0 46 50 3 49 50 0 0
    ED  read3 read2 0 47 50 2 49 50 0 0

.. admonition:: References

    - https://github.com/jts/sga/wiki/ASQG-Format


.. _bai_format:

BAI
---

The index file of a BAM file is a BAI file format. The BAI files are 
not used in **Bioconvert**. 

.. _bam_format:

BAM
---

:Type: Sequence alignement

The BAM (Binary Alignment Map) is the binary version of the Sequence 
Alignment Map (:ref:`SAM`) format.

.. admonition:: Bioconvert Conversions

    - :class:`~bioconvert.bam2sam.BAM2SAM`
    - :class:`~bioconvert.bam2cram.BAM2CRAM`
    - :class:`~bioconvert.bam2bedgraph.BAM2BEDGRAPH`
    - :class:`~bioconvert.bam2bed.BAM2BED`
    - :class:`~bioconvert.bam2bigwig.BAM2BIGWIG`
    - :class:`~bioconvert.bam2fasta.BAM2FASTA`
    - :class:`~bioconvert.bam2fastq.BAM2FASTQ`
    - :class:`~bioconvert.bam2json.BAM2JSON`
    - :class:`~bioconvert.bam2tsv.BAM2TSV`
    - :class:`~bioconvert.bam2wiggle.BAM2WIGGLE`

.. admonition:: References

    - http://samtools.github.io/hts-specs/SAMv1.pdf
    - http://genome.ucsc.edu/goldenPath/help/bam.html

.. seeqlso::

.. _bedgraph_format:

BEDGRAPH
--------

.. _bed_format:

BED
---

:reference: http://genome.ucsc.edu/FAQ/FAQformat.html#format1

BED file must has at least 3 columns (chrom, start, end).

.. _bigwig_format:

BIGWIG
------


FastA
-----

:Type: Sequence

This refers to the input FASTA file format where each record starts 
with a ">" line. Resulting sequences have a generic alphabet by default.   
There is no standard file extension for a text file containing FASTA formatted sequences. Although
their is a plethora of ad-hoc file extensions: fasta, fas, fa, seq, fsa, fna, ffn, faa, frn, we use only fasta, fa and fst. 


FastG
-----

:type: assembly
:reference: http://fastg.sourceforge.net/FASTG_Spec_v1.00.pdf

.. _fastq_format:

FastQ
-----

FASTQ files include sequences and their qualities. In general, *fastq*
refers to Sanger style FASTQ files which encode PHRED qualities using an
ASCII offset of 33. See also the incompatible "fastq-solexa" and "fastq-illumina"
variants used in early Solexa/Illumina pipelines, Illumina pipeline 1.8 produces Sanger FASTQ.
Be aware that there are different FASTQ formats for different sequencing technologiess


.. _gfa_format:

GFA
---

:type: assembly graph
:references: http://gfa-spec.github.io/GFA-spec/,

Overview
~~~~~~~~~~

The Graphical Fragment Assembly (GFA) can be used to represent genome
assemblies. GFA stores sequence graphs as the product of an
assembly, a representation of variation in genomes, splice graphs in genes, or
even overlap between reads from long-read sequencing technology.

The GFA format is a tab-delimited text format for describing a set of sequences
and their overlap. The first field of the line identifies the type of the line.
**Header** lines start with H. **Segment** lines start with S. **Link** lines start with L.
A **containment** line starts with C. A **path** line starts with P.


Terminology
~~~~~~~~~~~~~
- Segment a continuous sequence or subsequence.
- Link an overlap between two segments. Each link is from the end of one segment to the beginning of another segment. The link stores the orientation of each segment and the amount of basepairs overlapping.
- Containment an overlap between two segments where one is contained in the other.
- Path an ordered list of oriented segments, where each consecutive pair of oriented segments are supported by a link record.

See details in the reference above.

Example:
~~~~~~~~~

::

    H   VN:Z:1.0
    S   11  ACCTT
    S   12  TCAAGG
    S   13  CTTGATT
    L   11  +   12  -   4M
    L   12  -   13  +   5M
    L   11  +   13  +   3M
    P   14  11+,12-,13+ 4M,5M


Notes: sometimes you would have extra field (fourth one) on **segment** lines. 
Convertion to fasta will store this fourth line after the name.


GFA version 2
~~~~~~~~~~~~~~~~~~~~~~~~

GFA2 is a generalization of GFA that allows one to specify an assembly graph in
either less detail, e.g. just the topology of the graph, or more detail, e.g.
the multi-alignment of reads giving rise to each sequence. It is further
designed to be a able to represent a string graph at any stage of assembly, from
the graph of all overlaps, to a final resolved assembly of contig paths with
multi-alignments. Apart from meeting these needs, the extensions also supports
other assembly and variation graph types.

Like GFA, GFA2 is tab-delimited in that every lexical token is separated from
the next by a single tab.

.. _json_format:

JSON
----

TODO

.. _nexus_format:

Nexus
-----------

The NEXUS multiple alignment format, also known as PAUP format. 



PAF (Pairwise mApping Format)
--------------------------------

:reference: https://github.com/lh3/miniasm/blob/master/PAF.md

PAF is a text format describing the approximate mapping positions between two
set of sequences. PAF is used for instance in **miniasm** tool (see reference
above), an ultrafast de novo assembly for long noisy reads. PAF is TAB-delimited 
with each line consisting of the following predefined fields:

====== ======== ===========================================
Col     Type    Description
====== ======== ===========================================
1      string   Query sequence name
2       int     Query sequence length
3       int     Query start (0-based)
4       int     Query end (0-based)
5       char    Relative strand: "+" or "-"
6      string   Target sequence name
7       int     Target sequence length
8       int     Target start on original strand (0-based)
9       int     Target end on original strand (0-based)
10      int     Number of residue matches
11      int     Alignment block length
12      int     Mapping quality (0-255; 255 for missing)
====== ======== ===========================================

If PAF is generated from an alignment, column 10 equals the number of sequence
matches, and column 11 equals the total number of sequence matches, mismatches,
insertions and deletions in the alignment. If alignment is not available, column
10 and 11 are still required but can be approximate.

A PAF file may optionally contain SAM-like typed key-value pairs at the end of
each line.

PLINK flat files (MAP/PED)
-------------------------------

PLINK is a used application for analyzing genotypic data. It can be considered  the de-facto standard of the field. The MAP files describes the SNPs and contains those fields:

- chromosome number (integer)
- SNP marker ID (string)
- SNP generit position (cM) (float)
- SNP physical position (bp)

So it contains L lines with 4 columns. All SNPs must be ordered by physical
position. Example::

    X rs3883674 0 32380
    X rs12218882 0 48172
    9 rs10904045 0 48426
    9 rs10751931 0 49949

The PED (pedigree) file describes the individuals and the genetic data. The PED
file can be spaced or tab delimited. Each line corresponds to a single
individual. The first 6 columns are:

- family ID (or pedigree name): a unique alpha numeric identifier 
- individual ID: should be unique within his family
- father ID: 0 if unknown. If specified, must also appear as an individual in the file
- mother ID: same as above
- Sex: 1 Male, 2 Female
- Phenotype

- columns 7 and 8 code for the observed alleles at SNP1
- comumns 9 and 10 code for the observed alleles at SNP2 and so on

missing data are coded as "0 0". So we havez N lines 2L + 6 columns where N is
the number of individuals and L the numbers of SNPs

PLINK binary files (BED/BIM/FAM)
-------------------------------------
Same information as plink flat files. 

.. _qual_format:

QUAL
----

:Type: Sequence

TODO


BED for plink
~~~~~~~~~~~~~~
This BED format  is the binary PED file. Not to be confused with BED format used
with BAM files.

BIM files
~~~~~~~~~~~~~~~~~~~~

The fields are 

- chromosome number (integer)
- SNP marker ID (string)
- SNP generit position (cM) (float)
- SNP physical position (bp)
- Allele 1
- Allele 2

So, it is like the MAP with the 2 alleles, and the format is binary.

.. _fam_format:

FAM 
~~~

The first 6 columns of the PED file.


.. _sam_format:

SAM format
-------------

:Type: Sequence alignment
:reference: https://samtools.github.io/hts-specs/SAMv1.pdf


In the SAM format, each alignment line typically represents the linear alignment
of a segment.  Each line has 11 mandatory  fields in the same order. Their values
can be `0` or `*` if the field is unavailable. Here is an overview of those
fields:

======= ======= ======= ======================= ======================================
Col     Field   Type    Regexp/Range            Brief description
======= ======= ======= ======================= ======================================
1       QNAME   String  [!-?A-~]{1,254}         Query template NAME
2       FLAG    Int     [0,2^16-1]              bitwise FLAG
3       RNAME   String  \*|[!-()+-<>-~][!-~]*   Reference sequence NAME
4       POS     Int     [0,2^31-1]              1-based leftmost mapping POSition
5       MAPQ    Int     [0,2^8-1]               MAPping Quality
6       CIGAR   String  \*|([0-9]+[MIDNSHPX=])+ CIGAR string
7       RNEXT   String  \*|=|[!-()+-<>-~][!-~]* Ref.  name of the mate/next read
8       PNEXT   Int     [0,2^31-1]              Position of the mate/next read
9       TLEN    Int     [-2^31+1,2^31-1]        observed Template LENgth
10      SEQ     String  \*|[A-Za-z=.]+          segment SEQuence
11      QUAL    String  [!-~]+                  ASCII of Phred-scaled base QUALity+33
======= ======= ======= ======================= ======================================

All  optional   fields  follow  the TAG:TYPE:VALUE format  where TAG is  a  two-character  string  that  matches /[A-Za-z][A-Za-z0-9]/ .  Each TAG can only appear once in one alignment line.

The tag `NM:i:2` means: Edit distance to the reference (number of changes
necessary to make this equal to the reference, exceluding clipping).


The optional fields are tool-dependent. 

From BWA documentation, we can get this

==== ==================================================
Tag         Meaning
==== ==================================================
NM         Edit distance
MD         Mismatching positions/bases
AS         Alignment score
BC         Barcode sequence
X0         Number of best hits
X1         Number of suboptimal hits found by BWA
XN         Number of ambiguous bases in the referenece
XM         Number of mismatches in the alignment
XO         Number of gap opens
XG         Number of gap extentions
XT         Type: Unique/Repeat/N/Mate-sw
XA         Alternative hits; format: (chr,pos,CIGAR,NM;)*
XS         Suboptimal alignment score
XF         Support from forward/reverse alignment
XE         Number of supporting seeds
==== ==================================================

Note that XO and XG are generated by BWT search while the CIGAR string by
Smith-Waterman alignment. These two tags may be inconsistent with the CIGAR
string. This is not a bug

`SA:Z`: Other canonical alignments in a chimeric alignment, in the format of: (rname,pos,strand,CIGAR,mapQ,NM;)+. Each element in the semi-colon delimited list represents a part of the chimeric alignment. Conventionally, at a supplementary line, the first element points to the primary line.




.. _scf:

Trace File Format - Sequence Chromatogram Format (SCF)
------------------------------------------------------

:reference: https://wiki.nci.nih.gov/display/TCGA/Sequence+trace+files
:reference: http://staden.sourceforge.net/manual/formats_unix_2.html

Trace files are binary files containing raw data output from automated sequencing instruments.
This convertor was converted from BioPerl.


SCF file organisation (more or less)

====================================== ====================================
Length in bytes                        Data
====================================== ====================================
128                                    header
Number of samples * sample size        Samples for A trace
Number of samples * sample size        Samples for C trace
Number of samples * sample size        Samples for G trace
Number of samples * sample size        Samples for T trace
Number of bases * 4                    Offset into peak index for each base
Number of bases                        Accuracy estimate bases being 'A'
Number of bases                        Accuracy estimate bases being 'C'
Number of bases                        Accuracy estimate bases being 'G'
Number of bases                        Accuracy estimate bases being 'T'
Number of bases                        The called bases
Number of bases * 3                    Reserved for future use
Comments size                          Comments
Private data size                      Private data
====================================== ====================================





Stockholm
---------

The Stockholm alignment format is also known as PFAM format.   





Wiggle Track format (WIG)
-------------------------

:reference: http://genome.ucsc.edu/goldenPath/help/wiggle.html

The bigWig format is used for graphing track needs. The wiggle (WIG) format is
an older format for display of dense, continuous data such as GC percent. 
Wiggle data elements must be equally sized. 

Similar format such as the bedGraph format is also an older format used to display sparse data
or data that contains elements of varying size.

For speed and efficiency, wiggle data is compressed with a minor loss of precision when
data is exported from a wiggle track.


TODO
-------
bcf2vcf.py
bcf2wiggle.py
bigbed2wiggle.py
bigwig2bedgraph.py
bigwig2wiggle.py
bplink2plink.py
clustal2fasta.py
clustal2nexus.py
clustal2phylip.py
clustal2stockholm.py
csv2tsv.py
csv2xls.py
dsrc2gz.py
embl2fasta.py
embl2genbank.py
fasta2clustal.py
fasta2genbank.py
fasta2nexus.py
fasta2phylip.py
fasta2twobit.py
genbank2embl.py
genbank2fasta.py
genbank2gff3.py
gfa2fasta.py
gff22gff3.py
gff3gff2.py
gz2bz2.py
gz2dsrc.py
json2yaml.py
maf2sam.py
newick2nexus.py
newick2phyloxml.py
nexus2clustal.py
nexus2newick.py
nexus2phylip.py
nexus2phyloxml.py
ods2csv.py
phylip2clustal.py
phylip2fasta.py
phylip2nexus.py
phylip2stockholm.py
phylip2xmfa.py
phyloxml2newick.py
phyloxml2nexus.py
plink2bplink.py
plink2vcf.py
sam2paf.py
scf2fasta.py
scf2fastq.py
sra2fastq.py
stockholm2clustal.py
stockholm2phylip.py
tsv2csv.py
twobit2fasta.py
vcf2bcf.py
vcf2bed.py
vcf2bplink.py
vcf2plink.py
vcf2wiggle.py
wig2bed.py
xls2csv.py
xlsx2csv.py
xmfa2phylip.py
yaml2json.py
