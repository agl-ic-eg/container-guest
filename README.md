# Container guest repo

This image is based on yocto thud release.  

## Building the BSP for M3 Starter Kit

Currently using R-CarGen3 BSP 3.21.

### Clone build tree.

Move to build directry

	export WORK=`pwd`  
	repo init -u https://github.com/agl-ic-eg/container-guest.git  
	repo sync  

### Download proprietary driver modules to $WORK/proprietary folder.

You should see the following files:

	$ ls -1 $WORK/proprietary/*.zip  
	R-Car_Gen3_Series_Evaluation_Software_Package_for_Linux-weston5-20190802.zip  
	R-Car_Gen3_Series_Evaluation_Software_Package_of_Linux_Drivers-weston5-20190802.zip  

### Populate meta-renesas with proprietary software packages.

	export PKGS_DIR=$WORK/proprietary  
	cd $WORK/meta-renesas  
	sh meta-rcar-gen3/docs/sample/copyscript/copy_evaproprietary_softwares.sh -f $PKGS_DIR  
	unset PKGS_DIR  


### Setup build environment

	cd $WORK  
	source poky/oe-init-build-env  

### Prepare default configuration files.  

	cp $WORK/meta-renesas/meta-rcar-gen3/docs/sample/conf/m3ulcb/poky-gcc/mmp/*.conf ./conf/  
	cd $WORK/build  
	cp conf/local-wayland.conf conf/local.conf  

### Edit bblayers.conf with layer requirements:

	  ${TOPDIR}/../meta-container-guest \  


### Edit local.conf with evaluation packages requirements:

	DISTRO_FEATURES_append = " use_eva_pkg"  
	
	DISTRO_FEATURES_remove = "zeroconf"  

### Start the build

	bitbake core-image-weston  



## Building the BSP for QEMU

TBD
