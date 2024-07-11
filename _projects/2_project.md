---
layout: page
title: Visualization of trajectories with VMD
description: Isn't it beutiful?
img: assets/img/ubi.gif
importance: 2
category: work
giscus_comments: false
---

<p style="text-align: justify;">
Molecular dynamics (MD) simulations are essential for understanding the behavior of molecules over time, but interpreting their complex data can be challenging. Visualization programs like Visual Molecular Dynamics (VMD) are crucial for making sense of this data, offering several key benefits:
<br>
<ul>
<li>Enhanced Understanding: Visualization allows researchers to see molecular movements and interactions, providing deeper insights into processes like protein folding and molecular docking.</li>
<li>Critical Interaction Identification: By visualizing molecular trajectories, scientists can identify important interactions and conformational changes, which is vital for drug design and other applications.</li>
<li>Effective Communication: High-quality visualizations help communicate complex molecular phenomena to a broader audience, facilitating collaboration and understanding.</li>
<li>Validation and Hypothesis Testing: Visualization helps validate simulation results and refine models by comparing them with experimental data.</li>
<li>Educational Value: Tools like VMD are invaluable for teaching, providing an engaging way to explore and understand molecular dynamics.</li>
</ul>
<br>
In summary, VMD and similar visualization tools are indispensable in molecular dynamics, transforming complex data into comprehensible and visually appealing insights that advance our understanding of molecular science.<br><br>

Once you have a trajectory, how can you visualize it? Let's assume you have a mdcrd trajectory (AMBER type). Now you have to strip all the unnecessary atoms (water and ions used to reproduce a physiological condition) from the file. How?<br>
This can be done using CPPTRAJ which is integrated into the AmberTools package. You can find everything <a href="https://ambermd.org/tutorials/basic/tutorial0/">here</a>.<br><br>
<i>CPPTRAJ</i> is a powerful tool for analyzing and manipulating molecular trajectories. In this context, it's used to remove water molecules and ions from the trajectory, effectively isolating the molecules of interest. The conversion from mdcrd to xtc is necessary also for the subsequent analyses that will be done (RMSD, RMSF, ecc). <br><br>

It will be enough to:
<ol>
<li>Call cpptraj</li>
<li>Load the ionized prmtop file</li>
<li>Load the mdcrd trajectory of interest (from nvt1 on; the energy minimization trajectories are not visualizable)</li>
<li>Strip water molecules and ions</li>
<li>Save the output</li>
</ol>
</p>

Example:

{% raw %}
```html
cpptraj
parm yourfile_ionized.prmtop
trajin mdcrd.nvt1
strip :WAT,Na+ #this command will remove water atoms and Na+ ions from the trajectory
autoimage :1-46 #this command will remap the residues to correct the pbc problems
trajout nvt1_nowat.xtc xtc
go
quit
```
{% endraw %}
<br>

<p style="text-align: justify;">
Once this is done, we obtain a trajectory without water that can be visualized locally using VMD!
<code>vmd yourfile_nowat.pdb nvt1_nowat.xtc</code><br><br>

<b>OPS</b>, you don't have installed VMD on your computer? Nothing simplier!<br>
<ol>
<li> Visit the VMD website section <a href="https://www.ks.uiuc.edu/Development/Download/download.cgi?PackageName=VMD">Download</a></li>
<li> Download the correct version of <b>Version 1.9.3 (2016-11-30)</b>. You need to register to VMD website in order to have a license to install the program!</li>
<li> Untar the tar file you'll get</li>
<li> Move to the <b>vmd-1.9.3/</b> folder and open a terminal</li>
<li> Type <code>./configure</code></li>
<li> Move to the <b>src/</b> folder</li>
<li> Type <code>sudo make install</code></li>
<li> Everything would work, now, by simply typing <code>vmd</code> in your command line</li>
</ol>
</p>
