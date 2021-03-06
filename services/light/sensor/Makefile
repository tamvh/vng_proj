##############################################
# OpenWrt Makefile for vng.iot.services.ble.advertise service
#
##############################################

include $(TOPDIR)/rules.mk

# Name and release number of this package
PKG_NAME:=vng.iot.services.light.sensor
PKG_RELEASE:=1


# This specifies the directory where we're going to build the program.
# The root build directory, $(BUILD_DIR), is by default the build_mipsel
# directory in your OpenWrt SDK directory
PKG_BUILD_DIR := $(BUILD_DIR)/$(PKG_NAME)


include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk


# Specify package information for this program.
# The variables defined here should be self explanatory.
define Package/$(PKG_NAME)
	SECTION:=vng
	CATEGORY:=VNG
	TITLE:= VNG iot light sensor service
	DEPENDS:=+libstdcpp +vng.iot.sdk +libcurl
endef

# Specify what needs to be done to prepare for building the package.
# In our case, we need to copy the source files to the build directory.
# This is NOT the default.  The default uses the PKG_SOURCE_URL and the
# PKG_SOURCE which is not defined here to download the source from the web.
# In order to just build a simple program that we have just written, it is
# much easier to do it this way.
define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	$(CP) ./src/* $(PKG_BUILD_DIR)/
endef


# We do not need to define Build/Configure or Build/Compile directives
# The defaults are appropriate for compiling a simple program such as this one


# Specify where and how to install the program. Since we only have one file,
# the helloworld executable, install it by copying it to the /bin directory on
# the router. The $(1) variable represents the root directory on the router running
# OpenWrt. The $(INSTALL_DIR) variable contains a command to prepare the install
# directory if it does not already exist.  Likewise $(INSTALL_BIN) contains the
# command to copy the binary file from its current location (in our case the build
# directory) to the install directory.
define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/$(PKG_NAME) $(1)/bin/
	$(INSTALL_DIR) $(1)/etc/vng/services/light
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/files/etc/vng/services/light/sensor.json $(1)/etc/vng/services/light/sensor.json
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/files/etc/init.d/services/light/sensor.init $(1)/etc/init.d/vng.services.light.sensor
endef


# This line executes the necessary commands to compile our program.
# The above define directives specify all the information needed, but this
# line calls BuildPackage which in turn actually uses this information to
# build a package.
$(eval $(call BuildPackage,$(PKG_NAME)))