# Hydropathy-Plot

## **Project Overview**

* This project aims to produce the hydropathy plot for a given protein.
* Provide visualization of the physical properties of the given protein through analyzing its amino acid sequences where each amino acid has a specific hydropathy index.
* Allows the determination of the presence of any transmembrane domains and their corresponding locations within the amino acid sequence.
* The protein information comes from PDB (Protein Data Bank) where it provides the amino acid sequence and the residue count of a protein.
* This problem was interesting to me because in class, we were often sketch hydropathy plots by hand based on protein conformation in the membrane, and I have always been curious about how a hydropathy plot was generated by a computer program through the use of physical properties of amino acids.

## Design Choices and Reasoning

#### Systematic design
* Writing the signature for every function produced helps to code the function body. The signature tells us what paramaters should be used in the function body and what output type should be produced by the function body.
* Helper functions allows complicated problems to be dissected into smaller parts. For instance, designing the function that produces the hydropathy scores at each given point in an amino acid sequence required us to convert the sequence as a str to a list of hydropathy indices and to a list of hydropathy scores.

#### Program choices
* I have chosen to use Enumeration (AminoAcid) to represent all the amino acids and have chosen to store the sequence in the form of List[AminoAcid] because,
 
    1) There can any be one amino acid with a corresponding hydropathy index at any given position for a protein.

    2) Each amino acid has their own specific chemical/physical properties, and storing the sequence in the form of enumeration allows easier analysis if we wanted to analyze another property say hydrophilicity.


* I have chosen to end the sequence 10 amino acid residues early, because by the time python evaluates the average from the 10th last amino acid, it would already have finished "evaluating" the entire sequence, and if it moves on the the 9th last amino acid, it would not have enough sequences to calculate an average.

## Problem Solving
Goal: Produce a line plot that illustrates the hydrophobicity properties of a protein along its amino acid sequence number and a threshold line. Where sequence segments above the threshold line suggests a transmembrane domain (high hydropathy score). 

* Generated a list[int] for the amino acid number along the sequence for a given protein.
* Generated a list[int] for averages of every 10 amino acid residues in a "moving window" manner, where the amino acid sequences was converted from a (1) str to a (2) list of AminoAcid in enumeration to a (3) list of hydropathy indices and finally (4) a list of hydropathy score (average of 10 hydropathy indices).

#### What my program produced for 3NE2...
https://user-images.githubusercontent.com/74755460/109886716-62585880-7c35-11eb-90fb-4c64ce13f242.png

#### What ExPASy (Bioinformatics Resources Portal) produced for 3NE2...
https://user-images.githubusercontent.com/74755460/109886539-0ee60a80-7c35-11eb-97c8-edf552cc89c2.png


## Most Challenging
* To create a function that returns a list of the average of a set number of consecutive hydropathy indices as a "moving window".
* E.g. to find the average of 3 consecutive numbers (my programs finds the average of 10 numbers) from a list of [0, 1, 2, 3, 4, ..., 22, 23, 24, 25], the function returns [(0+1+2)/3, (1+2+3)/3, (2+3+4)/3, ..., (23+24+25)/3].
* This was the most difficult because we're not simply plotting hydrophathy indices against the residue number because the hydropathy score measures the relative hydrophobicity of a range of amino acids.
* This required the use of a function that was not taught in CPSC103 and several helper functions to convert the amino acid sequence number from the csv file as 1 string to a List of hydropathy index averages.

## Future Work
* This plot allows the determination of number of TMS and their general location within the amino acid sequence for a given protein.
* Researchers could use this information to determine % similairty between two proteins through comparing their amino acids sequences, or to determine if two proteins share the same domain through comparing the amino acid sequences at a specific region (e.g. N-terminus).

## Functions Template used for producing a hydropathy plot
```
from cs103 import *
from typing import NamedTuple, List
import csv
from enum import Enum
from matplotlib import pyplot

##################
# Data Definitions

ProteinInfo = NamedTuple("ProteinInfo",[('protein_id', str),
                                        ('resi_count', int), # in range [0, ...]
                                        ('sequence', str)])
# interp. a protein with a protein ID ('protein_ID'), residue count ('resi_count'),
#         and an amino acid sequence ('sequence')

PI1 = ProteinInfo('1FQY', 5, 'MASEF')
PI2 = ProteinInfo('1LDA', 5, 'MSQTS')
PI3 = ProteinInfo('2ABM', 3, 'MSQ')
PI4 = ProteinInfo('2D57', 12, 'MSQTSCPPWYFN')


@typecheck
#template based on Compound (3 fields)
def fn_for_protein_info(pi: ProteinInfo) -> ...:
    return ...(pi.protein_id,
               pi.resi_count, # int in range [0, ...]
               pi.sequence)


# List[ProteinInfo]
# interp. a list of proteins

LOPI0 = []
LOPI1 = [PI1]
LOPI2 = [PI1, PI2]
LOPI3 = [PI3, PI4]

@typecheck
# template based on arbitrarily sized and reference rule
def fn_for_lopi(lopi: List[ProteinInfo]) -> ...:
    #description of acc
    acc = ... #type: ...
    for pi in lopi:
        acc = ... (acc, fn_for_protein_info(pi))
    return acc

AminoAcid = Enum("AminoAcid", ['I', 'V', 'L', 'F', 'C', 'M', 'A', 'G', 'T', 'S', 'W', 'Y', 'P',
                               'H', 'E', 'Q', 'D', 'N', 'K', 'R'])
# interp. an amino acid is one of isoleucine ('I'), valine ('V'), leucine ('L'), Phenylalanine ('F'),
#         cysteine ('C'), methionine ('M'), alanine ('A'), glycine ('G'), threonine ('T'), serine ('S'),
#         tryptophan ('W'), tyrosine ('Y'), proline ('P'), histidine ('H'), glutamate ('E'), glutamine (Q),
#         aspartate ('D'), asparagine ('N'), lysine ('K'), or arginine ('R').
# Examples are redundant for enumerations

@typecheck
# template based on Enumeration (20 cases)
def fn_for_aa(aa: AminoAcid) -> ...:
    if (aa == AminoAcid.I):
        return ...
    elif (aa == AminoAcid.V):
        return ...
    elif (aa == AminoAcid.L):
        return ...
    elif (aa == AminoAcid.F):
        return ...
    elif (aa == AminoAcid.C):
        return ...
    elif (aa == AminoAcid.M):
        return ...
    elif (aa == AminoAcid.A):
        return ...
    elif (aa == AminoAcid.G):
        return ...
    elif (aa == AminoAcid.T):
        return ...
    elif (aa == AminoAcid.S):
        return ...
    elif (aa == AminoAcid.W):
        return ...
    elif (aa == AminoAcid.Y):
        return ...
    elif (aa == AminoAcid.P):
        return ...
    elif (aa == AminoAcid.H):
        return ...
    elif (aa == AminoAcid.E):
        return ...
    elif (aa == AminoAcid.Q):
        return ...
    elif (aa == AminoAcid.D):
        return ...
    elif (aa == AminoAcid.N):
        return ...
    elif (aa == AminoAcid.K):
        return ...
    elif (aa == AminoAcid.R):
        return ...

# List[AminoAcid]
# interp. a list of amino acids

LOAA0 = []
LOAA1 = [AminoAcid.N, AminoAcid.K, AminoAcid.M]
LOAA2 = [AminoAcid.V, AminoAcid.I, AminoAcid.G]

@typecheck
# template based on arbitrarily sized and reference rule
def fn_for_loaa(loaa: List[AminoAcid]) -> ...:
    #description of acc 
    acc = ... #type: ...
    for aa in loaa:
        acc = ... (acc, fn_for_aa(aa))
    return acc
```


# Code for producing a hydropathy plot
```
from cs103 import *
from typing import NamedTuple, List
import csv
from enum import Enum
from matplotlib import pyplot

# Main function
@typecheck
def main(filename: str, protein: str) -> None:
    """
    Reads the file from given filename and return a line plot (hydropathy plot) of a chosen protein
    by plotting the hydeopathy score (y-axis) against amino acid residue number (x-axis)
    """
    # Template from HtDAP, based on function composition 
    return hydro_plot(read(filename), protein) 
    
    
# Read function
@typecheck
def read(filename: str) -> List[ProteinInfo]:
    """    
    reads information from the specified file and returns a list of ProteinInfo containing
    a protein ID, residue count, and amino acid sequence
    """
    #return []  #stub
    # Template from HtDAP
    # lopi contains the result so far
    lopi = [] # type: List[ProteinInfo]

    with open(filename) as csvfile:
        
        reader = csv.reader(csvfile)
        next(reader) # skip header line

        for row in reader:
            # you may not need to store all the rows, and you may need
            # to convert some of the strings to other types
            pi = ProteinInfo(row[0],            # just a str
                             parse_int(row[8]), # represent an int [0, ...]
                             row[15])           # just a str
            lopi.append(pi)
    
    return lopi

# Analyze function
@typecheck
def hydro_plot(lopi: List[ProteinInfo], protein: str) -> None: 
    """ 
    show the physical properties (hydrophobic/hydrophilic) of a given protein via hydropathy plot (a line plot)
    in which we have the amino acid sequence of a protein and their corresponding hydrophobic index
    """ 
    # return None #stub
    # template from visualization
    
    #data values
    x_vals = get_residue(residue(lopi, protein))
    y_vals = get_hydro_score(list_of_hydro_index(list_of_aa_enum(list_of_aa_str(lopi, protein))))
    y_vals_thresh = get_thresh(residue(lopi, protein))
    
    
    # set the labels for the axes
    pyplot.xlabel("Amino Acid Sequence Number")
    pyplot.ylabel("Hydropathy score")
    pyplot.title("Hydropathy plot of " + protein)
    
    # plot our data 
    line_plot = pyplot.plot(x_vals, y_vals)
    line_threshold = pyplot.plot(x_vals, y_vals_thresh)

    # set some properties for the line (color to red, line width to 2, and marker to a small circle)
    pyplot.setp(line_plot, color='r', linewidth=2.0)
    
    
    # show the plot
    pyplot.show()
    
    return None


@typecheck
def is_protein(pi: ProteinInfo, protein: str) -> bool:
    """
    return True if a protein, pi, is the given protein,
    and return False if otherwise
    """
    # return False #stub
    # template from ProteinInfo with additional parameters
    
    return pi.protein_id == protein


@typecheck
def list_of_aa_str(lopi: List[ProteinInfo], protein: str) -> List[str]:
    """
    splits an amino acid sequence as str into a list of individual amino acid residues
    for a particular protein in lopi
    """
    # return [] #stub
    # template from List[ProteinInfo] with additional parameter
    # sequence contains all the amino acid in the str seen so far
    sequence = [] #type: List[str]
    for pi in lopi:
        if is_protein(pi, protein):
            sequence = list(pi.sequence)
    return sequence


@typecheck
def str_to_enum(aa: str) -> AminoAcid:
    """
    convert an amino acid residue as a str to AminoAcid (enumeration)
    """
    # return AminoAcid.N #stub
    # template: return ...(aa)
    
    if (aa == 'I'):
        return AminoAcid.I
    elif (aa == 'V'):
        return AminoAcid.V
    elif (aa == 'L'):
        return AminoAcid.L
    elif (aa == 'F'):
        return AminoAcid.F
    elif (aa == 'C'):
        return AminoAcid.C
    elif (aa == 'M'):
        return AminoAcid.M
    elif (aa == 'A'):
        return AminoAcid.A
    elif (aa == 'G'):
        return AminoAcid.G
    elif (aa == 'T'):
        return AminoAcid.T
    elif (aa == 'S'):
        return AminoAcid.S
    elif (aa == 'W'):
        return AminoAcid.W
    elif (aa == 'Y'):
        return AminoAcid.Y
    elif (aa == 'P'):
        return AminoAcid.P
    elif (aa == 'H'):
        return AminoAcid.H
    elif (aa == 'E'):
        return AminoAcid.E
    elif (aa == 'Q'):
        return AminoAcid.Q
    elif (aa == 'D'):
        return AminoAcid.D
    elif (aa == 'N'):
        return AminoAcid.N
    elif (aa == 'K'):
        return AminoAcid.K
    elif (aa == 'R'):
        return AminoAcid.R


@typecheck
def list_of_aa_enum(los: List[str]) -> List[AminoAcid]:
    """
    return a list of AminiAcid from aa_str
    """
    # return [] #stub
    # template from List[str]
    # sequence contains all the amino acids converted to AminoAcid in aa_str seen so far
    sequence = [] #type: List[AminoAcid]
    for s in los:
        sequence.append(str_to_enum(s))
    return sequence
   

# a function that returns the hydropathy index of an amino acid (float)
@typecheck
def hydro_index(aa: AminoAcid) -> float:
    """
    returns the hydropathy index of an amino acid
    """
    # return 0 #stub
    # template based on AminoAcid
    
    if (aa == AminoAcid.I):
        return 4.5
    elif (aa == AminoAcid.V):
        return 4.2
    elif (aa == AminoAcid.L):
        return 3.8
    elif (aa == AminoAcid.F):
        return 2.8
    elif (aa == AminoAcid.C):
        return 2.5
    elif (aa == AminoAcid.M):
        return 1.9
    elif (aa == AminoAcid.A):
        return 1.8
    elif (aa == AminoAcid.G):
        return -0.4
    elif (aa == AminoAcid.T):
        return -0.7
    elif (aa == AminoAcid.S):
        return -0.8
    elif (aa == AminoAcid.W):
        return -0.9
    elif (aa == AminoAcid.Y):
        return -1.3
    elif (aa == AminoAcid.P):
        return -1.6
    elif (aa == AminoAcid.H):
        return -3.2
    elif (aa == AminoAcid.E):
        return -3.5
    elif (aa == AminoAcid.Q):
        return -3.5
    elif (aa == AminoAcid.D):
        return -3.5
    elif (aa == AminoAcid.N):
        return -3.5
    elif (aa == AminoAcid.K):
        return -3.9
    elif (aa == AminoAcid.R):
        return -4.5


@typecheck
def list_of_hydro_index(loaa: List[AminoAcid]) -> List[float]:
    """
    return a list of hydropathy index (float) from loaa
    each amino acid has a specific hydropathy index
    """
    # return [] #stub
    # template from List[AminoAcid]
    # score contains all the hydropathy index in loaa seen so far
    index = [] #type: List[float]
    for aa in loaa:
        index.append(hydro_index(aa))
    return index


@typecheck
def get_hydro_score(lof: List[float]) -> List[float]:
    """
    return the average hydropathy score of 10 successive amino acids as a moving window,
    the first number of the list contains the average of 1st to 10th amino acid,
    the second number of the list contains the average of 2nd to 11th amino acid and so on,
    i.e. [1,2,3,4,5,6,7,8,9,10,11,12] returns a [5.5, 6.5, 7.5] as a list
    """
    # return [] #stub
    # template from List[float]
    # score contains the all the average of result in lof seen so far
    score = [] #type: List[float]
    for i in range(0, (len(lof) - 9)):
        curr_score = lof[i]/10 + lof[i+1]/10 + lof[i+2]/10 + lof[i+3]/10 + lof[i+4]/10 + lof[i+5]/10 + lof[i+6]/10 + lof[i+7]/10 + lof[i+8]/10 + lof[i+9]/10
        score.append(round(curr_score,1))
    return score

@typecheck
def residue(lopi: List[ProteinInfo], protein: str) -> int:
    """
    return the residue count of protein in lopi, 
    and if protein is not contained in lopi return 0
    """
    # return 0 #stub
    # template from List[ProteinInfo] with additional parameter

    for pi in lopi:
        if is_protein(pi, protein):
            return (len(pi.sequence) - 9)
    else:
        return 0

@typecheck
def get_residue(resi: int) -> List[int]:
    """
    return a list of int in the range of 0 to the residue count
    e.g. if residue count is 5, return [0, 1, 2, 3, 4]
    """
    # return [] #stub
    # template: return ...(resi)
    
    if resi == 0:
        return list(range(0))
    else:
        return list(range(resi))

@typecheck
def get_thresh(resi: int) -> List[float]:
    """
    return a list of constant threshold values at 0
    """
    # return [] #stub
    # template: return... (resi)
    
    return [0] * resi

start_testing()
# Examples and tests for main
expect(main('aquaporin_empty.csv', '1FQY'), None)
expect(main('aquaporin_test1.csv', '1LDA'), None)
expect(main('aquaporin_test2.csv', '2B5F'), None)
summary()


start_testing()
# Examples and tests for read
expect(read('aquaporin_empty.csv'), [])
expect(read('aquaporin_test1.csv'), [ProteinInfo('1FQY', 269, 'MASEFKKKLFWRAVVAEFLATTLFVFISIGSALGFKYPVGNNQTAVQDNVKVSLAFGLSIATLAQSVGHISGAHLNPAVTLGLLLSCQISIFRALMYIIAQCVGAIVATAILSGITSSLTGNSLGRNDLADGVNSGQGLGIEIIGTLQLVLCVLATTDRRRRDLGGSAPLAIGLSVALGHLLAIDYTGCGINPARSFGSAVITHNFSNHWIFWVGPFIGGALAVLIYDFILAPRSSDLTDRVKVWTSGQVEEYDLDADDINSRVEMKPK'),
                                     ProteinInfo('1LDA', 281, 'MSQTSTLKGQCIAEFLGTGLLIFFGVGCVAALKVAGASFGQWEISVIWGLGVAMAIYLTAGVSGAHLNPAVTIALWLFACFDKRKVIPFIVSQVAGAFCAAALVYGLYYNLFFDFEQTHHIVRGSVESVDLAGTFSTYPNPHINFVQAFAVEMVITAILMGLILALTDDGNGVPRGPLAPLLIGLLIAVIGASMGPLTGFAMNPARDFGPKVFAWLAGWGNVAFTGGRDIPYFLVPLFGPIVGAIVGAFAYRKLIGRHLPCDICVVEEKETTTPSEQKASL'),
                                    ProteinInfo('1LDF', 281, 'MSQTSTLKGQCIAEFLGTGLLIFFGVGCVAALKVAGASFGQWEISVIFGLGVAMAIYLTAGVSGAHLNPAVTIALWLFACFDKRKVIPFIVSQVAGAFCAAALVYGLYYNLFFDFEQTHHIVRGSVESVDLAGTFSTYPNPHINFVQAFAVEMVITAILMGLILALTDDGNGVPRGPLAPLLIGLLIAVIGASMGPLTGTAMNPARDFGPKVFAWLAGWGNVAFTGGRDIPYFLVPLFGPIVGAIVGAFAYRKLIGRHLPCDICVVEEKETTTPSEQKASL')])
expect(read('aquaporin_test2.csv'), [ProteinInfo('2ABM', 1848, 'MFRKLAAECFGTFWLVFGGCGSAVLAAGFPELGIGFAGVALAFGLTVLTMAFAVGHISGGHFNPAVTIGLWAGGRFPAKEVVGYVIAQVVGGIVAAALLYLIASGKTGFDAAASGFASNGYGEHSPGGYSMLSALVVELVLSAGFLLVIHGATDKFAPAGFAPIAIGLALTLIHLISIPVTNTSVNPARSTAVAIFQGGWALEQLWFFWVVPIVGGIIGGLIYRTLLEKRD'),
                                     ProteinInfo('2B5F', 1212, 'MSKEVSEEAQAHQHGKDYVDPPPAPFFDLGELKLWSFWRAAIAEFIATLLFLYITVATVIGHSKETVVCGSVGLLGIAWAFGGMIFVLVYCTAGISGGHINPAVTFGLFLARKVSLLRALVYMIAQCLGAICGVGLVKAFMKGPYNQFGGGANSVALGYNKGTALGAEIIGTFVLVYTVFSATDPKRSARDSHVPILAPLPIGFAVFMVHLATIPITGTGINPARSFGAAVIFNSNKVWDDQWIFWVGPFIGAAVAAAYHQYVLRAAAIKALGSFRSNPTNLEQKLISEEDLNSAVDHHHHHH'),
                                     ProteinInfo('2B6O', 263, 'MWELRSASFWRAIFAEFFATLFYVFFGLGASLRWAPGPLHVLQVALAFGLALATLVQAVGHISGAHVNPAVTFAFLVGSQMSLLRAICYVVAQLLGAVAGAAVLYSVTPPAVRGNLALNTLHPGVSVGQATIVEIFLTLQFVLCIFATYDERRNGRLGSVALAVGFSLTLGHLFGMYYTGAGMNPARSFAPAILTRNFTNHWVYWVGPVIGAGLGSLLYDFLLFPRLKSVSERLSILKGTRPSESNGQPEVTGEPVELKTQAL'),
                                     ProteinInfo('2B6P', 263, 'MWELRSASFWRAICAEFFASLFYVFFGLGASLRWAPGPLHVLQVALAFGLALATLVQAVGHISGAHVNPAVTFAFLVGSQMSLLRAICYMVAQLLGAVAGAAVLYSVTPPAVRGNLALNTLHPGVSVGQATIVEIFLTLQFVLCIFATYDERRNGRLGSVALAVGFSLTLGHLFGMYYTGAGMNPARSFAPAILTRNFTNHWVYWVGPVIGAGLGSLLYDFLLFPRLKSVSERLSILKGSRPSESNGQPEVTGEPVELKTQAL')])
summary()

start_testing()
# Examples and tests for analyze 
expect(hydro_plot(LOPI0, '1FQY'), None)
expect(hydro_plot(LOPI3, '2D57'), None)
summary()

start_testing()
# Examples and tests for is_protein
expect(is_protein(PI1, '1FQY'), True)
expect(is_protein(PI2, '1FQY'), False)
summary()

start_testing()
# Examples and tests for list_of_aa_str
expect(list_of_aa_str(LOPI0, '1FQY'), [])
expect(list_of_aa_str(LOPI1, '1FQY'), ['M','A','S','E','F'])
expect(list_of_aa_str(LOPI2, '1LDA'), ['M','S','Q','T','S'])
summary()

start_testing()
# Examples and tests for str_to_enum
expect(str_to_enum('I'), AminoAcid.I)
expect(str_to_enum('V'), AminoAcid.V)
expect(str_to_enum('L'), AminoAcid.L)
expect(str_to_enum('F'), AminoAcid.F)
expect(str_to_enum('C'), AminoAcid.C)
expect(str_to_enum('M'), AminoAcid.M)
expect(str_to_enum('A'), AminoAcid.A)
expect(str_to_enum('G'), AminoAcid.G)
expect(str_to_enum('T'), AminoAcid.T)
expect(str_to_enum('S'), AminoAcid.S)
expect(str_to_enum('W'), AminoAcid.W)
expect(str_to_enum('Y'), AminoAcid.Y)
expect(str_to_enum('P'), AminoAcid.P)
expect(str_to_enum('H'), AminoAcid.H)
expect(str_to_enum('E'), AminoAcid.E)
expect(str_to_enum('Q'), AminoAcid.Q)
expect(str_to_enum('D'), AminoAcid.D)
expect(str_to_enum('N'), AminoAcid.N)
expect(str_to_enum('K'), AminoAcid.K)
expect(str_to_enum('R'), AminoAcid.R)
summary()

start_testing()
# Examples and tests for list_of_aa_enum
expect(list_of_aa_enum([]), [])
expect(list_of_aa_enum(['I','V','L','F','C']), [AminoAcid.I, 
                                                AminoAcid.V, 
                                                AminoAcid.L,
                                                AminoAcid.F,
                                                AminoAcid.C])
expect(list_of_aa_enum(['M','A','G','T','S']), [AminoAcid.M, 
                                                AminoAcid.A, 
                                                AminoAcid.G,
                                                AminoAcid.T,
                                                AminoAcid.S])
expect(list_of_aa_enum(['W','Y','P','H','E']), [AminoAcid.W, 
                                                AminoAcid.Y, 
                                                AminoAcid.P,
                                                AminoAcid.H,
                                                AminoAcid.E])
expect(list_of_aa_enum(['Q','D','N','K','R']), [AminoAcid.Q, 
                                                AminoAcid.D, 
                                                AminoAcid.N,
                                                AminoAcid.K,
                                                AminoAcid.R])
summary()


start_testing()
# Examples and tests for hydro_index
expect(hydro_index(AminoAcid.I), 4.5)
expect(hydro_index(AminoAcid.V), 4.2)
expect(hydro_index(AminoAcid.L), 3.8)
expect(hydro_index(AminoAcid.F), 2.8)
expect(hydro_index(AminoAcid.C), 2.5)
expect(hydro_index(AminoAcid.M), 1.9)
expect(hydro_index(AminoAcid.A), 1.8)
expect(hydro_index(AminoAcid.G), -0.4)
expect(hydro_index(AminoAcid.T), -0.7)
expect(hydro_index(AminoAcid.S), -0.8)
expect(hydro_index(AminoAcid.W), -0.9)
expect(hydro_index(AminoAcid.Y), -1.3)
expect(hydro_index(AminoAcid.P), -1.6)
expect(hydro_index(AminoAcid.H), -3.2)
expect(hydro_index(AminoAcid.E), -3.5)
expect(hydro_index(AminoAcid.Q), -3.5)
expect(hydro_index(AminoAcid.D), -3.5)
expect(hydro_index(AminoAcid.N), -3.5)
expect(hydro_index(AminoAcid.K), -3.9)
expect(hydro_index(AminoAcid.R), -4.5)
summary()

start_testing()
# Examples and tests for list_of_hydro_index
expect(list_of_hydro_index([]), [])
expect(list_of_hydro_index([AminoAcid.M, AminoAcid.A]), [1.9, 1.8])
expect(list_of_hydro_index([AminoAcid.M, AminoAcid.R, AminoAcid.M]), [1.9, -4.5, 1.9])
summary()

start_testing()
# Examples and tests for get_hydro_score
expect(get_hydro_score([]), [])
expect(get_hydro_score([4.5, 4.2, -0.4, -0.7, 2.8, 1.9, 3.8, 2.8, 2.5, 1.9, 2.5, 3.8]), 
                           [2.3, 2.1, 2.1])
expect(get_hydro_score([1.9, 1.8, 2.5, 3.8, -3.5, -0.4, -0.7, 1.3, 2.8, 4.2, -3.5, -0.9, 2.5, 3.8]), 
                           [1.4, 0.8, 0.6, 0.6, 0.6])
summary()

start_testing()
# Examples and tests for residue
expect(residue(LOPI0, '1FQY'), 0)
expect(residue(LOPI3, '2D57'), 3)
summary()

start_testing()
# Examples and tests for get_residue
expect(get_residue(0), [])
expect(get_residue(5), [0, 1, 2, 3, 4])
expect(get_residue(12), [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11])
summary()

start_testing()
# Examples and tets for get_thresh
expect(get_thresh(0), [])
expect(get_thresh(12), [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0])

summary()
```

## Creating Hydropathy Plots
To create a hydropathy plot for any protein in the csv file
Use the following command.

```
main('aquaporin.csv', '2B6O')
```
Remember to replace aquaporin with the name of your csv and replace 2B60 with the protein ID of the desired protein.
