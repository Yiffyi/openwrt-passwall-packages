include $(TOPDIR)/rules.mk

PKG_NAME:=frp
PKG_VERSION:=0.62.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/fatedier/frp/tar.gz/v$(PKG_VERSION)?
PKG_HASH:=4bc2515c4840a48706963a53b919f1d2e75c1dbbd8eed167ba113d4c00c503d9

PKG_LICENSE:=Apache-2.0
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DEPENDS:=golang/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0
PKG_BUILD_FLAGS:=no-mips16

GO_PKG:=github.com/fatedier/frp
GO_PKG_BUILD_PKG:=github.com/fatedier/frp/cmd/...

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/golang/golang-package.mk

define Package/frp/install
	$(call GoPackage/Package/Install/Bin,$(PKG_INSTALL_DIR))
	$(INSTALL_DIR) $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/$(2) $(1)/usr/bin/

  $(INSTALL_DIR) $(1)/etc/$(2)
  $(INSTALL_DATA) $(PKG_BUILD_DIR)/conf/$(2)_full.ini $(1)/etc/$(2)/

	$(INSTALL_DIR) $(1)/etc/init.d/
	$(INSTALL_BIN) ./files/$(2).init $(1)/etc/init.d/$(2)
endef

define Package/frp/template
  define Package/$(1)
    SECTION:=net
    CATEGORY:=Network
    SUBMENU:=Web Servers/Proxies
    TITLE:=$(1) - fast reverse proxy $(2)
    URL:=https://github.com/fatedier/frp
    DEPENDS:=$(GO_ARCH_DEPENDS)
  endef

  define Package/$(1)/description
    $(1) is a fast reverse proxy $(2) to help you expose a local server behind
    a NAT or firewall to the internet.
  endef

  define Package/$(1)/conffiles
/etc/$(1)/
  endef

  define Package/$(1)/install
    $(call Package/frp/install,$$(1),$(1))
  endef
endef

$(eval $(call Package/frp/template,frpc,client))
$(eval $(call Package/frp/template,frps,server))
$(eval $(call GoBinPackage,frpc))
$(eval $(call GoBinPackage,frps))
$(eval $(call BuildPackage,frpc))
$(eval $(call BuildPackage,frps))