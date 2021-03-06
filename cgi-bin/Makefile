#
# CGI makefile for CUPS.
#
# Copyright 2007-2018 by Apple Inc.
# Copyright 1997-2006 by Easy Software Products.
#
# Licensed under Apache License v2.0.  See the file "LICENSE" for more information.
#

include ../Makedefs

LIBOBJS	=	\
		help-index.o \
		html.o \
		ipp-var.o \
		search.o \
		template.o \
		var.o
OBJS	=	\
		$(LIBOBJS) \
		admin.o \
		classes.o \
		help.o \
		jobs.o \
		makedocset.o \
		printers.o \
		testcgi.o \
		testhi.o \
		testtemplate.o
CGIS	=	\
		admin.cgi \
		classes.cgi \
		help.cgi \
		jobs.cgi \
		printers.cgi
LIBTARGETS =	\
		libcupscgi.a \
		$(LIBCUPSCGI)

UNITTARGETS =	\
		testcgi \
		testhi \
		testtemplate

TARGETS	=	\
		$(LIBTARGETS) \
		$(CGIS)


#
# Make all targets...
#

all:	$(TARGETS)


#
# Make library targets...
#

libs:


#
# Make unit tests...
#

unittests:	$(UNITTARGETS)


#
# Clean all object files...
#

clean:
	$(RM) $(OBJS) $(TARGETS) $(UNITTARGETS) makedocset
	$(RM) libcupscgi.so libcupscgi.dylib libcupscgi.a


#
# Update dependencies (without system header dependencies...)
#

depend:
	$(CC) -MM $(ALL_CFLAGS) $(OBJS:.o=.c) >Dependencies


#
# Install all targets...
#

install:	all install-data install-headers install-libs install-exec


#
# Install data files...
#

install-data:


#
# Install programs...
#

install-exec:
	if test $(LIBCUPSCGI) != libcupscgi.a; then \
		echo Installing libraries in $(LIBDIR)...; \
		$(INSTALL_DIR) -m 755 $(LIBDIR); \
		$(INSTALL_LIB) $(LIBCUPSCGI) $(LIBDIR); \
	fi
	if test "x$(SYMROOT)" != "x"; then \
		$(INSTALL_DIR) $(SYMROOT); \
		cp $(LIBCUPSCGI) $(SYMROOT); \
		dsymutil $(SYMROOT)/$(LIBCUPSCGI); \
	fi
	$(INSTALL_DIR) -m 755 $(SERVERBIN)/cgi-bin
	for file in $(CGIS); do \
		$(INSTALL_BIN) $$file $(SERVERBIN)/cgi-bin; \
	done
	if test "x$(SYMROOT)" != "x"; then \
		$(INSTALL_DIR) $(SYMROOT); \
		for file in $(CGIS); do \
			cp $$file $(SYMROOT); \
			dsymutil $(SYMROOT)/$$file; \
		done \
	fi


#
# Install headers...
#

install-headers:


#
# Install libraries...
#

install-libs:


#
# Uninstall all targets...
#

uninstall:
	for file in $(CGIS); do \
		$(RM) $(SERVERBIN)/cgi-bin/$$file; \
	done
	-$(RMDIR) $(SERVERBIN)/cgi-bin
	$(RM) $(LIBDIR)/libcupscgi.1.dylib
	$(RM) $(LIBDIR)/libcupscgi.a
	$(RM) $(LIBDIR)/libcupscgi.dylib
	$(RM) $(LIBDIR)/libcupscgi.so
	$(RM) $(LIBDIR)/libcupscgi.so.1
	-$(RMDIR) $(LIBDIR)


#
# libcupscgi.so.1
#

libcupscgi.so.1:	$(LIBOBJS)
	echo Linking $@...
	$(DSO) $(ARCHFLAGS) $(LDFLAGS) $(DSOFLAGS) -o $@ $(LIBOBJS) $(LIBS)
	$(RM) `basename $@ .1`
	$(LN) $@ `basename $@ .1`


#
# libcupscgi.1.dylib
#

libcupscgi.1.dylib:	$(LIBOBJS) libcupscgi.exp
	echo Linking $@...
	$(DSO) $(ARCHFLAGS) $(LDFLAGS) $(DSOFLAGS) -o $@ \
		-install_name $(libdir)/$@ \
		-current_version 1.0.0 \
		-compatibility_version 1.0.0 \
		-exported_symbols_list libcupscgi.exp \
		$(LIBOBJS) $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@
	$(RM) libcupscgi.dylib
	$(LN) $@ libcupscgi.dylib


#
# libcupscgi.la
#

libcupscgi.la:    $(LIBOBJS)
	echo Linking $@...
	$(LD_CC) $(ARCHFLAGS) $(LDFLAGS) $(DSOFLAGS) -o $@ $(LIBOBJS:.o=.lo) -rpath $(LIBDIR) \
		-version-info 1:0 $(LIBS)


#
# libcupscgi.a
#

libcupscgi.a:	$(LIBOBJS)
	echo Archiving $@...
	$(RM) $@
	$(AR) $(ARFLAGS) $@ $(LIBOBJS)
	$(RANLIB) $@


#
# admin.cgi
#

admin.cgi:	admin.o ../Makedefs ../cups/$(LIBCUPS) $(LIBCUPSCGI)
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -o $@ admin.o -lcupscgi $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@


#
# classes.cgi
#

classes.cgi:	classes.o ../Makedefs ../cups/$(LIBCUPS) $(LIBCUPSCGI)
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -o $@ classes.o -lcupscgi $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@


#
# help.cgi
#

help.cgi:	help.o ../Makedefs ../cups/$(LIBCUPS) $(LIBCUPSCGI)
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -o $@ help.o -lcupscgi $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@


#
# jobs.cgi
#

jobs.cgi:	jobs.o ../Makedefs ../cups/$(LIBCUPS) $(LIBCUPSCGI)
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -o $@ jobs.o -lcupscgi $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@


#
# printers.cgi
#

printers.cgi:	printers.o ../Makedefs ../cups/$(LIBCUPS) $(LIBCUPSCGI)
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -o $@ printers.o -L. -lcupscgi $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@


#
# testcgi
#

testcgi:	testcgi.o ../Makedefs libcupscgi.a ../cups/$(LIBCUPSSTATIC)
	echo Linking $@...
	$(LD_CC) $(ARCHFLAGS) $(LDFLAGS) -o $@ testcgi.o libcupscgi.a \
		../cups/$(LIBCUPSSTATIC) $(COMMONLIBS) $(SSLLIBS) $(DNSSDLIBS) \
		$(LIBZ) $(LIBGSSAPI)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@
	echo Testing CGI API...
	./testcgi


#
# testhi
#

testhi:	testhi.o ../Makedefs libcupscgi.a ../cups/$(LIBCUPSSTATIC)
	echo Linking $@...
	$(LD_CC) $(ARCHFLAGS) $(LDFLAGS) -o $@ testhi.o libcupscgi.a \
		../cups/$(LIBCUPSSTATIC) $(COMMONLIBS) $(SSLLIBS) $(DNSSDLIBS) \
		$(LIBZ) $(LIBGSSAPI)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@
	echo Testing help index API...
	./testhi


#
# testtemplate
#

testtemplate:	testtemplate.o ../Makedefs libcupscgi.a ../cups/$(LIBCUPSSTATIC)
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -o $@ testtemplate.o libcupscgi.a ../cups/$(LIBCUPSSTATIC) \
		$(COMMONLIBS) $(SSLLIBS) $(DNSSDLIBS) $(LIBZ) $(LIBGSSAPI)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@


#
# Dependencies...
#

include Dependencies
