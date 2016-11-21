# Coin 

A description for an adhoc version control system where each file maintains its own a repository.

*(Draft version)*

# Introduction

In this document I describe an adhoc approach for a version control system that tracks a single file instead of a set of files. The version control system maintains a repository for each file. If there are five files then there will be five repositories. Each repository is its own thing. The version control system doesn't maintain files as sets; rather it maintains a repository for each file. I started this for exercise. Resolving problems and rebuilding existing things can be a great way to learn. I want to solve and build a simple version control system.


# Concepts

* *Content* refers to the content of a file.

* The *source file* is the file that is submitted to the revision control system to be commited.

* A *repository* is a data structure that describes the revision history of a source file. A repository contains the __original__, __revision patches__, and metadata about the __source file__.

* The *repository file* contains the repository data structure. The file is maintained by the revision control system.

* The *original* is the first __content__ ever submitted to the revision control system. The original is stored verbatim as raw content upon submission to the revision control system. The original serves an important role as the content from which all revisions are derived during reconstruction.

* *Data differencing* (or simply *differencing*) is the operation of finding the __difference__ between two values. On a larger scale, data differencing helps to find differences between two __content__. It plays its role by helping to reduce storage space when it is used to track individual character changes between __commits__.

* A *difference* (or *delta*) is given from differencing a source character and a target character. A difference may be expressed as `a - b`.

* *`delta(a,b)`* is a function used to find the difference. The function accepts two character arguments and finds the differences between them and returns the result. `delta(a,b)` calculates `a` minus `b` where `a = last_revision` and `b = original`.

* A *source* refers to __content__ which is patched up to produce a __target__ during the reconstruction process.

* A *target* refers to a reconstructed form of a selected revision. A target is produced from reconstructing a __source__ and with __patches__.

* An *update* is new __content__ to be submitted to the revision control system.

* The *head* refers to the most recent __revision__, and also refers to the last __patch list__.
        
* A *revision* refers to any submitted changes to the file. After an update is accepted and processed by the revision control system, the changes to the __content__ of the __source file__ is referred to as a __revision__. A revision maintains one __patch list__ to keep the changes made to that revision.

* A *revision number* is an identification number that identifies a __revision__. Each revision is assigned a revision number. From a technical aspect, a revision number is assigned to the __patch list__ of a __revision__.
  
* A *patch* is a structure of information that represents the __differences__ found between the __head__ and an __update__.

* A *patch list* represents a group of patches for a revision.
    
    
# Design

## Data differencing

To record changes, I employ the usage of data differencing. Data differencing involves taking two values and finding the difference between them. The difference between the two values determine whether there was change made to the content or not. If the difference between the two values is zero, then no change was made. However, if the difference is a non-zero, then there was a change.

To find changes made to content using data differencing, two strings are compared character by character to find the differencing between each character of the two strings. The corresponding characters are differenced, and if the result is a non-zero, the difference is recorded along with the index location of the compared characters within the string and is stored as a patch.

## Patching

Patches are produced when the version control system processes an update. When an update is submitted, it is differenced against the head. The differences found between the head and update are stored as patches in a patch list.

A patch is used to "patch up" a revision during the reconstruction process. A patch contains two key pieces of information that are needed for reconstruction: 1) The calculated difference between two corresponding source characters; and 2) the index location of the corresponding characters. Patches are represented as patch encodings. A *patch encoding* is a structure that consists of all needed key pieces of information. A patch encoding is written as follows:

```
diff@index
```
        
### Example of a patch

```  
History:   Hello -> Hallo
Index:     0 1 2 3 4
Original:  H e l l o
Revision:  H a l l o
Patch:     -4@1
```
            
The revision control system never saves the revised text "Hallo" in its entirety. Only the individual changes were saved as a patches. The patch for this revision is encoded as `-4@1`. See, the integer value of the ASCII character `e` is `101`, and the integer value for `a` is `97`. The difference between 101 and 97 when expressed as `delta(97,101)` is `-4`.
  

## Reconstruction (what reconstruction is)
        
Reconstruction is a process of producing a target. A target is produced by iterative passes through each revision. The iterative-pass process begins with the original, where the original is copied as a source. Using the patches for the next revision, a target is produced. The target is then treated as the source for the next pass. This iteration continues until the desired target is produced.

### Example of the reconstruction process
        
In the following example, Tupac Shakur titles his new unreleased album. But unsure of what the album's title should be, he occasionally changes its name to one he finds more suitable. He starts with "Bedmen", but soon realizes that he made a typographical error. He corrects his mistake by changing the lowercased e's to lowercased a's, resulting in the title "Badman". However, having a fetish for words with lowercased letters, he adjusts the album's title to suit this preferred typographical style, and gets the title "badman". Then without rational reasoning he changes it once more, finally settling with the title, "bad".

  Bedmen -> Badman -> badman -> bad
    
In the following table, the 2Pac's title changes are listed as revisions and the actual character changes are listed as patches.

```
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
```

Reconstruction revision 3 starting from original:

```
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
```

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
