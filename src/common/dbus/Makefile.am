noinst_LIBRARIES = libhexchatdbus.a
libhexchatdbus_a_SOURCES =			\
	dbus-plugin.c				\
	dbus-plugin.h				\
	dbus-client.c				\
	dbus-client.h

EXTRA_DIST =				\
	remote-object.xml		\
	apps_hexchat_url_handler.schemas	\
	marshallers.list		\
	example.py			\
	org.hexchat.service.service.in \
	README

BUILT_SOURCES =				\
	marshallers.h			\
	remote-object-glue.h

CLEANFILES = $(BUILT_SOURCES)

AM_CPPFLAGS = $(COMMON_CFLAGS) $(DBUS_CFLAGS)

noinst_PROGRAMS = example
example_SOURCES = example.c 
example_LDADD = $(DBUS_LIBS) $(GLIB_LIBS)

remote-object-glue.h: remote-object.xml
	$(LIBTOOL) --mode=execute $(DBUS_BINDING_TOOL) --prefix=remote_object --mode=glib-server --output=$@ $<

marshallers.h: marshallers.list
	$(LIBTOOL) --mode=execute $(GLIB_GENMARSHAL)  --header --body $< > $@

# Dbus service file
servicedir = $(DBUS_SERVICES_DIR)
service_in_files = org.hexchat.service.service.in
service_DATA = $(service_in_files:.service.in=.service)

# Rule to make the service file with bindir expanded
$(service_DATA): $(service_in_files) Makefile
	@sed -e "s|\@bindir\@|$(bindir)|" $< > $@

if DO_GCONF
GCONF_SCHEMA_CONFIG_SOURCE = `$(GCONFTOOL) --get-default-source`
GCONF_SCHEMA_FILE_DIR = $(sysconfdir)/gconf/schemas
schemadir = $(GCONF_SCHEMA_FILE_DIR)
schema_DATA = apps_hexchat_url_handler.schemas
install-data-local:
	if test -z "$(DESTDIR)" ; then \
		for p in $(schema_DATA) ; do \
			GCONF_CONFIG_SOURCE=$(GCONF_SCHEMA_CONFIG_SOURCE) $(GCONFTOOL) --makefile-install-rule $(srcdir)/$$p; \
		done \
	fi
else
install-data-local:
endif
