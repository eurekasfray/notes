# Coin 

A description for an adhoc version control system where each file maintains its own a repository.

*(Draft version)*

# Introduction

In this document I describe an adhoc approach for a version control system tracks a single file instead of a set of files. The version control system maintains a repository for each file. If there are five files then there will be five repositories. Each repository is its own thing. The version control system doesn't maintain files as sets; rather it maintains a repository for each file. I started this for exercise. Resolving problems and rebuilding existing things can be a great way to learn. I want to solve and build a simple version control system.

# Concepts


* *Content* refers to any file content.

* The *source file* is the file that is submitted to the revision control system to be commited.

* A *repository* is a data structure that describes the revision history of a source file. A repository contains the original, revision patches, and metadata about the source file.

* The *repository file* contains the repository data structure. The file is maintained by the revision control system.

* The *original* is the initial content submitted to the revision control system. The original is stored verbatim as the raw content of the file upon its first submission to the revision control system. The original serves an important role as the content from which all revisions are derived during reconstruction.

* *Data differencing* (or simply *differencing*) is the operation of finding the difference between two values. Data differencing helps to find differences between two content and reduces storage space by tracking individual character changes between commits.

* A *difference* (or *delta*) is given from differencing a source character and a target character. A difference may be expressed as `a - b`.

* *`delta(a,b)`* is a function used to find the difference. The function accepts two character arguments and finds the differences between them and returns the result.

* A *source* refers to content which is patched up to produce a target during the reconstruction process.

* A *target* refers to a reconstructed form of a selected revision. A target is produced from reconstructing a source and with patches

* An *update* is new content submitted to the revision control system.

* The *head* refers to the most recent revision, and also refers to the last patch list.
        
* A *revision* refers to any submitted change to the file. After an update is accepted and processed by the revision control system, each change to the file is referred to as a revision.

* A *revision number* is an identification number. Each revision is assigned a revision number. From a technical aspect, a revision number is assigned to the patch list of a revision.
  
* A *patch* is a structure of information that represents the difference found between the head and an update. A patch is produced from a difference between the head and an update. When an update is submitted, it is differenced against the head. The differences found are stored as patches.
    
  As its name describes, a patch is used to "patch up" a revision during the reconstruction process. A patch contains two key pieces of information that are needed for reconstruction: The calculated difference between two corresponding source characters; and the index location of the corresponding characters. Patches are represented as patch encodings. A patch encoding is a structure that consists of all needed key pieces of information. A comprehensive summary of a patch encoding is shown in the following:
    
  Patch notation format

  * `diff@index`
            
  Example
  
      History:   Hello -> Hallo
      Index:     0 1 2 3 4
      Original:  H e l l o
      Revision:  H a l l o
      Patch:     -4@1
            
  Meaning
  
  * The revision control system never saves the revised text "Hallo" in its entirety. Only the individual changes was saved as a patches. The patch for this revision is encoded as `-4@1`. See, the integer value of the ASCII character `e` is <code>101</code>, and the integer value for `a` is `97`. The difference between 101 and 97 when expressed as `delta(97,101)` (which calculates last_revision minus original) is `-4`.
        
# Design

To record changes made to content, the method designed for revision control management employs the usage of data differencing. Data differencing involves the process of taking two values and finding the difference between them. It is the different between the values that determine whether there was change made to content and if there was not changed. If the difference between the two values is zero, then there was no change made. However, if the difference is a non-zero, then there is a change. Using data differencing, two strings are compared character by character to find differencing between each character of the two strings. Where there are changes, the values are differenced and if the result is a non-zero, the difference is recorded along with the location of the value within the string and is stored as a patch.

## Patch list

A patch list represents a group of patches of a revision.

## Reconstruction (what reconstruction is)
        
Reconstruction is a process of producing a target. A target is produced by iterative passes through each revision. The iterative-pass process begins with the original, where the original is copied as a source. Using the patches for the next revision, a target is produced. The target is then treated as the source for the next pass. This iteration continues until the desired target is produced.

### Example of the reconstruction process
        
In the following example, Tupac Shakur titles his new unreleased album. But unsure of what the album's title should be, he occasionally changes its name to one he finds more suitable. He starts with "Bedmen", but soon realizes that he made a typographical error. He corrects his mistake by changing the lowercased e's to lowercased a's, resulting in the title "Badman". However, having a fetish for words with lowercased letters, he adjusts the album's title to suit this preferred typographical style, and gets the title "badman". Then without rational reasoning he changes it once more, finally settling with the title, "bad".

  Bedmen -> Badman -> badman -> bad
    
In the following table, the 2Pac's title changes are listed as revisions and the actual character changes are listed as patches.

  ------  --------  ---------------------
  Label   Revision  Patch
  ------  --------  ---------------------
  O:      Bedmen
  R1/P1:  Badman    -4@1, -4@4
  R2/P2:  badman    32@0
  R3/P3:  bad       -109@3, -91@4, -110@5
  ------  --------  ---------------------
  
  Abbreviations:
  O - Original
  R - Revision
  P - Patch

Reconstruction revision 3 starting from original:

    -----------------------------------------
    0       1       2       3       4       5
    -----------------------------------------
    B       e       d       m       e       n       Original
    |       |               |       |       |
    |    +(-4)              |    +(-4)      |       P1 (Patch 1)
    |                       |       |       |
  (-32)                     |       |       |       P2
                            |       |       |
                         +(-109) +(-97)  +(-110)    P3
    b       a       d
    -----------------------------------------
    0       1       2       3       4       5
    -----------------------------------------

### Target reconstruction algorithm
        
The reconstruction of a target involves the process of walking through each revision and patching it up until the desired target is derived.

The following is an algorithm for reconstructing any given revision by using the original and deriving the target revision by the use of patches. The algorithm takes in as input the desired revision to which a derivation must be reconstructed from only using the differences calculated when the data was changed. As output, the algorithm provides the derived target.

The Working of the Algorithm: The algorithm retrieves the original, and copies it as the source, because the reconstruction process must start somewhere&mdash;and where best to start but at the original? Each patching up of the source to create the target is called as a pass. So with that definition, it can be said that for each revision, the source is patched up to create the target; then, that the patched up&mdash;or in other words, reconstructed&mdash;target is used as the source for the next patching pass.

* Get the **revision number** of **target**
* Store it as **target revision number** 
* Get the **original**
* Store it as **source**
* Get all revisions associated with the **original** in ascending order (from oldest to newest)
* For each revision do:
  * Get **patch list** associated with the current revision in ascending order (from first to last)
  * For each **patch encoding** in the current **patch list** do:
    * Extract the difference from the current patch encoding and store it as **diff**
    * Extract the index from the current **patch encoding** and store it as **diff index**
    * It's time to construct the target, so:
      * Copy the **source** and store it as the **target**, because we need to begin patching the source in order to derive the target.
      * If length of **source** is less than or equal to the **diff index** then
        * In the **target** string, set the value at the **diff index** equal to the result of the equation (**source[diff_index]** + **diff**)
      * Else, if the **diff_index** is greater than the length of **source**
        * Calculate the expression: (0 + **diff_index**)
        * Appended the result to **target**
      * End if
    * Hand **target** off to the next pass: Copy the **target** and set it as the **source**
  * End foreach
  * If revision number of current revision is equal to the revision number of the target, then the desired target has been reached:
    * Break away from this loop
  * End if
* End foreach
* Return **target**
