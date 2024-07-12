---
layout: page
title: Analyses of MD trajectories
description: a simple tutorial to analyse your MD trajectory with Gromacs
img: assets/img/ubi.gif
#redirect: https://unsplash.com
importance: 3
category: work
---
<b>WORK IN PROGRESS!</b> Please, wait 'till this page is completed!

<p style="text-align: justify;">
The Molecular Dynamics (MD) trajectory will be analyzed using the modules contained in the <i>Gromacs 2023</i> suite. This part of the tutorial can be performed directly on a server or locally on your own PC, where Gromacs must be installed.<br>
If Gromacs is installed, simply run the command <code>gmx -version</code> to check which version is installed. If it is not installed, you can install it using the command <code>sudo apt-get install -y gromacs</code>.
</p>
<br>

<p style="text-align: justify;">
<b style="font-size: 20px;">Step 1</b>: retrieve the structure<br><br>
The file must be prepared for subsequent analyses by fitting the trajectory. Rotational and translational movements must be removed. Removing rotational and translational movements from a molecular dynamics trajectory focuses the analysis on the internal dynamics of the system. This ensures that calculated metrics like RMSD reflect changes in the molecule's shape and internal structure, rather than its overall position or orientation.<br>
For the purposes of this tutorial, the fitting will be performed on the entire protein without its hydrogens. This can be done using the command:<br><br>
<code>gmx trjconv -s yourfile_nowat.pdb -f your_trajectory.xtc -fit progressive -o your_trajectory_fitted.xtc</code><br>

where:

<ul>
<li><b>-s</b> specificies the reference structure;</li>
<li><b>-f</b>specifies the input trajectory;</li>
<li><b>-fit</b> instructs Gromacs to do a progressive fit in which the first timeframe is fitted to the reference structure in the structure file to obtain and each subsequent timeframe is fitted to the previously fitted structure;</li>
<li><b>-o</b> specifies the name of the output</li>
</ul>
<br>
Once the command is executed, select <b>Protein-H</b> as the group to fit the trajectory by typing <b>2</b>, and <b>System</b> as the group to write the output trajectory by typing 0. You can visualize the difference between the trajectory fitted and not fitted using VMD!
</p>
<br>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/nofit.gif" title="nofit" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-5 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/bulbasaur.gif" title="bulbasaur" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/fit.gif" title="fit" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    At your left, the unfitted MD trajectory of Ubiquitine. At your right, the fitted trajectory. In the middle? Just a cute Bulbasaur.
</div>

<br>
<p style="text-align: justify;">
<b style="font-size: 20px;">Step 2</b>: installation of Xmgrace<br><br>
To visualize the resulting files from the analyses performed with Gromacs, you will need to install xmgrace. <b>Xmgrace</b> is a scientific plotting tool for creating high-quality graphs and visualizations. It is widely used in the scientific community for plotting data from molecular dynamics simulations and other types of scientific data.<br>
From the command line, simply type:<br><br>
<code>sudo apt-get install grace</code><br><br>
For	more	information,	visit	the	Xmgrace <a href="https://plasma-gate.weizmann.ac.il/Grace/doc/UsersGuide.html">manual</a>.
</p>

<br>
<p style="text-align: justify;">
<b style="font-size: 20px;">Step 3</b>: Root Mean Square Deviation (RMSD)<br><br>
RMSD (Root Mean Square Deviation) is a measure used to quantify the difference between two three- dimensional structures of a molecule. It is calculated as the square root of the average of the squared distances between corresponding atoms in the two structures.
RMSD is often used for evaluating structural stability, comparing conformations or studying conformational transitions. Usually a typical output should have on the x-axis the simulation time. On the y-axis, there is the RMSD value expressed in <i>nm</i>.<br>
Typically, the curve shows a rapid increase in the early part of the simulation, then stabilizes around a value and oscillates around it. This means that the simulation took the initial part of the time to complete equilibration, then reached a stable phase called a <i>plateau</i> or <i>convergence</i>.<br>
For the purposes of this tutorial, the RMSD will be calculated on the entire protein without the hydrogens. To calculate the RMSD, use the command:<br><br>
<code>gmx rms -s your_file.pdb -f your_trajectory_fitted.xtc -tu ns -o rmsd.xvg</code><br><br>

where:<br>

<ul>
<li><b>-s</b> specificies the reference structure;</li>
<li><b>-f</b>specifies the input trajectory;</li>
<li><b>-tu</b> ns intructs Gromacs to convert picoseconds to nanosecond. The time in the output will be written in ns;</li>
<li><b>-o</b> specifies the name of the output</li>
</ul>
<br>
Once the command is executed, select <b>Protein-H</b> as the reference group by typing <b>2</b>, and <b>Protein-H</b> as the group to calculate the RMSD with by typing <b>2</b>. <br> You will retrieve a <b>rmsd.xvg</b> that can be opened using the command:<br><br>
<code>xmgrace rmsd.xvg</code>
</p>
<br><br>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/rmsd.png" title="RMSD_ubiquitine" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    RMSD of Protein-H of Ubiquitine over 100 ns of classical MD simulation.
</div>

<br>
<p style="text-align: justify;">
<b style="font-size: 20px;">Step 4</b>: Root Mean Square Fluctuations (RMSF)<br><br>
RMSF (Root Mean Square Fluctuation) is a measure used to quantify the flexibility or mobility of each atom in a molecule throughout a MD simulation. It is calculated as the square root of the average of the squared fluctuations of each atom's position relative to its average position in the trajectory.<br>
RMSF is often used for identifying flexible regions in proteins, such as loop regions or protein-ligand binding sites. The N and C-terms are a lot flexible, as shown in figure for N-terminal part.<br>
A typical output plot of RMSF shows on the x-axis the residue index or number, and on the y-axis, the RMSF value expressed in <i>nm</i>. <br>
For the purposes of this tutorial, RMSF will be calculated for each residue of the protein without the hydrogens. To calculate RMSF, use the command:<br><vbr>
</p>

<code>gmx rmf -s your_file.pdb -f your_trajectory_fitted.xtc -res -o rmsf.xvg</code><br><br>

<ul>
<li><b>-s</b> specificies the reference structure;</li>
<li><b>-f</b>specifies the input trajectory;</li>
<li><b>-res</b> intructs Gromacs to insert the residue number on the x-axis;</li>
<li><b>-o</b> specifies the name of the output</li>
</ul>
<p style="text-align: justify;">
Once the command is executed, select <b>Protein-H</b> as the reference group to calculate RMSF by typing <b>2</b>. You will retrieve a <b>rmsf.xvg</b> that can be opened using the command:<br><br>
<code>xmgrace rmsf.xvg</code>
<br><br></p>

<br><br>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/rmsf.png" title="RMSF_ubiquitine" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    RMSF of Protein-H of Ubiquitine over 100 ns of classical MD simulation.
</div>

<p style="text-align: justify;">
<b style="font-size: 20px;">Step 5</b>: Principal Component Analysis (PCA)<br><br>
PCA (Principal Component Analysis) is a statistical method used to identify and analyze the major patterns in the variations of a dataset. In the context of MD simulations, PCA is often used to identify the principal motions of a biomolecule. In PCA, eigenvalues and eigenvectors are fundamental concepts for understanding the major modes of variation in the data.
<br>
<ol>
<li><b>Eigenvalue</b>: Eigenvalues are scalar factors that indicate the variance along each of the principal directions in the data. In practical terms, eigenvalues represent the importance of each principal component in explaining the overall variation in the data. A larger eigenvalue indicates that the corresponding principal component captures more variation in the data.</li>
<li><b>Eigenvector</b>: Eigenvectors are the vectors associated with eigenvalues that indicate the direction in which the variation in the data is maximal. In other words, eigenvectors represent the principal directions along which the data extends the most. Each eigenvector is associated with a single eigenvalue, and together they form an orthogonal basis that represents the major modes of variation in the data.</li>
</ol><br><br>
In PCA applied to MD trajectories, the eigenvectors of the covariance matrix represent the major modes of variation in the positions of atoms over time. The eigenvalues indicate how much each principal component contributes to the overall variance of the trajectory. These concepts are fundamental for understanding and interpreting the results of PCA and for identifying the major modes of molecular motion.<br>
From a PCA analysis, important files to retrieve are:<br><br>
</p>

<ul>
<li><b>2d projections</b> (2dprojection.xvg) : shows the projections of the trajectories onto the principal components (PC1 and PC2);</li>
<li><b>Collective motions</b> (collectivemotions1.pdb, collectivemotions2.pdb, collectivemotions3.pdb) : shows the important motions of the examined system.</li>
</ul>
<br>
To perform PCA, use the command:<br><br>
<code>gmx trjconv -s your_file.pdb -f your_trajectory_fitted.xtc -skip 10 -o traj_fitted_skip10.xtc</code><br><br>
where:<br>
<ul>
<li><b>-skip 10</b> : is useful to light the trajectory and to allow a faster calculation: every 10 frames of the original trajectory, a frame is written in the output trajectory.</li>
</ul><br>
<code>gmx covar -s myfile.pdb -f traj_fitted_skip10.xtc -o eigenval.xvg -v eigenvec.trr -l PCA.log -av average_PCA.pdb -tu ns</code><br><br>
where:<br>
<ul>
<li><b>-o eigenval.xvg</b>: specifies the output file name where the eigenvalues will be saved in XVG format;</li>
<li><b>-v eigenvec.trr</b>: specifies the output file name where the eigenvectors will be saved in TRR format;</li>
<li><b>-l PCA.log</b>: specifies the output log file name where information about the principal component analysis (PCA) will be saved;</li>
<li><b>-av average_PCA.pdb</b>: specifies the output file name where the average structure (average of all conformations in the trajectory) will be saved in PDB format;</li>
<li><b>-tu ns</b>: specifies the time unit as nanoseconds.</li>
</ul><br>

<br>
For all information about Gromacs 2023, please visit the <a href="https://manual.gromacs.org/2023/">manual</a>. Thanks to <b>Alessia Pinto</b> for beta-testing and debugging this tutorial!

<br><br>
WORK IN PROGRESS! Please, wait 'till this page is completed!
