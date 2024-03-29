# Domain Auto Finder
The program DAFi is a useful tool for the analysis of single-crystal X-ray diffraction data from polycrystalline samples, as its algorithm finds subsets of reflections in the reciprocal space belonging to the distinct crystallites.

## License and Citation
This repository can be used only for personal/research/non-commercial purposes. For commercial requests, please contact us directly at matvii.aslandukov@gmail.com or andrii.aslandukov@uni-bayreuth.de. Please cite the following paper if you use this repository:

```
Aslandukov, A., Aslandukov, M., Dubrovinskaia, N. & Dubrovinsky, L. (2022). 
J. Appl. Cryst. 55, 1383-1391, https://doi.org/10.1107/S1600576722008081
```

## Installation
1. Download and unzip the latest release from the [releases page](https://github.com/AsMaNick/Domain-Auto-Finder/releases).
2. Unzipped data should consist of 4 files: an executable file (`main.exe`), a configuration file (`config.txt`), and two examples of input data described in chapter 3 of the paper (`example1_BasaltRock.tabbin`, `example2_Y5N14.tabbin`).
3. Validate your installation by executing the `main.exe` file: it should process the example data file (the whole execution can take about 20 seconds). Although this program might also work on other Windows variants, we have only tested it on Windows 10 64-bit (x64).

## Process your own dataset
1. Prepare `.tabbin` file from your experiment using CrysAlis<sup>Pro</sup> software.
2. Specify the path to the `.tabbin` filename in the `config.txt` file as well as other necessary parameters.
3. Execute the `main.exe` program. Be careful, after the execution the original `.tabbin` file will be overwritten (if you don't want to lose the original `.tabbin` file, use its copy).
4. Return back to the CrysAlis<sup>Pro</sup> software and continue working with a labeled dataset.

## Setupping the configuration file
### General parameters
- `input_data_directory` - string; a path to the directory where `.tabbin` file is located.
- `filename` - string; name of the input file to be processed.
- `groups_per_tabbin` - integer; maximum number of groups that can be displayed in one `.tabbin` file.
- `number_of_threads` - integer; number of threads to use; corresponds to the first speed optimization in the section 2.5; recommended to set equal to the number of processor cores.
- `groups_to_find` - integer; total number of groups to find.
- `groups_to_find_in_one_iteration` - integer; corresponds to the third speed optimization in the section 2.5; bigger values speed up the program however may decrease the quality.
- `minimum_line_size` - integer from 3 to 5, recommended value = 4; a threshold for minimum row size to be included in the group; bigger values result in more precise groups but with a smaller size.
- `extend_best_group_with_small_lines` - boolean; if true, decreases `minimum_line_size` parameter by one but only for the best found direction.

### First stage of the algorithm
- `common_directions_method` - string, one of the two options: "naive" or "smart"; "naive" corresponds to the naive approach described in the section 2.3.1, "smart" corresponds to the smart approach described in the section 2.3.2.
- `eps_coordinates1` - real from 0.0001 to 0.02; used only by "smart" method in the internal phase of clusterizing the direction vectors; corresponds to the $\varepsilon_1$ defined in the section 2.3.1.
- renormalization_period - integer; corresponds to the second speed optimization in the section 2.5; number of iterations after which points' renormalization and recalculation of direction distribution is necessary; for the `common_directions_method = naive` recommended value is 1000 while for the smart method, which doesn't allow precise distribution recalculation, recommended value is in range from 5 to 20.

### Second stage of the algorithm
- `directions_to_consider` - integer; corresponds to the $N_{dirs}$ defined in the section 2.3; number of best directions to process during the second stage of the algorithm.
- `eps_coordinates2` - real from 0.0001 to 0.02, recommended value = 0.002; epsilon for comparing points projections.
- `eps_length` - real from 0.0001 to 0.2, recommended value = 0.02; corresponds to the $\varepsilon_2$ defined in the sections 2.3.2 and 2.4; epsilon for comparing distances between points on one line.

### Optional parameters
- `pause_at_the_end` (supported from v2.1.5) - boolean; if true, waits until the user presses any key before terminating the program; true by default. 
- `exec_command_after_completing_tabbin` - string; executes external command after finishing job with a `.tabbin` file: "command path_to_tabbin_file group_id1 group_id2 ... group_idn"; can be used for calling external programs for further processing; empty for no execution; empty by default.
- `extend_manually_found_lines` - integer; if you want to extend manually found rows, specify group id in which corresponding rows are present; 0 for search from scratch; 0 by default.
- `random_angle_seed` - integer; random seed for rotation of all points by random angle; 0 for no rotation; 0 by default.
- `data_format` (supported from v2.2.1) - string, one of the three options: "CrysAlisPro", "APEX4" or "Text"; by default data format is guessed based on the extension of the `filename`: `.tabbin` corresponds to the "CrysAlisPro" data format, `.p4p` - to the "APEX4" and `.txt` - to the "Text".
