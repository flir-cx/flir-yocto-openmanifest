README - flir-yocto-openmanifest
================================
flir-yocto-openmanifest is used to build up a flir yocto environment (yocto 2.5 generation - "sumo")<br>
Supported target is "Sherlock" (internal name for FLIR C5 product)<br>
(NOTE: other branches exists to support more products than "Sherlock". Check for yocto3.3 branch in this repo, possibly others)

The manifests in this repo will populate a yocto source tree from various external and FLIR git repos
using "repo tool"  
(install from: https://source.android.com/setup/build/downloading#installing-repo)

To init a new flir-yocto tree, perform as described here:
1.  mkdir \<your selection\>
2.  cd \<your selection\>
3.  **repo init -u https://github.com/flir-cx/flir-yocto-openmanifest.git**  
    (default.xml as default manifest, master as default branch. 
     use **-b** and/or **-m** flags if change to this is required 
     (own manifest, own layout)
4.  **repo sync**
5.  create build folder (and prepare build) as:<br> 
    **MACHINE=ec201 source ./flir-setup-release.sh -b build_ec201**
6.  **bitbake \<your recipes..\>**

## About flir-yocto-openmanifest
This manifest will add a number of "standard" layers for yocto to the local file tree. It will also add a `meta-flir-base` yocto layer provided by FLIR

Default branch of this manifest uses *master.xml* which adds *meta-flir-base* at a given commit ID<br>
There is a *flir-exported.xml* manifest that adds *meta-flir-base* at HEAD of *yocto2.5* (meta-flir-base) branch, independent of its commit-id.<br>
There are also a number of manifest .xml files that corresponds to specific public releases of the FLIR C5 yocto code.<br>
Typically *master_ec201_v1.40.xml* will check out the code as in delivered rootfs *ec201_v1.40*
To get code corresponding to ec201_v1.40, use commands:<br>
**repo init -m master_ec201_v1.40.xml**<br>
**repo sync**<br>
(Examining git history of this repository shows how these release .xml files are added)


For the real published FLIR builds, there also exists a *meta-flir-internal* layer that contains some non-public code that is added to rootfs.<br>
This *meta-flir-internal* layer is NOT published as open source.<br>
Addition of the meta-flir-internal layer would add a few recipes and would create a slightly different conf/bblayers.conf<br> 
Adding meta-flir-internal will not change published result from any of the public recipes provided in *meta-flir-base*<br>
But the content of binary images built from this open flir-yocto-openmanifest will contain fewer artifacts 
