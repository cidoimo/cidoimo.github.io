---
layout: page
title: MD Simulation of Ubiquitine with AMBER
description: A simple tutorial to start your career in Molecular Dynamics
img: assets/img/ubi.gif
importance: 1
category: work
related_publications: false
---
<p style="text-align: justify;">
This tutorial aims to provide a comprehensive introduction to classical molecular dynamics simulations using the Amber software package. Amber is a widely used tool for simulating biomolecular systems at the atomic level, enabling researchers to investigate a wide range of phenomena, including protein folding, ligand-protein interactions, and enzyme catalysis.<br>
This tutorial focuses on a small protein, Ubiquitin, which will be simulated for a time of 100 ns. Ubiquitin is a well-studied protein involved in various cellular processes, making it a suitable example for learning molecular dynamics simulations using Amber 2022.</p>
<br>
<hr>
<br>

<p style="text-align: justify;">
<b style="font-size: 20px;">Step 1</b>: retrieve the structure<br><br>
The Protein Data Bank (PDB) is a repository of three-dimensional structures of macromolecules, such as proteins and nucleic acids. To download the ubiquitin protein structure (PDB ID: <b>1UBI</b>), follow these steps:

<ul>
<li>Access the PDB website: open a web browser and navigate to the PDB <a href="https://www.rcsb.org/">website</a>;</li>
<li>Search for the ubiquitin structure;</li>
<li>Download the PDB file: on the structure page, locate the "Download" section. Click on the "PDB" option to download the PDB file for the ubiquitin structure;</li>
<li>Save the PDB file: choose a location on your computer to save the PDB file. The file will typically be named 1UBI.pdb.</li>
</ul>
<br>

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/PDB.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/ubiquitine.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">

</div>

<br>
<hr>
<br>

<b style="font-size: 20px;">Step 2</b>: cleaning the pdb file for topology preparation <br><br>
The PDB file must be prepared for simulation. It is recommended to keep a copy of the downloaded PDB file from the PDB as a reference. This is because you may need to refer back to the original PDB file if you encounter any problems during the preparation or simulation process. <br><br>

<ul style="text-align: justify;">
<li><b>Clean</b> the file: CONNECT records define connectivity between atoms, but AMBER utilizes its own internal bonding information. Remove them from the PDB.</li>
<li><b>Add</b> all necessary TER records, as well as END at the end of the PDB, if not present: The TER records mark the end of a chain, while the END record indicates the PDB file’s termination.</li>
<li><b>Strip</b> all hydrogen atoms: It is recommended that AMBER be used to add hydrogen atoms to the structure being prepared. AMBER may sometimes have issues during topology creation. Therefore, it is recommended to remove them using a text editor (Atom, Gedit) or program like Pymol or VMD.</li>
<li><b>Always</b> visualize the file using VMD: If there are no strange bonds and/or incorrect visualizations, the file can be used for the next step.</li>
</ul>
<br>
</p>

<br>
<hr>
<br>

<p style="text-align: justify;">
<b style="font-size: 20px;">Step 3</b>: building topolgy using TLeap module of AMBER <i>(on local server)</i> <br><br>
Once you have the prepared pdb, you can pass it to <i>TLeap</i> module for topology construction. TLeap is a powerful tool within the Amber molecular simulation software suite specifically designed for preparing molecular systems for simulations. It offers a comprehensive set of functionalities to manipulate and modify molecular structures. For more information, visit this <a href="https://ambermd.org/tutorials/pengfei">link</a>. <br>
For the purpose of this tutorial, it is recommended to open <i>TLeap</i> and type one command at a time to understand what is being done. The command is: <br><br>
<code>tleap</code><br><br>

The workflow has a setting like this: <br><br></p>

<ul style="text-align: justify;">
<li>Load all the necessary force field (FF): this loading <b>ALWAYS</b> depend on the type of molecules present in the PDB you are using. If there are only proteins, it must be load only the protein FF (protein.ff19SB). If there are proteins and RNA, FFs of proteins and RNA(RNA.OL3) must be loaded. After the loading of all necessary FF, also the FF for water (water.TIP3P) must be loaded to solvate the complex.<br>
All 	force 	fields 	available 	for 	AMBER22 	are 	at 	the 	following <a href="https://ambermd.org/AmberModels.php">link</a>. <br>
To load the FF for proteins, use the command:<br><br>
<code>source leaprc.protein.ff19SB</code></li><br>
<li>Load the PDB. The PBD must be loaded using a variable name and the <i>loadpdb</i> command, as follows:<br><br></li>
<code> pdb = loadpdb yourpdb.pdb</code><br><br>
<li>Check the loaded PDB file for any errors using the <i>check</i> command:<br><br>
<code>check</code><br><br></li>
<li>Check the charge of the loaded pdb using the <i>charge</i> command (keep in mind the charge of the complex under examination)<br><br>
<code>charge</code><br><br></li>
<li>Save the AMBER processed PDB without water using the <i>savepdb</i> command:<br><br>
<code>savepdb pdb yourfile_nowat.pdb </code><br><br></li>
<li>Save prmtop (topology file) and rst7 (coordinates file) without water using the <i>saveamberparms</i> command:<br><br>
<code>saveamberparms pdb yourfile_nowat.prmtop yourfile_nowat.rst7</code><br><br></li>
<li>Solvate the system. The solvateOct command is used in the link below, but for now it is preferred a cubic box. Therefore, the command shown must be used:<br><br>
<code>solvateBox pdb TIP3PBOX padding </code><br><br>
The padding is a number and represents the thickness of water to be inserted around the complex. A minimum padding of 10.0 angstroms is required.<br><br></li>
<li>Neutralize the system by adding counterions. Keeping track of the system's charge, use the command:<br><br>
<code>addIons pdb ion 0 command </code><br><br>
For Ion use  Na+/K+ or Cl- based on the system's charge. The 0 at the end of the command indicates that the total charge must be 0, therefore neutral. TLeap will replace water molecules with the necessary ions.</li><br>
<li>Save the hydrated and ionized PDB using the <i>savepdb</i> command</li><br>
<li>Save the hydrated and ionized prmtop and rst7 using the <i>saveamberparms</i> command</li><br></ul>

<p style="text-align: justify;">
The <b>prmtop</b> is a common file format used to store protein topology information. This information includes the types of atoms in the protein, their positions, and their connections to each other. The <b>rst7</b> is a less common file format that is also used to store protein topology information. It is typically used in conjunction with the prmtop file to provide additional information about the protein structure.<br>
The topology refers to the arrangement of atoms in a molecule. This information is important for understanding how the molecule functions. The coordinates refer to the positions of the atoms in a molecule. This information is important for visualizing the molecule and understanding its structure. <br>
For some specifications, please refer to the <a href="https://ambermd.org/tutorials/">link</a>.</p>
<br>
<hr>
<br>


<p style="text-align: justify;">
<b style="font-size: 20px;">Step 4</b>: Energy Minimization <br><br>
In MD simulations, Energy Minimization (EM) is the process of finding a structure of a molecular system that corresponds to a local minimum on its potential energy surface (PES). This is done by iteratively adjusting the positions of the atoms in the system until the force on each atom is approximately zero. The resulting structure is often referred to as the "optimized geometry" or "minimum energy structure" of the system.<br>
EM is a common first step in MD simulations, as it provides a starting point for more complex simulations such as molecular dynamics trajectories. It can also be used to study the properties of molecules in their equilibrium states.
EM is usually done in four steps, with decreasing constraints for the first 3 minimizations. It is recommended to run the minimizations step by step. It is done "in vacuum", so there are no parameters that indicate temperature, volume or pressure.<br>
Four separate parameter files are required (which must have the <i>.in</i> extension for AMBER; example: em.in). The number of steps to be performed (<b>maxcyc</b>), how often to save coordinates (<b>ntwx</b>), etc. must be specified in these files. For all the parameters to specify in the parameter files, please refer to the <a href="https://ambermd.org/tutorials/">AMBER tutorial</a>.<br><br>
Therefore, you need to prepare:<br></p>
<ul>
<li>em.in, in which the costraints are at 20.0 kcal/mol</li>
<li>em2.in, costraints at 10.0 kcal/mol </li>
<li>em3.in, costraints at 5.0 kcal/mol </li>
<li>em4.in, no costraints</li><br></ul>


The constraints must be added at the end of each <i>file.in</i>, using the following wording (after backslash <b>/</b> sign ): <br><br>
{% raw %}
```html
Hold the Ubiquitin fixed
20.0
RES x y
END END
```
{% endraw %}

<p style="text-align: justify;">
In this case <b>x</b> and <b>y</b> represent the first and last residue of the ubiquitin protein, so they would technically be <b>1</b> and <b>76</b>. This wording is used to specify the range of residues within which to apply the constraints.<br><br></p>

<b>N.B.</b>Compared to the parameters suggested in the <a href="https://ambermd.org/tutorials/basic/tutorial0/index.php">tutorial</a>, insert also: <br><br>
<ul style="text-align: justify;">
<li>ut = 9.0 ! non-bonded interactions cutoff </li>
<li>ncyc = 500 ! Initially do 500 steps of steepest descent minimization followed by 1500 steps (MAXCYC - NCYC) steps of conjugate gradient minimization.</li>
<li>ntb = 1 ! pbc for constant volume</li>
<li>ntpr = 1 ! print the progress of the minimization every ntpr step </li>
<li>iwrap = 1 ! enforcment of pbc conditions </li>
<li>ntwx = 100 ! frequency of writing coordinated to trajectory file </li>

</ul>


<p style="text-align: justify;">
Once the files are prepared, the <i>pmemd</i> command is invoked to run the minimization. Example: <br><br>
<code>pmemd -O -i em.in -p complex.prmtop -c complex.rst7 –ref complex.rst7 -o mdout.em1 -inf mdinfo.em1 -r restrt.em1 -x mdcrd.em1</code><br><br>

where <b>mdinfo</b> is the file with the run specifications, while <b>mdout</b> is the file where you can check how the run is proceeding (you can see the time and the various parameters of the specific step you are checking). The <b>mdcrd</b> file indicates the trajectory file.<br><br>

<b>IMPORTANT</b>: from the second minimization onwards, you must switch to <b>-c</b> and <b>-ref</b> no longer complex.rst7, but the <b>restart</b> of the previous run. So in em2 it will be <b>-c restrt.em2 -ref restrt.em2</b>.<br><br>

EM is a computational method that aims to find the most stable geometrical conformation of a molecular system. This process is based on the minimization of an energy function, which represents the total potential energy of the system. Different constraint forces can be applied to guide the minimization towards a specific configuration.<br>
However, it is important to note that simple energy minimization does not involve writing an mdcrd trajectory file. This is because EM focuses exclusively on the geometrical configuration of the system, neglecting information about the movement and velocity of the molecules.<br>
Despite specifying a value of <b>ntwx=100</b> in the input file (.in), which indicates the number of minimization steps to be performed, the mdcrd file will not contain any frames describing the evolution of the system over time. The mdcrd trajectory is only generated if a dynamic simulation is performed, which simulates the motion of the molecules based on the laws of molecular mechanics.<br>
In summary, EM is a static process that determines the most stable geometrical conformation of a system, while dynamic simulations generate trajectories that describe the motion of molecules over time. To obtain an mdcrd file containing the system trajectory, it is necessary to perform a dynamic simulation and not just an energy minimization.<br>
</p>

<br>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/pikachu.gif" title="rage" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/duck.gif" title="confusion" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/meow.gif" title="desperation" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
     At this point you'll probably reseamble one of these moods. Choose yours and go on!
</div>

<br>
<hr>
<br>

<p style="text-align: justify;">
<b style="font-size: 20px;">Step 5</b>: Equilibration (NVT) <br><br>
After completing the four minimization steps, the system must be equilibrated first in temperature and then in pressure. An NVT simulation allows the molecules in the system to move around and interact with each other, gradually reaching a state of equilibrium where their properties like positions, velocities, and energies fluctuate around a stable average value.<br>
In order to equilibrate the system, four NVT simulations with decreasing constraints are needed: <br>
<ul>
<li>nvt1.in costraints at 0.5 kcal/mol</li>
<li>nvt2.in, costraints at 0.25 kcal/mol </li>
<li>nvt3.in, costraints at 0.10 kcal/mol</li>
<li>nvt4.in, costraints at 0.05 kcal/mol</li><br></ul></p>

<p style="text-align: justify;">
The parameter file should be structured similarly to the previous minimization file. It should include the definition of the constraints and the range of residues to which they should be applied. For all the parameters to specify the parameter file, please refer to the <a href="https://ambermd.org/tutorials/basic/tutorial0/index.php">link</a>.<br>
It is important to remember that the system is at a temperature of 0 K at the beginning of the first NVT simulation. Therefore, in NVT1, the system must be heated from 0 K to 300K (37°C) by adding two lines of parameters (only at the end of the file nvt1.in, after <b>&end</b> sign):<br><br>
</p>

{% raw %}
```html
&wt type='TEMP0', istep1=0, istep2=2500000,
value1=0.0,value2=300.0, &end
&wt type='END', &end
```
{% endraw %}

<p style="text-align: justify;">
In this case, the system is being forced to go from 0.0 K to 300.00 K over 2500000 steps, which is 5 ns.<br>
In the other parameters files it would be sufficient fix the value1 to 300.0, such as follows:<br></p>

{% raw %}
```html
&wt type='TEMP0', istep1=0, istep2=2500000,
value1=300.0,value2=300.0, &end
&wt type='END', &end
```
{% endraw %}

<p style="text-align: justify;">
<b>N.B.</b> The following parameters should be used instead of those suggested in the manual (Heat.in): <br>
<ul style="text-align: justify;">
<li>cut  =  9.0 ! non-bonded interactions cutoff</li>
<li>nstlim = 2500000 ! total number of steps of NVT; in this case 2500000 are 5 ps </li>
<li>dt = 0.002 ! timestep in ps; the timestep determines how muche the simulated time advances with each computational step </li>
<li>gamma_ln = 1.0 ! parameter related to Langevin thermostat, used to mantain constant temperature </li>
<li>ntpr = 500 ! frequency (in timesteps) at which pressure and temperature information are written </li>
<li>ntwx = 500 ! frequency (in timesteps) at which coordinates are writted </li>
<li>ntwr  = 500 ! frequency (in timesteps) at which the system restart file is written </li><br><br>
</ul></p>


<p style="text-align: justify;">
From NVT onwards (NVT, NPT, and MD), the commands run on the <b>GPU</b>. Proceed as follows: <br>
<ol style="text-align: justify;">
<li>Check the GPU usage status using the command: <br>
<code>nvidia-smi</code><br><br>
The command will display the details of the available GPUs, including their identification number (0/1) and whether or not they are being used (<b>%</b>). If the <b>%</b> value is <b>0</b>, then the GPU is free. </li><br>
<li>Run the command:<br>
<code>export CUDA_VISIBLE_DEVICES=x</code><br><br>
where <b>x</b> is the number of the free GPU (0 or 1).</li><br>
<li>Run the command <i>pmemd.cuda</i>:
<code>nohup pmemd.cuda -O -i nvt1.in -p complesso.prmtop –c restrt.em4 o mdout.nvt1  -inf mdinfo.nvt1 -r restrt.nvt1 -x mdcrd.nvt1 & </code><br><br>
Using <b>nohup</b> and <b>&</b> will run the command in the background. You can monitor the processe using: <br>

<ul>
<li><i>htop</i> to display the information about the CPU processes;</li>
<li><i>nvidia-smi</i> o shown the GPU information; <b>tail -f mdinfo file</b> to see how long the run will take.</li>
</ul>
</li>
</ol><br><br>
</p>

<br>
<hr>
<br>

<p style="text-align: justify;">
<b style="font-size: 20px;">Step 6</b>: Presurrization (NPT) <br><br>
The NPT Ensemble is a simulation technique used to maintain a constant number of particles (N), pressure (P), and temperature (T) in a system. This is particularly useful for studying systems under pressure or in contact with a surrounding environment.<br>
Performed the four NVT runs, a single NPT run is required, in which the system is relaxed (no costraints are present).  The pressure is applied. <br>
In this case, it is necessary to prepare a single npt.in file of parameters. The npt.in file should be similar to the previous nvt steps files. The costraints wording must be removed. For all the parameters to specify the parameter file, please refer to the <a href="https://ambermd.org/tutorials/basic/tutorial0/index.php">link</a>.<br>
<br>
<b>N.B.</b>The following parameters should be used instead of those suggested in the manual:
<ul style="text-align: justify;">
<li>ntx = 5 ! Number of timesteps for temperature equilibration  </li>
<li>ntb = 2 ! This variable controls whether or not periodic boundaries are imposed on the system during the calculation of non-bonded interactions.  </li>
<li>pres0 = 1.0 ! Target pressure in MPa  </li>
<li>npt = 1 ! Type of NPT ensemble (1 for classical NPT)  </li>
<li>taup = 2.0 ! Pressure relaxation time in picoseconds  </li>
<li>ntr = 0 ! No restraints applied  </li>
<li>temp0 = 300.0 ! Initial temperature in Kelvin</li>
<li>tempi = 300.0 ! Target temperature in Kelvin  </li>
<li>gamma_ln = 1.0 ! Sphericity parameter for Langevin thermostat </li>
<li>nstlim = 2500000 ! Maximum number of timesteps  </li>
<li>dt = 0.002 ! Timestep in picoseconds  </li>
<li>ntpr = 500 ! Frequency (in timesteps) for printing pressure  </li>
<li>ntwx = 500 ! Frequency (in timesteps) for writing trajectory data </li>
<li>ntwr = 500 ! Frequency (in timesteps) for writing restart data </li>
</ul><br><br>

Once the npt.in file has been appropriately prepared, the NPT can be launched as the previous NVTs were (using <i>pmemd.cuda</i>). <br>
</p>
<br>
<hr>
<br>

<p style="text-align: justify;">
<b style="font-size: 20px;">Step 7</b>: Classical Molecular Dynamics (MD <br><br>
In the production run, the actual data collection for the properties of interest occurs. This is the most time-consuming part of the simulation, as it requires a large number of steps to generate a statistically representative trajectory. The length of the production run will depend on the system being studied and the properties of interest. A general rule of thumb is to run the simulation for at least 10 times the relaxation time of the system. The relaxation time is the time it takes for the system to forget its initial state and reach equilibrium.<br>
Once the system equilibration phase is complete, the system is ready for production.<br>
A md.in parameter file is required, structured similarly to that of the previous steps! It will be sufficient to take the one from the npt and modify it by setting <b>nstlim</b> to the number of steps sufficient to run <b>100 ns</b> of dynamics.<br>  <br>

Remember that  <b>nstlim * dt = time in ps and 1000 ps = 1 ns </b>.<br><br>
So if 2500000 steps were run in NPT (2500000 * 0.002 = 5000 ps = 5 ns), how many nstlim are needed to run 100000 ps / 100 ns? The answer is simple: 50000000 (50000000 * 0.002 = 100 ns). <br>
Once the md.in file has been appropriately modified, the simulation can be launched as the previous NVT and NPT were (<i>pmemd.cuda</i>).  <br>
Of course the MD step will take a longer time to terminate! The run depends strictly from the number of atoms of the system. <br>
</p>

<br>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/victory.gif" title="cheers" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/ubi.gif" title="simulation of ubiquitine" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/cheers.gif" title="bye" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
      Congrats, you've reached the end of this tutorial! Are you still sane? Go to the "Visualization of trajectory with VMD"
</div>


<br>
<p>For all information about AMBER22, please visit the <a href="https://ambermd.org/doc12/Amber22.pdf">manual</a>. Thanks to <b>Alessia Pinto</b> for beta-testing and debugging this tutorial!</p>
