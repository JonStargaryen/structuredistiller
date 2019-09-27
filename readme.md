### StructureDistiller
StructureDistiller is an algorithm which allows to quantify the structural relevance of individual 
contacts and residues in protein structures. It takes protein structures (in PDB format) as input, transforms it into a 
contact map, quantifies the relevance of each contact on reconstruction fidelity (i.e. how much does a reconstruction 
improve if that particular contact is known), and outputs the results in a concise format.
 
Run it with:

    java -jar distiller.jar <confoldPath> <tmalignPath> <workingDirectory> <threads> [baselineFrequency - optional, default: 0.3]

* `confoldPath` - path to [CONFOLD](https://github.com/multicom-toolbox/CONFOLD) - see page for DSSP 
and CNS installation
* `tmalignPath` - path to [TM-align](https://zhanglab.ccmb.med.umich.edu/TM-align/)
* `workingDirectory` - all files in PDB format in this directory will be processed, output will be 
written here
* `threads` - how many threads to use
* `baselineFrequency` - optional argument: how many entries of the native contact map should be used to compute the 
baseline reconstructs - specify this value if you determined that the default value of `0.3` is not the best choice for 
a particular protein

Output is in the tab-separated format:

    (24,68) false 2.4960 0.6408 0.0433 2.5540 0.6337 0.0433 -0.0580 -0.0072 0.0000
    
* 1st column: contact identifier, here between residue number 24 and 68
* 2nd: was the contact removed from the corresponding contact map
* 3rd: RMSD of baseline reconstruct
* 4th: TM-score of baseline reconstruct
* 5th: fraction of native contacts in baseline reconstruct
* 6th: average RMSD of reconstructs
* 7th: average TM-score of reconstructs
* 8th: average fraction of native contacts in reconstructs
* **9th: average decrease in RMSD for contact**
* 10th: average increase in TM-score for contact
* 11th: average increase in fraction of native contacts for contact

Be sure to test the installation of the dependencies beforehand.

### Known Problems & Limitations
* Use single chain, single domain structures as input. Structures should be small (i.e. up to 200 residues) to avoid 
problems.
* The TM-align implementation (at least that used by me) seems to leak file descriptors which may cause an `IOException` 
at some point because no new processes can be spawned. You can avoid the error “Too many open files (24)” by increasing
the limit of file descriptors in the OS. I could not recreate or fix this, but especially large proteins with long 
computations are prone to this exception.

### Source Code
This project just wraps the underlying source code for release. Please find the implementation 
[here](https://github.com/JonStargaryen/jstructure). The implementation is a proof-of-concept and surely has room to
improve. Feel free to contact me, if you need any support.