# Copyright (c) 2013-2016 The Bitcoin Core developers
# Distributed under the MIT software license, see the accompanying
# file COPYING or http://www.opensource.org/licenses/mit-license.php.

DIST_SUBDIRS = secp256k1 univalue

AM_LDFLAGS = $(PTHREAD_CFLAGS) $(LIBTOOL_LDFLAGS) $(HARDENED_LDFLAGS)
AM_CXXFLAGS = $(HARDENED_CXXFLAGS) $(ERROR_CXXFLAGS)
AM_CPPFLAGS = $(HARDENED_CPPFLAGS)
EXTRA_LIBRARIES =

if EMBEDDED_UNIVALUE
LIBUNIVALUE = univalue/libunivalue.la

$(LIBUNIVALUE): $(wildcard univalue/lib/*) $(wildcard univalue/include/*)
	$(AM_V_at)$(MAKE) $(AM_MAKEFLAGS) -C $(@D) $(@F)
else
LIBUNIVALUE = $(UNIVALUE_LIBS)
endif

BITCOIN_CONFIG_INCLUDES=-I$(builddir)/config
BITCOIN_INCLUDES=-I$(builddir) -I$(builddir)/obj $(BDB_CPPFLAGS) $(BOOST_CPPFLAGS) $(LEVELDB_CPPFLAGS) $(CRYPTO_CFLAGS) $(SSL_CFLAGS)

BITCOIN_INCLUDES += -I$(srcdir)/secp256k1/include
BITCOIN_INCLUDES += $(UNIVALUE_CFLAGS)

CLASHIC_SEEDER_INCLUDES = -I$(srcdir)/seeder
CLASHIC_SEEDER_INCLUDES += $(BITCOIN_INCLUDES)

LIBCLASHIC_SERVER=libclashic_server.a
LIBCLASHIC_COMMON=libclashic_common.a
LIBCLASHIC_CONSENSUS=libclashic_consensus.a
LIBCLASHIC_CLI=libclashic_cli.a
LIBCLASHIC_UTIL=libclashic_util.a
LIBCLASHIC_CRYPTO=crypto/libclashic_crypto.a
LIBCLASHICQT=qt/libclashicqt.a
LIBSECP256K1=secp256k1/libsecp256k1.la

if ENABLE_ZMQ
LIBCLASHIC_ZMQ=libclashic_zmq.a
endif
if build_clashic_LIBS
LIBCLASHICCONSENSUS=libclashicconsensus.la
endif
if build_clashic_SEEDER
LIBCLASHIC_SEEDER=libclashic_seeder.a
endif
if ENABLE_WALLET
LIBCLASHIC_WALLET=libclashic_wallet.a
endif

$(LIBSECP256K1): $(wildcard secp256k1/src/*) $(wildcard secp256k1/include/*)
	$(AM_V_at)$(MAKE) $(AM_MAKEFLAGS) -C $(@D) $(@F)

# Make is not made aware of per-object dependencies to avoid limiting building parallelization
# But to build the less dependent modules first, we manually select their order here:
EXTRA_LIBRARIES += \
  $(LIBCLASHIC_CRYPTO) \
  $(LIBCLASHIC_UTIL) \
  $(LIBCLASHIC_COMMON) \
  $(LIBCLASHIC_CONSENSUS) \
  $(LIBCLASHIC_SERVER) \
  $(LIBCLASHIC_CLI) \
  $(LIBCLASHIC_SEEDER) \
  $(LIBCLASHIC_WALLET) \
  $(LIBCLASHIC_ZMQ)

lib_LTLIBRARIES = $(LIBCLASHICCONSENSUS)

bin_PROGRAMS =
noinst_PROGRAMS =
TESTS =
BENCHMARKS =

if build_clashicD
  bin_PROGRAMS += clashicd
endif

if build_clashic_SEEDER
  bin_PROGRAMS += clashic-seeder
endif

if build_clashic_UTILS
  bin_PROGRAMS += clashic-cli clashic-tx
endif

.PHONY: FORCE check-symbols check-security
# bitcoin abcd #
BITCOIN_CORE_H = \
  addrdb.h \
  addrman.h \
  base58.h \
  bloom.h \
  blockencodings.h \
  cashaddr.h \
  chain.h \
  chainparams.h \
  chainparamsbase.h \
  chainparamsseeds.h \
  checkpoints.h \
  checkqueue.h \
  clientversion.h \
  coins.h \
  compat.h \
  compat/byteswap.h \
  compat/endian.h \
  compat/sanity.h \
  compressor.h \
  config.h \
  consensus/consensus.h \
  core_io.h \
  core_memusage.h \
  cuckoocache.h \
  globals.h \
  httprpc.h \
  httpserver.h \
  indirectmap.h \
  init.h \
  key.h \
  keystore.h \
  dbwrapper.h \
  limitedmap.h \
  memusage.h \
  merkleblock.h \
  miner.h \
  net.h \
  net_processing.h \
  netaddress.h \
  netbase.h \
  netmessagemaker.h \
  noui.h \
  policy/fees.h \
  policy/policy.h \
  pow.h \
  protocol.h \
  random.h \
  reverselock.h \
  rpc/blockchain.h \
  rpc/client.h \
  rpc/misc.h \
  rpc/protocol.h \
  rpc/server.h \
  rpc/register.h \
  scheduler.h \
  script/scriptcache.h \
  script/sigcache.h \
  script/sign.h \
  script/standard.h \
  script/ismine.h \
  streams.h \
  support/allocators/secure.h \
  support/allocators/zeroafterfree.h \
  support/cleanse.h \
  support/events.h \
  support/lockedpool.h \
  sync.h \
  threadsafety.h \
  threadinterrupt.h \
  timedata.h \
  torcontrol.h \
  txdb.h \
  txmempool.h \
  ui_interface.h \
  undo.h \
  util.h \
  utilmoneystr.h \
  utiltime.h \
  validation.h \
  validationinterface.h \
  versionbits.h \
  wallet/coincontrol.h \
  wallet/crypter.h \
  wallet/db.h \
  wallet/finaltx.h \
  wallet/rpcdump.h \
  wallet/rpcwallet.h \
  wallet/wallet.h \
  wallet/walletdb.h \
  warnings.h \
  zmq/zmqabstractnotifier.h \
  zmq/zmqconfig.h\
  zmq/zmqnotificationinterface.h \
  zmq/zmqpublishnotifier.h


obj/build.h: FORCE
	@$(MKDIR_P) $(builddir)/obj
	@$(top_srcdir)/share/genbuild.sh $(abs_top_builddir)/src/obj/build.h \
	  $(abs_top_srcdir)
libclashic_util_a-clientversion.$(OBJEXT): obj/build.h

# server: shared between clashicd and clashic-qt
libclashic_server_a_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES) $(MINIUPNPC_CPPFLAGS) $(EVENT_CFLAGS) $(EVENT_PTHREADS_CFLAGS)
libclashic_server_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libclashic_server_a_SOURCES = \
  addrman.cpp \
  addrdb.cpp \
  bloom.cpp \
  blockencodings.cpp \
  chain.cpp \
  checkpoints.cpp \
  config.cpp \
  globals.cpp \
  httprpc.cpp \
  httpserver.cpp \
  init.cpp \
  dbwrapper.cpp \
  merkleblock.cpp \
  miner.cpp \
  net.cpp \
  net_processing.cpp \
  noui.cpp \
  policy/fees.cpp \
  policy/policy.cpp \
  pow.cpp \
  rest.cpp \
  rpc/abc.cpp \
  rpc/blockchain.cpp \
  rpc/mining.cpp \
  rpc/misc.cpp \
  rpc/net.cpp \
  rpc/rawtransaction.cpp \
  rpc/server.cpp \
  script/scriptcache.cpp \
  script/sigcache.cpp \
  script/ismine.cpp \
  timedata.cpp \
  torcontrol.cpp \
  txdb.cpp \
  txmempool.cpp \
  ui_interface.cpp \
  validation.cpp \
  validationinterface.cpp \
  versionbits.cpp \
  $(BITCOIN_CORE_H)

if ENABLE_ZMQ
libclashic_zmq_a_CPPFLAGS = $(BITCOIN_INCLUDES) $(ZMQ_CFLAGS)
libclashic_zmq_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libclashic_zmq_a_SOURCES = \
  zmq/zmqabstractnotifier.cpp \
  zmq/zmqnotificationinterface.cpp \
  zmq/zmqpublishnotifier.cpp
endif


# wallet: shared between clashicd and clashic-qt, but only linked
# when wallet enabled
libclashic_wallet_a_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
libclashic_wallet_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libclashic_wallet_a_SOURCES = \
  wallet/crypter.cpp \
  wallet/db.cpp \
  wallet/finaltx.cpp \
  wallet/rpcdump.cpp \
  wallet/rpcwallet.cpp \
  wallet/wallet.cpp \
  wallet/walletdb.cpp \
  $(BITCOIN_CORE_H)

# crypto primitives library
crypto_libclashic_crypto_a_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_CONFIG_INCLUDES)
crypto_libclashic_crypto_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
crypto_libclashic_crypto_a_SOURCES = \
  crypto/aes.cpp \
  crypto/aes.h \
  crypto/chacha20.h \
  crypto/chacha20.cpp \
  crypto/common.h \
  crypto/hmac_sha256.cpp \
  crypto/hmac_sha256.h \
  crypto/hmac_sha512.cpp \
  crypto/hmac_sha512.h \
  crypto/ripemd160.cpp \
  crypto/ripemd160.h \
  crypto/sha1.cpp \
  crypto/sha1.h \
  crypto/sha256.cpp \
  crypto/sha256.h \
  crypto/sha512.cpp \
  crypto/sha512.h

# consensus: shared between all executables that validate any consensus rules.
libclashic_consensus_a_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
libclashic_consensus_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libclashic_consensus_a_SOURCES = \
  amount.h \
  arith_uint256.cpp \
  arith_uint256.h \
  consensus/merkle.cpp \
  consensus/merkle.h \
  consensus/params.h \
  consensus/validation.h \
  hash.cpp \
  hash.h \
  hash_blake2.cpp \
  hash_blake2.h \
  prevector.h \
  primitives/block.cpp \
  primitives/block.h \
  primitives/transaction.cpp \
  primitives/transaction.h \
  pubkey.cpp \
  pubkey.h \
  script/clashicconsensus.cpp \
  script/sighashtype.h \
  script/interpreter.cpp \
  script/interpreter.h \
  script/script.cpp \
  script/script.h \
  script/script_error.cpp \
  script/script_error.h \
  serialize.h \
  tinyformat.h \
  uint256.cpp \
  uint256.h \
  utilstrencodings.cpp \
  utilstrencodings.h \
  version.h

# common: shared between clashicd, and clashic-qt and non-server tools
libclashic_common_a_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
libclashic_common_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libclashic_common_a_SOURCES = \
  amount.cpp \
  base58.cpp \
  cashaddr.cpp \
  chainparams.cpp \
  coins.cpp \
  compressor.cpp \
  core_read.cpp \
  core_write.cpp \
  key.cpp \
  keystore.cpp \
  netaddress.cpp \
  netbase.cpp \
  protocol.cpp \
  scheduler.cpp \
  script/sign.cpp \
  script/standard.cpp \
  warnings.cpp \
  $(BITCOIN_CORE_H)

# util: shared between all executables.
# This library *must* be included to make sure that the glibc
# backward-compatibility objects and their sanity checks are linked.
libclashic_util_a_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
libclashic_util_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libclashic_util_a_SOURCES = \
  support/lockedpool.cpp \
  chainparamsbase.cpp \
  clientversion.cpp \
  compat/glibc_sanity.cpp \
  compat/glibcxx_sanity.cpp \
  compat/strnlen.cpp \
  random.cpp \
  rpc/protocol.cpp \
  support/cleanse.cpp \
  sync.cpp \
  threadinterrupt.cpp \
  util.cpp \
  utilmoneystr.cpp \
  utilstrencodings.cpp \
  utiltime.cpp \
  $(BITCOIN_CORE_H)

if GLIBC_BACK_COMPAT
libclashic_util_a_SOURCES += compat/glibc_compat.cpp
AM_LDFLAGS += $(COMPAT_LDFLAGS)
endif

# cli: shared between clashic-cli and clashic-qt
libclashic_cli_a_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
libclashic_cli_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libclashic_cli_a_SOURCES = \
  rpc/client.cpp \
  $(BITCOIN_CORE_H)

# seeder library
libclashic_seeder_a_CPPFLAGS = $(AM_CPPFLAGS) $(PIE_FLAGS) $(CLASHIC_SEEDER_INCLUDES)
libclashic_seeder_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libclashic_seeder_a_SOURCES = \
  seeder/bitcoin.cpp \
  seeder/bitcoin.h \
  seeder/compat.h \
  seeder/db.cpp \
  seeder/db.h \
  seeder/dns.c \
  seeder/dns.h \
  seeder/netbase.cpp \
  seeder/netbase.h \
  seeder/protocol.cpp \
  seeder/protocol.h \
  seeder/strlcpy.h \
  seeder/util.cpp \
  seeder/util.h

nodist_libclashic_util_a_SOURCES = $(srcdir)/obj/build.h
#

# clashicd binary #
clashicd_SOURCES = clashicd.cpp
clashicd_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
clashicd_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
clashicd_LDFLAGS = $(RELDFLAGS) $(AM_LDFLAGS) $(LIBTOOL_APP_LDFLAGS)

if TARGET_WINDOWS
clashicd_SOURCES += clashicd-res.rc
endif

clashicd_LDADD = \
  $(LIBCLASHIC_SERVER) \
  $(LIBCLASHIC_COMMON) \
  $(LIBUNIVALUE) \
  $(LIBCLASHIC_UTIL) \
  $(LIBCLASHIC_WALLET) \
  $(LIBCLASHIC_ZMQ) \
  $(LIBCLASHIC_CONSENSUS) \
  $(LIBCLASHIC_CRYPTO) \
  $(LIBLEVELDB) \
  $(LIBLEVELDB_SSE42) \
  $(LIBMEMENV) \
  $(LIBSECP256K1)

clashicd_LDADD += $(BOOST_LIBS) $(BDB_LIBS) $(SSL_LIBS) $(CRYPTO_LIBS) $(MINIUPNPC_LIBS) $(EVENT_PTHREADS_LIBS) $(EVENT_LIBS) $(ZMQ_LIBS)

# clashic-cli binary #
clashic_cli_SOURCES = clashic-cli.cpp
clashic_cli_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES) $(EVENT_CFLAGS)
clashic_cli_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
clashic_cli_LDFLAGS = $(RELDFLAGS) $(AM_LDFLAGS) $(LIBTOOL_APP_LDFLAGS)

if TARGET_WINDOWS
clashic_cli_SOURCES += clashic-cli-res.rc
endif

clashic_cli_LDADD = \
  $(LIBCLASHIC_CLI) \
  $(LIBUNIVALUE) \
  $(LIBCLASHIC_UTIL) \
  $(LIBCLASHIC_CRYPTO)

clashic_cli_LDADD += $(BOOST_LIBS) $(SSL_LIBS) $(CRYPTO_LIBS) $(EVENT_LIBS)
#

# clashic-seeder binary #
clashic_seeder_SOURCES = seeder/main.cpp
clashic_seeder_CPPFLAGS = $(AM_CPPFLAGS) $(CLASHIC_SEEDER_INCLUDES)
clashic_seeder_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
clashic_seeder_LDFLAGS = $(RELDFLAGS) $(AM_LDFLAGS) $(LIBTOOL_APP_LDFLAGS)

clashic_seeder_LDADD = \
  $(LIBCLASHIC_SEEDER) \
  $(LIBCLASHIC_UTIL) \
  $(LIBCLASHIC_CRYPTO)

clashic_seeder_LDADD += $(BOOST_LIBS) $(BDB_LIBS) $(SSL_LIBS) $(CRYPTO_LIBS) $(MINIUPNPC_LIBS) $(EVENT_PTHREADS_LIBS) $(EVENT_LIBS) $(ZMQ_LIBS)
#

# clashic-tx binary #
clashic_tx_SOURCES = clashic-tx.cpp
clashic_tx_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
clashic_tx_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
clashic_tx_LDFLAGS = $(RELDFLAGS) $(AM_LDFLAGS) $(LIBTOOL_APP_LDFLAGS)

if TARGET_WINDOWS
clashic_tx_SOURCES += clashic-tx-res.rc
endif

clashic_tx_LDADD = \
  $(LIBUNIVALUE) \
  $(LIBCLASHIC_COMMON) \
  $(LIBCLASHIC_UTIL) \
  $(LIBCLASHIC_CONSENSUS) \
  $(LIBCLASHIC_CRYPTO) \
  $(LIBSECP256K1)

clashic_tx_LDADD += $(BOOST_LIBS) $(CRYPTO_LIBS)
#

# clashicconsensus library #
if build_clashic_LIBS
include_HEADERS = script/clashicconsensus.h
libclashicconsensus_la_SOURCES = $(crypto_libclashic_crypto_a_SOURCES) $(libclashic_consensus_a_SOURCES)

if GLIBC_BACK_COMPAT
  libclashicconsensus_la_SOURCES += compat/glibc_compat.cpp
endif

libclashicconsensus_la_LDFLAGS = $(AM_LDFLAGS) $(RELDFLAGS)
libclashicconsensus_la_LIBADD = $(LIBSECP256K1)
libclashicconsensus_la_CPPFLAGS = $(AM_CPPFLAGS) -I$(builddir)/obj -I$(srcdir)/secp256k1/include $(BITCOIN_INCLUDES) -DBUILD_BITCOIN_INTERNAL
libclashicconsensus_la_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)

endif
#

CTAES_DIST =  crypto/ctaes/bench.c
CTAES_DIST += crypto/ctaes/ctaes.c
CTAES_DIST += crypto/ctaes/ctaes.h
CTAES_DIST += crypto/ctaes/README.md
CTAES_DIST += crypto/ctaes/test.c

CBLAKE2_DIST = crypto/cblake2/blake2-impl.h
CBLAKE2_DIST += crypto/cblake2/blake2.h
CBLAKE2_DIST += crypto/cblake2/blake2b-ref.c
CBLAKE2_DIST += crypto/cblake2/blake2bp-ref.c
CBLAKE2_DIST += crypto/cblake2/blake2s-ref.c
CBLAKE2_DIST += crypto/cblake2/blake2sp-ref.c
CBLAKE2_DIST += crypto/cblake2/blake2xb-ref.c
CBLAKE2_DIST += crypto/cblake2/blake2xs-ref.c
CBLAKE2_DIST += crypto/cblake2/genkat-c.c
CBLAKE2_DIST += crypto/cblake2/genkat-json.c

CLEANFILES = $(EXTRA_LIBRARIES)

CLEANFILES += *.gcda *.gcno
CLEANFILES += compat/*.gcda compat/*.gcno
CLEANFILES += consensus/*.gcda consensus/*.gcno
CLEANFILES += crypto/*.gcda crypto/*.gcno
CLEANFILES += policy/*.gcda policy/*.gcno
CLEANFILES += primitives/*.gcda primitives/*.gcno
CLEANFILES += script/*.gcda script/*.gcno
CLEANFILES += support/*.gcda support/*.gcno
CLEANFILES += univalue/*.gcda univalue/*.gcno
CLEANFILES += wallet/*.gcda wallet/*.gcno
CLEANFILES += wallet/test/*.gcda wallet/test/*.gcno
CLEANFILES += zmq/*.gcda zmq/*.gcno

DISTCLEANFILES = obj/build.h

EXTRA_DIST = $(CTAES_DIST) $(CBLAKE2_DIST)

clean-local:
	-$(MAKE) -C secp256k1 clean
	-$(MAKE) -C univalue clean
	-rm -f leveldb/*/*.gcda leveldb/*/*.gcno leveldb/helpers/memenv/*.gcda leveldb/helpers/memenv/*.gcno
	-rm -rf test/__pycache__

.rc.o:
	@test -f $(WINDRES)
	## FIXME: How to get the appropriate modulename_CPPFLAGS in here?
	$(AM_V_GEN) $(WINDRES) $(DEFS) $(DEFAULT_INCLUDES) $(INCLUDES) $(CPPFLAGS) -DWINDRES_PREPROC -i $< -o $@

.mm.o:
	$(AM_V_CXX) $(OBJCXX) $(DEFS) $(DEFAULT_INCLUDES) $(INCLUDES) $(AM_CPPFLAGS) \
	  $(CPPFLAGS) $(AM_CXXFLAGS) $(QT_INCLUDES) $(AM_CXXFLAGS) $(PIE_FLAGS) $(CXXFLAGS) -c -o $@ $<

check-symbols: $(bin_PROGRAMS)
if GLIBC_BACK_COMPAT
	@echo "Checking glibc back compat..."
	$(AM_V_at) READELF=$(READELF) CPPFILT=$(CPPFILT) $(top_srcdir)/contrib/devtools/symbol-check.py < $(bin_PROGRAMS)
endif

check-security: $(bin_PROGRAMS)
if HARDEN
	@echo "Checking binary security..."
	$(AM_V_at) READELF=$(READELF) OBJDUMP=$(OBJDUMP) $(top_srcdir)/contrib/devtools/security-check.py < $(bin_PROGRAMS)
endif

%.pb.cc %.pb.h: %.proto
	@test -f $(PROTOC)
	$(AM_V_GEN) $(PROTOC) --cpp_out=$(@D) --proto_path=$(<D) $<

if EMBEDDED_LEVELDB
include Makefile.leveldb.include
endif

if ENABLE_TESTS
include Makefile.test.include
endif

if ENABLE_BENCH
include Makefile.bench.include
endif

if ENABLE_QT
include Makefile.qt.include
endif

if ENABLE_QT_TESTS
include Makefile.qttest.include
endif
