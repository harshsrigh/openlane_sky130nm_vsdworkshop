# OpenLANE Workshop

This repository used to document the progress of the 5-days workshop on Physical design using OpenLANE flow(an orchestra of open source tools put together to build/create an open source community of digital and analog IP's).

## Day Progress & Learnings

During the workshop, most of the labs are performed using a pre-coded picorv32a project.

## Day 1: Introduction to OpenLANE Flow

### Lab Work

#### Invoking OpenLANE

1. Invoking the openLANE using `./flow.tcl -interactive`.
   The chip design is an iterative process, hence interactive mode offers better control and flexibility.

2. Load required dependencies of openLANE using following Command.
   `package require openlane 0.9`

   ![Invoking OpenLANE Image](./images/package.PNG)

#### Preparing Design

All the designs are kept at the `/openlane_working_dir/designs`. For the workshop, 'picorv32a' design has been used.

The command used for setting up the design with 'LEF' and 'TECH' file. The 'LEF' file format contains information related to the terminals, different layers, dimensions etc, where Tech files contains layer definition and design rule checks(DRC).

Set 'LIB_SYNTH' variable to the `./openlane_dir/pdks/sky130A/libs.ref/sky130_fd_sc_hd/lib/sky130_fd_sc_hd_tt_025C_1v80.lib`. This liberty file contains Standard cell characteristics for 25 Deg Celsius Temperature and 1.8V. Add this variable in the config.tcl file present in the picorv32a folder.             
![Preparation Image](./images/prep.png "Setting-up Design")             

#### Running Synthesis
Use command `run_synthesis`                                                                                               
![run synthesis Image](./images/run_synth.PNG "Running for synthesis")                                                                      
File resulted from the synthesis                                                                           
![run synthesis Image](./images/gen_synth.PNG "File resulted from the synthesis")

## Day 2: Floorplanning and Placement

### Floorplanning

Floorplanning includes placement of I/O pads and macros as well as power planning(Note: OpenLANE flow allows Power Planning post CTS).

#### Floorplan Control Parameters

1. Aspect Ratio: This determine the shape and size of the chip. It is defined as ratio of height to width.
   Note: Aspect Ratio 1 means shape is square.

2. Core Utilization: Core Utilization defined as the total area occupied by the netlist to the total area of the core.
   if Core Utilization is 0.6 then it means 40% of the area is going to be used for routing purposes.

For more control parameter refer to [openLANE configuration Readme](https://github.com/efabless/openlane/blob/master/configuration/README.md)

#### Decoupling Capacitor

#### Importance of Power Planning

#### Executing Floorplanning on openLANE

Run Command `run_floorplan` after adjusting proper control parameters in config.tcl.

Command generates 'DEF' file in the folder `/designs/picorv32a/run/<setup-folder>/results/floorplan/`
![run floorplan Image](./images/floorplane_folder.PNG "Floorplan file")

***Viewing 'DEF' file using Magic***
Magic requires three files to view Picorv32a DEF file:
   1. Magic Tech file: sky130A.tech
   2. LEF File: Merged.lef
   3. DEF File: Any Def file generated using this LEF file
Command to run magic:
`magic -T <Tech File> lef read <LEF File> def read <DEF File> &`
Floorplan result:                                                      
![floorplan Image](./images/floorplane.PNG "Floorplan result")

#### Executing Placement on openLANE

After Floorplan, next step(in physical design) is the placement stage. Standard cells has been mapped as per the synthesized netlist and their standard rows are determined by the floorplan.

Placement in OpenLANE happens in two stages. First stage is the Global placement in which placements of the cells are not legalized. Followed by Detailed Placement in which proper legalization is considered.

***Run Placement Command:*** `run_placement`                                                                                    
***Placement Result:***                                        
![Placement Result](./images/placement_result.PNG "Legalization Pass result")                          
Placement result on Magic:                                                                           
![Placement Image](./images/placement_image.PNG "Placement result on Magic")                 
