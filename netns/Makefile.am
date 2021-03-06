# CORE
# (c)2010-2013 the Boeing Company.
# See the LICENSE file included in this distribution.
#
# author: Jeff Ahrenholz  <jeffrey.m.ahrenholz@boeing.com>
#
# Makefile for building netns.
#

AM_CFLAGS = -Wall -fno-strict-aliasing -O3 -g @libev_CFLAGS@
SETUPPY	= setup.py
SETUPPYFLAGS = -v

SRC_COMMON = vnode_msg.c vnode_cmd.c vnode_chnl.c vnode_io.c \
	     vnode_msg.h vnode_cmd.h vnode_chnl.h vnode_io.h \
	     vnode_tlv.h myerr.h netns.h
SRC_VNODED = vnoded_main.c vnode_server.c netns.c \
	     vnode_server.h
SRC_VCMD = vcmd_main.c vnode_client.c \
	   vnode_client.h
SRC_NETNS = netns_main.c netns.c netns.h

noinst_PROGRAMS	= vnoded vcmd netns
vnoded_LDADD = @libev_LIBS@
vnoded_SOURCES = $(SRC_COMMON) $(SRC_VNODED)
vcmd_LDADD = @libev_LIBS@
vcmd_SOURCES = $(SRC_COMMON) $(SRC_VCMD)
netns_SOURCES = $(SRC_NETNS)

# this triggers automake to run setup.py for building the Python libraries
#   actual library names are netns.so and vcmd.so
#   SOURCES line prevents 'make dist' from looking for a 'libnetns.c' file
noinst_LIBRARIES = libnetns.a
libnetns_a_SOURCES = netnsmodule.c vcmdmodule.c
libnetns.a:
	LDFLAGS="$(LDFLAGS) @libev_LIBS@" CFLAGS="$(CFLAGS) @libev_CFLAGS@" $(PYTHON) setup.py build_ext

# Python libraries install
install-exec-local:
	$(PYTHON) $(SETUPPY) $(SETUPPYFLAGS) install \
		--root=/$(DESTDIR) \
		--prefix=$(prefix) \
		--install-lib=$(pythondir) \
		--single-version-externally-managed \
		--no-compile

# Python libraries uninstall
uninstall-hook:
	rm -rf core_netns.egg-info
	rm -rf $(DESTDIR)/$(pythondir)/core_netns-$(PACKAGE_VERSION)-py$(PYTHON_VERSION).egg-info
	rm -f $(DESTDIR)/$(bindir)/{vnoded,vcmd,netns}
	rm -f $(DESTDIR)/$(pythondir)/{netns.so,vcmd.so}

# Python libraries cleanup
clean-local: clean-local-check
.PHONY: clean-local-check
clean-local-check:
	-rm -rf build

distclean-local:
	-rm -rf core_netns.egg-info

# extra cruft to remove
DISTCLEANFILES = Makefile.in MANIFEST

# include source files for Python libraries with distribution tarball
EXTRA_DIST = setup.py MANIFEST.in

