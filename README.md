README - flir-yocto-openmanifest
================================
flir-yocto-openmanifest is used to build up a flir yocto environment (yocto 3.3 generation - "hardknott")<br>
This yocto environment represents the platform software used in a number of FLIR products.

There are a number of supported targets listed below

- *ec302* - board name for FLIR E8 Pro product
- *ec201* - board name for FLIR C5 product
- *eoco* -  board name for FLIR G-Series products
- *evco* -  board name for FLIR Exx-Series and FLIR T-Series

(note that only *ec302* has been verified with flir-yocto-openmanifest, yocto3.3 as of now)<br>

The manifests in this repo will populate a yocto source tree from various external git repos
using "repo tool"  
(install from: https://source.android.com/setup/build/downloading#installing-repo)

To init a new flir-yocto source tree, perform as described here:
1.  *mkdir \<your folder\>*
2.  *cd \<your folder\>*
3.  **repo init -u https://github.com/flir-cx/flir-yocto-openmanifest.git -b yocto3.3 -m master.xml**  
    (default.xml as default manifest, master is default branch - you should use *yocto3.3* here.<br> 
     use **-b** and/or **-m** flags if change to this is required 
     (own manifest, own layout)
4.  *repo sync* (or *repo sync -d*) <br>
    populates the repos given by the manifest<br>
    <br>
5.  A) First time setup (creates new build folder):<br>
    **MACHINE=ec302 source ./flir-setup-release -b build_ec302**<br>
	Creates a build folder configured for given MACHINE and sets up bitbake for this environment<br>
    (replace *ec302* with your intended MACHINE if building for another hw than ec302)<br>
	<br>
	B) Restore work (reuses existing build folder):<br>
    **source setup-environment build_ec302**<br>
	"attaches" to an existing build folder and sets up bitbake for the environment<br>
    (To restore work, you may of course use *flir-setup-release* instead, but then edits to
    *local.conf* are lost)<br>
	<br>
6.  To let recipes fetch external code and to build artifacts:<br>
    *bitbake \<your recipes..\>*<br>
    (recipe is normally *flir-image*, i.e. *bitbake flir-image*)
	
## About flir-yocto-openmanifest
This manifest will add a number of "standard" layers for yocto to the local file tree. It will also add a `meta-flir-base` yocto layer provided by FLIR

Default branch of this manifest uses *master.xml* which adds *meta-flir-base* at a given commit ID<br>
There is a *flir-exported.xml* manifest that adds *meta-flir-base* at HEAD of *yocto3.3* (meta-flir-base) branch, independent of its commit-id.<br>
There are also a number of manifest .xml files that corresponds to specific public releases of FLIR yocto code.<br>
Typically *master_ec302_v1.7.xml* will check out the code as in delivered rootfs *ec302_v1.7*<br>
To get code corresponding to ec302_v1.7,<br> 
Set up the initial environment as described above.<br>
Then use commands:<br>
**repo init -m ec302_v1.7.xml**<br>
**repo sync**<br>
(Examining git history of this repository shows how these release .xml files are added)

For the real published FLIR builds, there also exists a *meta-flir-internal* layer that contains some non-public code that is added to rootfs.<br>
This *meta-flir-internal* layer is NOT published as open source.<br>
Addition of the meta-flir-internal layer would add a few recipes and would create a slightly different conf/bblayers.conf<br> 
Adding meta-flir-internal will not change published result from any of the public recipes provided in *meta-flir-base*<br>
But the content of binary images built from this open flir-yocto-openmanifest will contain fewer artifacts 

## About (flir-yocto-openmanifest) branches (2023-08-17)

* **master** branch is for yocto2.5; supports ec201
* **yocto3.3** branch is for yocto3.3; supports ec302 (ec201, eoco, evco)
* **match_eoco_v1.10** branch is for yocto3.3 eoco release *eoco_v1.10* only

Other branches might also exist for specific purposes, probably described in branch files and/or commit messages

Note that the FLIR application binaries distributed with a product expects a specific level of rootfs platform
If for instance building and installing a yocto3.3 version of ec201 platform onto a FLIR C5, the intalled camera application will not run. 
Wrong level of linked libraries.
