#Copying and distribution of this file, with or without modification,
#are permitted in any medium without royalty provided the copyright
#notice and this notice are preserved.  This file is offered as-is,
#without any warranty

GITVERSION_FILE=flexisip_gitversion.h
GITVERSION_FILE_TMP=flexisip_gitversion.h.tmp
GITDESCRIBE=`git describe`
GITREVISION=`git rev-parse HEAD`
GIT_TAG=`cd $(top_srcdir) && git describe --abbrev=0`
AUTOMAKE_OPTIONS = subdir-objects

ECHO=/bin/echo

CLEANFILES=$(GITVERSION_FILE)
BUILT_SOURCES=$(GITVERSION_FILE)

SUBDIRS=xml presence tclap
nodistsources=

bin_PROGRAMS=flexisip
thesources= \
			utils/flexisip-exception.hh \
			utils/signaling-exception.hh \
			agent.cc agent.hh \
			common.cc common.hh \
			sdp-modifier.hh  sdp-modifier.cc \
			callstore.hh  callstore.cc \
			forkcontext.hh forkcontext.cc \
			callcontext-mediarelay.hh  callcontext-mediarelay.cc \
			forkcallcontext.hh  forkcallcontext.cc \
			forkmessagecontext.hh  forkmessagecontext.cc \
			forkbasiccontext.cc forkbasiccontext.hh \
			registrardb-internal.cc registrardb-internal.hh registrardb.cc registrardb.hh \
			recordserializer-c.cc recordserializer.hh \
			recordserializer-json.cc cJSON.c cJSON.h \
			etchosts.cc etchosts.hh \
			lpconfig.cc lpconfig.h \
			configmanager.cc configmanager.hh\
			configdumper.cc configdumper.hh \
			event.cc event.hh \
			transaction.cc transaction.hh \
			module.cc module.hh \
			monitor.cc monitor.hh \
			entryfilter.cc entryfilter.hh \
			stun.cc stun.hh \
			stun/stun.c stun/stun_udp.c stun/flexisip_stun.h stun/flexisip_stun_udp.h \
			mediarelay.cc mediarelay.hh \
			authdb.hh authdb.cc authdb-file.cc \
			module-dos.cc \
			module-sanitychecker.cc \
			module-garbage-in.cc \
			module-forward.cc \
			module-gatewayadapter.cc \
			module-contact-route-inserter.cc \
			module-nat-helper.cc \
			module-registrar.cc \
			module-router.cc \
			module-transcode.cc \
			module-statistics-collector.cc \
			module-mediarelay.cc \
			module-auth.cc \
			module-loadbalancer.cc \
			stats.cc stats.hh \
			expressionparser.cc expressionparser.hh \
			sipattrextractor.cc sipattrextractor.hh \
			h264iframefilter.cc h264iframefilter.hh \
			telephone-event-filter.cc telephone-event-filter.hh \
			log/logmanager.cc log/logmanager.hh \
			eventlogs/eventlogs.cc eventlogs/eventlogs.hh \
			contact-masquerader.cc contact-masquerader.hh \
			uac-register.cc uac-register.hh \
			$(GITVERSION_FILE) \
			module-redirect.cc module-presence.cc \
			domain-registrations.cc domain-registrations.hh \
			utils/threadpool.cc utils/threadpool.hh



flexisip_LDADD= $(SOFIA_LIBS) $(ORTP_LIBS) $(MEDIASTREAMER_LIBS) $(HIREDIS_LIBS) $(PROTOBUF_LIBS) $(NETSNMPAGENT_LIBS)

AM_CXXFLAGS= $(SOFIA_CFLAGS) $(ORTP_CFLAGS) $(MEDIASTREAMER_CFLAGS) $(HIREDIS_CFLAGS) \
				$(PROTOBUF_CFLAGS) $(MYSQL_CFLAGS) \
				-DCONFIG_DIR=\"$(CONFIG_DIR)\" -DORTP_DEBUG_MODE $(WARNINGCXXFLAGS)

AM_CFLAGS=$(ORTP_CFLAGS) $(MEDIASTREAMER_CFLAGS) -DORTP_DEBUG_MODE

AM_LDFLAGS=-no-undefined


if BUILD_XSD
flexisip_LDADD+= xml/libxml_binding_generated.la $(XERCESC_LIBS)
AM_CXXFLAGS+= $(XSDCXX_CPPFLAGS) $(XERCESC_CFLAGS)
endif

if BUILD_SNMP
thesources+=snmp-agent.cc snmp-agent.h
AM_CPPFLAGS=-Imib
endif

if BUILD_PRESENCE
#module cannot be statically initialized in shared lib
flexisip_LDADD += presence/libflexisip-presence.la
AM_CXXFLAGS+= -DBELLE_SIP_USE_STL $(BELLESIP_CFLAGS)
endif

if USE_MONOTONIC_CLOCK
AM_CXXFLAGS+=-DUSE_MONOTONIC_CLOCK
endif

if WITHOUT_HIREDIS_CONNECT_CALLBACK
AM_CXXFLAGS +=-DWITHOUT_HIREDIS_CONNECT_CALLBACK
endif

if BUILD_ODBC
AM_CFLAGS+=-DENABLE_ODBC $(ODBC_CFLAGS)
AM_CXXFLAGS+=-DENABLE_ODBC $(ODBC_CFLAGS)

flexisip_LDADD+=$(ODBC_LIBS)
thesources+= authdb-odbc.cc
endif

if BUILD_DATEHANDLER
thesources+=module-datehandler.cc
endif

if BUILD_TRANSCODER
thesources+=	callcontext-transcoder.cc callcontext-transcoder.hh
endif

if BUILD_PUSHNOTIFICATION
thesources+=		module-pushnotification.cc \
			pushnotification/pushnotification.hh \
			pushnotification/googlepush.cc pushnotification/googlepush.hh \
			pushnotification/firebasepush.cc pushnotification/firebasepush.hh \
			pushnotification/applepush.cc pushnotification/applepush.hh \
			pushnotification/microsoftpush.cc pushnotification/microsoftpush.hh \
			pushnotification/pushnotificationservice.cc pushnotification/pushnotificationservice.hh \
			pushnotification/pushnotificationclient.cc pushnotification/pushnotificationclient.hh \
			pushnotification/pushnotificationclient_wp.cc pushnotification/pushnotificationclient_wp.hh \
			pushnotification/genericpush.cc pushnotification/genericpush.hh
AM_CXXFLAGS+=-DENABLE_PUSHNOTIFICATION $(OPENSSL_CPPFLAGS)
flexisip_LDADD+=$(OPENSSL_LIBS)
endif

if BUILD_REDIS
thesources+=registrardb-redis-async.cc registrardb-redis.hh \
        registrardb-redis-sofia-event.h
AM_CXXFLAGS+=-DENABLE_REDIS
endif

pb_files=$(srcdir)/recordserializer-protobuf.proto

if BUILD_PROTOBUF

$(builddir)/pb_generated_src_stamp : $(pb_files)
	protoc --proto_path=$(builddir) --cpp_out=$(builddir) $(pb_files)
	touch $(builddir)/pb_generated_src_stamp

BUILT_SOURCES+=$(builddir)/pb_generated_src_stamp

thesources+=recordserializer-protobuf.cc
nodistsources+=recordserializer-protobuf.pb.cc recordserializer-protobuf.pb.h
AM_CXXFLAGS+=-DENABLE_PROTOBUF
AM_CFLAGS+=-DENABLE_PROTOBUF

endif


# Redis
if BUILD_REDIS

bin_PROGRAMS+=flexisip_ctdumper flexisip_serializer flexisip_binder
flexisip_ctdumper_SOURCES=tools/ctdumper.cc $(thesources)
flexisip_ctdumper_LDADD=$(flexisip_LDADD)
nodist_flexisip_ctdumper_SOURCES=$(nodistsources)

endif


if ENABLE_SOCI

thesources+=authdb-soci.cc
flexisip_LDADD+=$(SOCI_LIBS)
AM_CXXFLAGS+=-DENABLE_SOCI $(SOCI_CFLAGS)

endif

flexisip_serializer_SOURCES=tools/serializer.cc tools/tool_utils.hh $(thesources)
flexisip_serializer_LDADD=$(flexisip_LDADD)
nodist_flexisip_serializer_SOURCES=$(nodistsources)

flexisip_binder_SOURCES=tools/binder.cc tools/tool_utils.hh $(thesources)
flexisip_binder_LDADD=$(flexisip_LDADD)
nodist_flexisip_binder_SOURCES=$(nodistsources)

noinst_PROGRAMS=expr
expr_SOURCES=test/expr.cc expressionparser.cc expressionparser.hh sipattrextractor.hh utils/flexisip-exception.hh
expr_CXXFLAGS=-DTEST_BOOL_EXPR -DNO_SOFIA $(MEDIASTREAMER_CFLAGS) $(ORTP_CFLAGS)
expr_LDADD= $(SOFIA_LIBS) $(ORTP_LIBS)


if BUILD_PUSHNOTIFICATION
bin_PROGRAMS+=flexisip_pusher
flexisip_pusher_SOURCES=tools/pusher.cc $(thesources)
flexisip_pusher_LDADD=$(flexisip_LDADD)
nodist_flexisip_pusher_SOURCES=$(nodistsources)
endif

flexisip_SOURCES=main.cc $(thesources)
nodist_flexisip_SOURCES=$(nodistsources)
EXTRA_DIST=$(pb_files) $(GITVERSION_FILE) tools/ctdumper.cc

clean-local:
	rm -f $(builddir)/recordserializer-protobuf.pb* $(builddir)/pb_generated_src_stamp


make_gitversion_h:
	if test "$(GITDESCRIBE)" != "" ; then \
		if [ "$(GIT_TAG)" != "$(PACKAGE_VERSION)" ]; then \
			$(ECHO) "*** PACKAGE_VERSION and git tag differ. Please put them identical."; \
			exit 1; \
		fi ; \
		$(ECHO) "#define FLEXISIP_GIT_VERSION \"$(GITDESCRIBE)\"" > $(GITVERSION_FILE_TMP) ; \
	elif test "$(GITREVISION)" != "" ; then \
		$(ECHO) -n "#define FLEXISIP_GIT_VERSION \"$(LINPHONE_VERSION)_$(GITREVISION)\"" > $(GITVERSION_FILE_TMP) ; \
	else \
		$(ECHO) -n "" > $(GITVERSION_FILE_TMP) ; \
	fi
	if test ! -f $(srcdir)/$(GITVERSION_FILE) ; then \
		cp -f $(GITVERSION_FILE_TMP) $(srcdir)/$(GITVERSION_FILE) ; \
	fi
	if test "`cat $(GITVERSION_FILE_TMP)`" != "`cat $(srcdir)/$(GITVERSION_FILE)`" ; then \
		cp -f $(GITVERSION_FILE_TMP) $(srcdir)/$(GITVERSION_FILE) ; \
	fi
	rm -f $(GITVERSION_FILE_TMP) ;

$(GITVERSION_FILE): make_gitversion_h
