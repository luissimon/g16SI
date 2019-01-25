# g16SI

Script to create supporting information documents with cartesian coordinates from gaussian output files.

This script is intended to automatize the boring and laborious task of creating supporting or supplementary information (SI, ESI, etc) files for computational chemistry. It will create a .doc file including a title (an arbitrary text that can be edited) and a two-column section including the energy, the imaginary frequency, and the cartesian coordinates of each structure. Optionally, a pymol-rendered image of the structure will also be added. This image will also be generated automatically, showing, if required, an “optimal” orientation.

The script is able to work with many (hundreds) of structures, but it only takes two files: one of the files containing frequency information and other other with the energy information for the same structure. Then, it compares the names of the two files to determine what do they have in common, and the prefix, suffix, route, etc. that characterizes all the files with frequency information and all the files with single point information. The possibility of using two files covers the very common case in which different level of theory is used in single point energy evaluation and geometry optimization/frequency calculation, but if a single file is specified, it will be used to extract both. 

The script requires the library docx. If image rendering is requested, it also requires numpy, scipy and pymol installed. The environmental variable PYTHONPATH must include the path to the directory where pymol/cmd.py lives or the script will not be able to import the pymol libraries. Mac users: macpymol can not be used. Instead, pymol must be installed (e.g. by homebrew, etc).

The frequency and energy output file names have the form:
                                                                           prefix_for_freq+NAME+suffix_for_freq.ext         
                                                                               prefix_for_sp+NAME+suffix_for_sp.ext         

- “ext” is the extension of the files. It could be anything (usually: “.out”, “.log”), but FILES SHARING THE SAME NAME BUT DIFFERENT EXTENSION (e.g., input files, etc) MUST NOT BE PRESENT in the same folder.

- Files with frequency information and with energy information for a single structure MUST SHARE the same NAME. It will be used as the title of each structure in the final .doc file.

- All files with frequency information MUST SHARE prefix_for_freq and suffix_for_freq; all files with energy information MUST SHARE prefix_for_sp and suffix_for_sp.

- All files with frequency information MUST be in the same directory; all files with energy information MUST be in the same directory. All files CAN be in a single directory.

- Any of: prefix_for_freq, suffix_for_freq, prefix_for_sp, suffix_for_sp could be nothing (if the four are nothing, frequency files and energy files must be in different directories, but the operating system will force this anyway).

- If the same file (name and path) is specified for frequency and energy information, a single file will be used for both (similar behaviour as if only one is specified).



Usage:      g16SI.py  -keywords 

Keywords:

-energy_file [file]: the gaussian output file containing energy information, including the path (“./” or “~/” is also allowed).

-freq_file [file]: the gaussian output file containing frequency information, including the path (“./” or “~/” is also allowed).
If only one of “-energy_file” or “-freq_file” is specified, it will be used for both. If none is specified, the script will stop.

-doc_file [file]: name of the .doc file that will be created. If not specified, the file “SI.doc” will be created.

-q: the script pairs up frequency files and energy files and then prompt if the assignment is correct. If -q is specified, correctness will be assumed without prompt.

-incl_pict: if it included, a picture of the structure, rendered by pymol, will be added.

-pict_res_x [int] and -pict_res_y [int]: number of pixels in the x and y dimensions of the rendered images. Defaults  are 800 points for both.

-pict_size [int]: size (in cm) of the picture included in the .doc file. Default is 7cm, which is good for 2-column documents.

-optz_pict [option]: optimize the orientation of the structure during image rendering using the criteria in [option] (see below). if it is not given the structure will not be optimized.

-keep_files: if included, the files generated for rendering images (NAME.png and NAME.pdb) will not be deleted.


Options for optimizing the orientation of the structure during image rendering: 

The criteria used for the optimization is one of: 
      “prevent_eclipsing”: projects the atoms in the XY plane and attempts to maximize their distances.
      “eclipse”: the opposite. 
      “span”: projects the atoms in the XY plane and attempts to maximize the sum of their distances to the center.

The atoms on which these criteria is applied can also be specified: _notH (non-H atoms), _HL (atoms in the high-level layer in ONIOM calculations), _:x-y (involves atoms with numbers x and y). If nothing is specified, all atoms will be used. 
Examples:
                   prevent_eclipsing  : tries to increase the distances between all atoms after they are projected in the XY plane.
                   span_HLnotH: tries to expand all non-H atoms in the High Level layer.
                   eclipse_:5-11             searches an orientation in which 5 and 11 are eclipsed.
                   span_:5-11,4-8,3-7        searches an orientation in wich the bonds 5-11, 4-8, and 3-7 are shown longer.





