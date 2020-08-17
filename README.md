README - flir-yocto-openmanifest
================================
flir-yocto-openmanifest is used to build up a flir yocto environment (Q3\-18 generation, targets "Sherlock", FLIR imx7)  
It populates a yocto source tree from various external and FLIR git repos
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
There is also a *flir-exported.xml* manifest that adds *meta-flir-base* at HEAD of *yocto2.5* (meta-flir-base) branch.<br>
(Other manifests might be added later on)


(For FLIR internal builds, there exists also a *meta-flir-internal* layer that contains som non-public code.<br>
This layer in NOT published as open source.<br>
Addition of the meta-flir-internal layer would add a few recipes and would create a slightly different conf/bblayers.conf<br> 
However, it would not change any of the recipes provided in *meta-flir-base*
)


/Ulf Palmér 2020-08-21
