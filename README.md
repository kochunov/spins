SPINS
=====

- study code: SPN01
- sites: CMH, ZHH, MRC
- idiosyncratic naming conventions: 
  - PHA = phantom
  - Subject IDs that start with P (e.g. SPN01_CMH_P001) are human phantoms
  - Subject IDs that are >9990 (e.g. SPN01_CMH_9999_01_01) are ???

See /archive/data-2.0/DTI/CMH/metadata/scans.csv for the mapping into xnat.

This contains the back-end code and user guides for the SPINS database at the Center for Addiction and Mental Health. No data is stored here. This SPINS database is built on the XNAT platform, with original modifications based on the SPReD database from the Baycrest Research Center.

+ [Introduction](#introduction)
+ [Quality Control](#quality-control)
+ [Admin](#admin)

Introduction
------------

**Connectivity**

+ SFTP: da55.pet.utoronto.ca:993
+ HTTP: http://da55.pet.utoronto.ca:5004/spred
+ DICOM: da55.pet.utoronto.ca:8104 [need 'calling' and 'called' title].

**bin/**

Contains the main programs run from the command line to interact with the xnat database. This code exports the dicom data, produces .pdf reports of the raw data, and pre-processes / analyzes the data in a basic way. These programs are currently a work-in-progress.

**code/**

Contains scripts that are either being kept as refernece during the development of the code found in /bin, or are independent scripts called on by bin/. It contains some dependencies not written by myself. See code/README.md for the lineage of various files.

**analysis/**

Contains old analysis done on the data. For example, initial phantom analysis runs, and the inter-site analysis of the human phantom data. Eventually this will contain on-going analysis (when we have enough participants in the study).

**archive/**

Contains files used in the construction of the study. For exmaple, the videos presented as part of the EA task, or the original timing files used to generate the GLM analysis. This is primarily for backup purposes.

**assets/**

Contains non-binary files that the code in bin/ and code/ call on for various tasks.

**docs/**

Contains documentation related to the study. E.g., scanner protocols, xnat-related guides, experimental protocols.

**website/**

A seperate branch of the repo lives inside here (this is kept from the master branch via .gitignore). This renders the website found at http://imaging-genetics.camh.ca/spins/, which is a space for posting updates about the project, analysis, etc.

Quality Control
---------------
We've built a number of quality control pipelines to help track quality across sites and image modalities. The outputs are `.PDF` files typically containing many plots. Below is a brief description on the outputs of each.

**T1-contrast, BOLD contrast, B0 contrast**

A set of axial slices designed to give an overview of the sequence. This is useful for identifying geometric distortions in the image, severe inhomogeneities, or orientation errors.  

**Head Motion**

For functional scans. Uses the motion-correction realignment parameters to calculate the framewise displacement (in mm/TR) of the head. This calculation currently assumes a head radius of 50mm to convert degrees of rotation into millimeters of displacement. The red line denotes the cut-off for TR scrubbing suggested in [1] resting-state scans. Subjects with a lot of TRs above this line may need to be removed from downstream analysis.

> [1] Spurious but systematic correlations in functional connectivity MRI networks arise from subject motion. Jonathan D. Power et al. 2011. Neuroimage 59:3.

**SFNR**

Shows voxel-wise signal-to-fluctuation noise ratio, a measure from the fBIRN QC pipeline.

**Slice/TR Abnormalities**

Shows the average and standard deviation of all values within each acquisition slice across all TRs. This allows us to visualize large deviations from the average mean value on a slice wise basis. This can happen on only some slices, and can help detect the presense of localized artifacts that occour during acquisition or reconstruction. The slice-dependent effects are particularly obvious in the DTI sequences, where particular gradient directions may be more susceptible to spike-noise. 

**DTI**

A compliment to the slice/TR abnormalities plot for DTI sequences, this shows a single coronal slice through the center of the acquisition for each TR. This can also help us identify artifactual spatial patterns that might not be obvious in the slice/TR plot.

**Phantom Scatterplots: ADNI**

This tracks the T1 weighted value across the 5 primary ROIs in the ADNI phantom, and the T1 ratios between each of the higher ones with the lowest one. For more information, please see http://www.phantomlab.com/library/pdf/magphan_adni_manual.pdf.

**Phantom Scatterplots: fBIRN fMRI**

This uses the fBIRN pipeline to define % signal fluctuation, linear drift, signal to noise ratio, signal-to-fluctuation noise ratio, and radius of decorrelation. For more information, please see [1], http://www.ncbi.nlm.nih.gov/pubmed/16649196.

> [2] Report on a multicenter fMRI quality assurance protocol. Friedman L et al. 2006. J Magn Reson Imaging 23(6). 

Admin
-----
**XNAT Upgrade**

Instructions: https://wiki.xnat.org/display/XNAT16/How+to+Upgrade+XNAT. If you want to preserve the existing database then you need to back it up before running the upgrade script.  


**Connect kimsrv to the VM** 

    # run as localadmin
    spins-xnat-connection # opens a tmux session 'xenattach' running autossh
    tmux attach-session -t xenattach # 
    killall autossh                  # kills connection


**VM Configuration**

XNAT now lives on the XEN VM platform, see configuration details [Here](https://github.com/TIGRLab/spins/wiki/XNAT-XEN-Transformation).

To access the XEN server:

    ssh localadmin@dti
    password == std. admin password for tigrlab

**Data Location**

The entire VM is located at `/var/lib/xen/images` on the XEN server. Within this, the DICOM data (and this repo) is stored in `/usr/xnat`, with `spred/` containing the DICOM data. 

**Project Upload Permissions**

For auto-upload scripting to work, you must set `Define Prearchive Settings` under `Manage` to 'All image data will be placed into the archive automatically and will overwrite existing files. Data which doesn't match a pre-existing project will be placed in an 'Unassigned' project.'

**Orginal Installation instructions**

I'm storing this here for archival purposes. However note that our current configuration also requires us to build.

+ Install the vbox iamge.  Choose 'new' virtual machine, select 'redhat' as the
  guest OS, and use the existing image as the storage.
+ The image uses NAT. Ports https:8443 and ssh:22 on the guest are mapped to
  5004 and 5003 on the host, respectively.  Feel free to change them if you
  like.
+ The main storage of SPReD is by default at /usr/xnat. If you want to use NFS
  mount for extended storage space then you need to
    3.1 - copy existing folders in /usr/xnat to /your/nfs/mount\_point
    3.2 - goto administer page after logging into SPReD, then
          'configuration'--> 'file system', and change all the paths except for
          the 'pipeline installation location' accordingly.

+ VM recommand spec. 4 cpus and 4GB of memory. hostname: spred.
+ Upload Applet is broken. We downloaded the  applet jars and drop them in the folder /var/lib/tomcat6/webapp/spred/applet from ftp://ftp.nrg.wustl.edu/pub/xnat/XNAT-1.6.2.1-applet-jars.zip.
+ Our VM has these now, but the applets still fail to work as of the last test.


2014_1202_AE005: Moved to DTI
-----------------------------
This subject is actually a DTI follow up. JDV linked they to DTI:

    + 2014_1202_AE005
    + DTI_CMH_S077_02_01
    + 06S077
    + 3133
