soutdir = $(pluginsdir)/stream_out

libstream_out_dummy_plugin_la_SOURCES = stream_out/dummy.c
libstream_out_cycle_plugin_la_SOURCES = stream_out/cycle.c
libstream_out_delay_plugin_la_SOURCES = stream_out/delay.c
libstream_out_stats_plugin_la_SOURCES = stream_out/stats.c
libstream_out_description_plugin_la_SOURCES = stream_out/description.c
libstream_out_standard_plugin_la_SOURCES = stream_out/standard.c
libstream_out_standard_plugin_la_CPPFLAGS = $(AM_CPPFLAGS) $(CPPFLAGS_access_output_srt)
libstream_out_standard_plugin_la_LIBADD = $(SOCKET_LIBS)
libstream_out_duplicate_plugin_la_SOURCES = stream_out/duplicate.c
libstream_out_es_plugin_la_SOURCES = stream_out/es.c
libstream_out_display_plugin_la_SOURCES = stream_out/display.c
libstream_out_gather_plugin_la_SOURCES = stream_out/gather.c
libstream_out_bridge_plugin_la_SOURCES = stream_out/bridge.c
libstream_out_mosaic_bridge_plugin_la_SOURCES = stream_out/mosaic_bridge.c
libstream_out_autodel_plugin_la_SOURCES = stream_out/autodel.c
libstream_out_record_plugin_la_SOURCES = stream_out/record.c
libstream_out_smem_plugin_la_SOURCES = stream_out/smem.c
libstream_out_setid_plugin_la_SOURCES = stream_out/setid.c
libstream_out_transcode_plugin_la_SOURCES = \
	stream_out/transcode/transcode.c stream_out/transcode/transcode.h \
	stream_out/transcode/spu.c \
	stream_out/transcode/audio.c stream_out/transcode/video.c
libstream_out_transcode_plugin_la_CFLAGS = $(AM_CFLAGS)
libstream_out_transcode_plugin_la_LIBADD = $(LIBM)

sout_LTLIBRARIES = \
	libstream_out_dummy_plugin.la \
	libstream_out_cycle_plugin.la \
	libstream_out_delay_plugin.la \
	libstream_out_stats_plugin.la \
	libstream_out_description_plugin.la \
	libstream_out_standard_plugin.la \
	libstream_out_duplicate_plugin.la \
	libstream_out_es_plugin.la \
	libstream_out_display_plugin.la \
	libstream_out_gather_plugin.la \
	libstream_out_bridge_plugin.la \
	libstream_out_mosaic_bridge_plugin.la \
	libstream_out_autodel_plugin.la \
	libstream_out_record_plugin.la \
	libstream_out_smem_plugin.la \
	libstream_out_setid_plugin.la \
	libstream_out_transcode_plugin.la

# RTP plugin
sout_LTLIBRARIES += libstream_out_rtp_plugin.la
libstream_out_rtp_plugin_la_SOURCES = \
	stream_out/rtp.c stream_out/rtp.h stream_out/rtpfmt.c \
	stream_out/rtcp.c stream_out/rtsp.c stream_out/vod.c
libstream_out_rtp_plugin_la_CFLAGS = $(AM_CFLAGS)
libstream_out_rtp_plugin_la_LIBADD = $(SOCKET_LIBS)
if HAVE_GCRYPT
SRTP_CFLAGS = -I$(srcdir)/access/rtp
SRTP_LIBS = libvlc_srtp.la
libstream_out_rtp_plugin_la_CFLAGS += -DHAVE_SRTP $(SRTP_CFLAGS) \
	$(GCRYPT_CFLAGS)
libstream_out_rtp_plugin_la_LIBADD += $(SRTP_LIBS) $(GCRYPT_LIBS)
endif

# Chromaprint plugin
libstream_out_chromaprint_plugin_la_SOURCES = stream_out/chromaprint.c stream_out/chromaprint_data.h dummy.cpp
libstream_out_chromaprint_plugin_la_CPPFLAGS = $(AM_CPPFLAGS) $(CHROMAPRINT_CFLAGS)
libstream_out_chromaprint_plugin_la_LDFLAGS = $(AM_LDFLAGS) -rpath '$(soutdir)'
libstream_out_chromaprint_plugin_la_LIBADD = $(CHROMAPRINT_LIBS)
EXTRA_LTLIBRARIES += libstream_out_chromaprint_plugin.la
sout_LTLIBRARIES += $(LTLIBstream_out_chromaprint)

# Chromecast plugin
SUFFIXES += .proto .pb.cc

%.pb.h %.pb.cc: %.proto
	$(PROTOC) --cpp_out=. -I$(srcdir) $<

libdemux_chromecast_plugin_la_SOURCES = stream_out/chromecast/chromecast_demux.cpp stream_out/chromecast/chromecast.h \
                                        stream_out/chromecast/chromecast_common.h
libdemux_chromecast_plugin_la_CPPFLAGS = $(AM_CPPFLAGS) -Istream_out/chromecast

libstream_out_chromecast_plugin_la_SOURCES = stream_out/chromecast/cast.cpp stream_out/chromecast/chromecast.h \
                                             stream_out/chromecast/cast_channel.proto \
                                             stream_out/chromecast/chromecast_common.h stream_out/chromecast/chromecast_ctrl.cpp \
                                             misc/webservices/json.h misc/webservices/json.c stream_out/chromecast/chromecast_communication.cpp
nodist_libstream_out_chromecast_plugin_la_SOURCES = stream_out/chromecast/cast_channel.pb.cc
libstream_out_chromecast_plugin_la_CPPFLAGS = $(AM_CPPFLAGS) -Istream_out/chromecast $(CHROMECAST_CFLAGS)
libstream_out_chromecast_plugin_la_LIBADD = $(CHROMECAST_LIBS) $(SOCKET_LIBS)
CLEANFILES += $(nodist_libstream_out_chromecast_plugin_la_SOURCES)

if ENABLE_SOUT
if BUILD_CHROMECAST
BUILT_SOURCES += stream_out/chromecast/cast_channel.pb.h
sout_LTLIBRARIES += libstream_out_chromecast_plugin.la
demux_LTLIBRARIES += libdemux_chromecast_plugin.la
endif
endif
