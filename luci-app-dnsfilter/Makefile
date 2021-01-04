include $(TOPDIR)/rules.mk

PKG_NAME:=luci-app-dnsfilter
PKG_VERSION:=1.0
PKG_RELEASE:=4
PKG_LICENSE:=GPLv2
PKG_MAINTAINER:=Small_5

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
  SECTION:=LuCI
  CATEGORY:=LuCI
  SUBMENU:=3. Applications
  TITLE:=LuCI support for DNSFilter
  DEPENDS:=+curl +dnsmasq-full +ipset +luci-compat
  PKGARCH:=all
endef

define Package/$(PKG_NAME)/description
	Luci Support for DNSFilter.
endef

define Build/Prepare
	$(foreach po,$(wildcard ${CURDIR}/po/zh-cn/*.po), \
		po2lmo $(po) $(PKG_BUILD_DIR)/$(patsubst %.po,%.lmo,$(notdir $(po)));)
endef

define Package/$(PKG_NAME)/postinst
#!/bin/sh
[ -n "${IPKG_INSTROOT}" ] || {
	(. /etc/uci-defaults/luci-app-dnsfilter) && rm -f /etc/uci-defaults/luci-app-dnsfilter
	exit 0
}
endef

define Build/Compile
endef

define Package/$(PKG_NAME)/conffiles
/etc/config/dnsfilter
/etc/dnsfilter/
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/controller
	$(INSTALL_DATA) ./luasrc/controller/* $(1)/usr/lib/lua/luci/controller/
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/model/cbi/dnsfilter
	$(INSTALL_DATA) ./luasrc/model/cbi/dnsfilter/* $(1)/usr/lib/lua/luci/model/cbi/dnsfilter/
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/view/dnsfilter
	$(INSTALL_DATA) ./luasrc/view/dnsfilter/* $(1)/usr/lib/lua/luci/view/dnsfilter/
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/i18n
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/dnsfilter.*.lmo $(1)/usr/lib/lua/luci/i18n/
	$(INSTALL_DIR) $(1)/etc/dnsfilter
	$(INSTALL_DATA) ./root/etc/dnsfilter/* $(1)/etc/dnsfilter/
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_DATA) ./root/etc/config/* $(1)/etc/config/
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./root/etc/init.d/* $(1)/etc/init.d/
	$(INSTALL_DIR) $(1)/etc/uci-defaults
	$(INSTALL_BIN) ./root/etc/uci-defaults/* $(1)/etc/uci-defaults/
	$(INSTALL_DIR) $(1)/usr/share/dnsfilter
	$(INSTALL_BIN) ./root/usr/share/dnsfilter/* $(1)/usr/share/dnsfilter/
	$(INSTALL_DIR) $(1)/usr/share/rpcd/acl.d
	$(INSTALL_DATA) ./root/acl.d/* $(1)/usr/share/rpcd/acl.d/
endef

$(eval $(call BuildPackage,$(PKG_NAME)))
