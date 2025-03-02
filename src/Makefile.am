# Copyright (c) 2013-2016 The Bitcoin Core developers
# Copyright (c) 2014-2018 The Dash Core developers
# Distributed under the MIT software license, see the accompanying
# file COPYING or http://www.opensource.org/licenses/mit-license.php.

# Pattern rule to print variables, e.g. make print-top_srcdir
print-%: FORCE
	@echo '$*'='$($*)'

DIST_SUBDIRS = secp256k1

AM_LDFLAGS = $(LIBTOOL_LDFLAGS) $(HARDENED_LDFLAGS) $(GPROF_LDFLAGS) $(SANITIZER_LDFLAGS)
AM_CXXFLAGS = $(DEBUG_CXXFLAGS) $(HARDENED_CXXFLAGS) $(WARN_CXXFLAGS) $(NOWARN_CXXFLAGS) $(ERROR_CXXFLAGS) $(GPROF_CXXFLAGS) $(SANITIZER_CXXFLAGS)
AM_CPPFLAGS = $(DEBUG_CPPFLAGS) $(HARDENED_CPPFLAGS)
AM_LIBTOOLFLAGS = --preserve-dup-deps
PTHREAD_FLAGS = $(PTHREAD_CFLAGS) $(PTHREAD_LIBS)
EXTRA_LIBRARIES =

if ENABLE_STACKTRACES
if ENABLE_CRASH_HOOKS
if CRASH_HOOKS_WRAPPED_CXX_ABI
# Wrap internal C++ ABI's so that we can attach stacktraces to exceptions
LDFLAGS_WRAP_EXCEPTIONS = -Wl,-wrap,__cxa_allocate_exception -Wl,-wrap,__cxa_free_exception
if TARGET_WINDOWS
LDFLAGS_WRAP_EXCEPTIONS += -Wl,-wrap,_assert -Wl,-wrap,_wassert
else
LDFLAGS_WRAP_EXCEPTIONS += -Wl,-wrap,__assert_fail
endif
endif
endif

if TARGET_WINDOWS
BACKTRACE_LIB = -ldbghelp -lbacktrace
else
BACKTRACE_LIB = -lbacktrace
endif
endif #ENABLE_STACKTRACES

BITCOIN_INCLUDES=-I$(builddir) -I$(srcdir)/secp256k1/include -I$(srcdir)/$(UNIVALUE_INCLUDE_DIR_INT) $(BDB_CPPFLAGS) $(BOOST_CPPFLAGS) $(LEVELDB_CPPFLAGS)
BITCOIN_INCLUDES+=-I$(srcdir)/dashbls/include -I$(srcdir)/dashbls/depends/relic/include -I$(srcdir)/dashbls/depends/minialloc/include
BITCOIN_INCLUDES+=-I$(srcdir)/immer

LIBOGVA_SERVER=libogva_server.a
LIBOGVA_COMMON=libogva_common.a
LIBOGVA_CONSENSUS=libogva_consensus.a
LIBOGVA_CLI=libogva_cli.a
LIBOGVA_UTIL=libogva_util.a
LIBOGVA_CRYPTO_BASE=crypto/libogva_crypto_base.a
LIBOGVAQT=qt/libogvaqt.a
LIBDASHBLS=dashbls/libdashbls.la
LIBSECP256K1=secp256k1/libsecp256k1.la

if ENABLE_ZMQ
LIBOGVA_ZMQ=libogva_zmq.a
endif
if BUILD_BITCOIN_LIBS
LIBOGVACONSENSUS=libogvaconsensus.la
endif
if ENABLE_WALLET
LIBOGVA_WALLET=libogva_wallet.a
LIBOGVA_WALLET_TOOL=libogva_wallet_tool.a
endif

LIBOGVA_CRYPTO= $(LIBOGVA_CRYPTO_BASE)
if ENABLE_SSE41
LIBOGVA_CRYPTO_SSE41 = crypto/libogva_crypto_sse41.a
LIBOGVA_CRYPTO += $(LIBOGVA_CRYPTO_SSE41)
endif
if ENABLE_AVX2
LIBOGVA_CRYPTO_AVX2 = crypto/libogva_crypto_avx2.a
LIBOGVA_CRYPTO += $(LIBOGVA_CRYPTO_AVX2)
endif
if ENABLE_X86_SHANI
LIBOGVA_CRYPTO_X86_SHANI = crypto/libogva_crypto_x86_shani.a
LIBOGVA_CRYPTO += $(LIBOGVA_CRYPTO_X86_SHANI)
endif
if ENABLE_ARM_SHANI
LIBOGVA_CRYPTO_ARM_SHANI = crypto/libogva_crypto_arm_shani.a
LIBOGVA_CRYPTO += $(LIBOGVA_CRYPTO_ARM_SHANI)
endif

$(LIBDASHBLS):
	$(AM_V_at)$(MAKE) $(AM_MAKEFLAGS) -C $(@D)

$(LIBSECP256K1): $(wildcard secp256k1/src/*.h) $(wildcard secp256k1/src/*.c) $(wildcard secp256k1/include/*)
	$(AM_V_at)$(MAKE) $(AM_MAKEFLAGS) -C $(@D) $(@F)

# Make is not made aware of per-object dependencies to avoid limiting building parallelization
# But to build the less dependent modules first, we manually select their order here:
EXTRA_LIBRARIES += \
  $(LIBOGVA_CRYPTO) \
  $(LIBOGVA_UTIL) \
  $(LIBOGVA_COMMON) \
  $(LIBOGVA_CONSENSUS) \
  $(LIBOGVA_SERVER) \
  $(LIBOGVA_CLI) \
  $(LIBOGVA_WALLET) \
  $(LIBOGVA_WALLET_TOOL) \
  $(LIBOGVA_ZMQ)

lib_LTLIBRARIES = $(LIBOGVACONSENSUS)
noinst_LTLIBRARIES =

bin_PROGRAMS =
noinst_PROGRAMS =
TESTS =
BENCHMARKS =

if BUILD_BITCOIND
  bin_PROGRAMS += ogvad
endif

if BUILD_BITCOIN_NODE
  bin_PROGRAMS += ogva-node
endif

if BUILD_BITCOIN_CLI
  bin_PROGRAMS += ogva-cli
endif
if BUILD_BITCOIN_TX
  bin_PROGRAMS += ogva-tx
endif
if ENABLE_WALLET
if BUILD_BITCOIN_WALLET
  bin_PROGRAMS += ogva-wallet
endif
endif

.PHONY: FORCE check-symbols check-security
# ogva core #
BITCOIN_CORE_H = \
  addrdb.h \
  addressindex.h \
  spentindex.h \
  addrman.h \
  attributes.h \
  banman.h \
  base58.h \
  batchedlogger.h \
  bech32.h \
  bip39.h \
  bip39_english.h \
  blockencodings.h \
  bloom.h \
  cachemap.h \
  cachemultimap.h \
  blockfilter.h \
  bloom.h \
  chain.h \
  chainparams.h \
  chainparamsbase.h \
  chainparamsseeds.h \
  checkqueue.h \
  clientversion.h \
  coinjoin/coinjoin.h \
  coinjoin/client.h \
  coinjoin/common.h \
  coinjoin/context.h \
  coinjoin/options.h \
  coinjoin/server.h \
  coinjoin/util.h \
  coins.h \
  compat.h \
  compat/assumptions.h \
  compat/byteswap.h \
  compat/cpuid.h \
  compat/endian.h \
  compressor.h \
  context.h \
  consensus/consensus.h \
  consensus/tx_check.h \
  consensus/tx_verify.h \
  core_io.h \
  core_memusage.h \
  cuckoocache.h \
  ctpl_stl.h \
  cxxtimer.hpp \
  dbwrapper.h \
  deploymentinfo.h \
  deploymentstatus.h \
  evo/assetlocktx.h \
  evo/dmn_types.h \
  evo/cbtx.h \
  evo/creditpool.h \
  evo/deterministicmns.h \
  evo/dmnstate.h \
  evo/evodb.h \
  evo/mnauth.h \
  evo/mnhftx.h \
  evo/providertx.h \
  evo/simplifiedmns.h \
  evo/specialtx.h \
  evo/specialtxman.h \
  dsnotificationinterface.h \
  governance/governance.h \
  governance/classes.h \
  governance/common.h \
  governance/exceptions.h \
  governance/object.h \
  governance/validators.h \
  governance/vote.h \
  governance/votedb.h \
  gsl/assert.h \
  gsl/pointers.h \
  flat-database.h \
  hdchain.h \
  flatfile.h \
  fs.h \
  httprpc.h \
  httpserver.h \
  i2p.h \
  index/base.h \
  index/blockfilterindex.h \
  index/coinstatsindex.h \
  index/disktxpos.h \
  index/txindex.h \
  indirectmap.h \
  init.h \
  interfaces/chain.h \
  interfaces/coinjoin.h \
  interfaces/handler.h \
  interfaces/node.h \
  interfaces/wallet.h \
  key.h \
  key_io.h \
  limitedmap.h \
  llmq/blockprocessor.h \
  llmq/chainlocks.h \
  llmq/clsig.h \
  llmq/commitment.h \
  llmq/context.h \
  llmq/debug.h \
  llmq/dkgsession.h \
  llmq/dkgsessionhandler.h \
  llmq/dkgsessionmgr.h \
  llmq/ehf_signals.cpp \
  llmq/ehf_signals.h \
  llmq/instantsend.h \
  llmq/options.h \
  llmq/params.h \
  llmq/quorums.h \
  llmq/signing.h \
  llmq/signing_shares.h \
  llmq/snapshot.h \
  llmq/utils.h \
  logging.h \
  logging/timer.h \
  mapport.h \
  masternode/node.h \
  masternode/meta.h \
  masternode/payments.h \
  masternode/sync.h \
  masternode/utils.h \
  memusage.h \
  merkleblock.h \
  messagesigner.h \
  miner.h \
  net.h \
  net_permissions.h \
  net_processing.h \
  net_types.h \
  netaddress.h \
  netbase.h \
  netfulfilledman.h \
  netmessagemaker.h \
  node/blockstorage.h \
  node/coin.h \
  node/coinstats.h \
  node/context.h \
  node/psbt.h \
  node/transaction.h \
  node/ui_interface.h \
  node/utxo_snapshot.h \
  noui.h \
  outputtype.h \
  policy/feerate.h \
  policy/fees.h \
  policy/packages.h \
  policy/policy.h \
  policy/settings.h \
  pow.h \
  protocol.h \
  psbt.h \
  random.h \
  randomenv.h \
  reverse_iterator.h \
  rpc/blockchain.h \
  rpc/client.h \
  rpc/mining.h \
  rpc/protocol.h \
  rpc/rawtransaction_util.h \
  rpc/register.h \
  rpc/request.h \
  rpc/server.h \
  rpc/util.h \
  saltedhasher.h \
  scheduler.h \
  script/descriptor.h \
  script/keyorigin.h \
  script/sigcache.h \
  script/sign.h \
  script/signingprovider.h \
  script/standard.h \
  shutdown.h \
  source_location.h \
  spork.h \
  stacktraces.h \
  streams.h \
  statsd_client.h \
  support/allocators/mt_pooled_secure.h \
  support/allocators/pooled_secure.h \
  support/allocators/secure.h \
  support/allocators/zeroafterfree.h \
  support/cleanse.h \
  support/events.h \
  support/lockedpool.h \
  sync.h \
  timestampindex.h \
  threadinterrupt.h \
  threadsafety.h \
  timedata.h \
  torcontrol.h \
  txdb.h \
  txmempool.h \
  undo.h \
  unordered_lru_cache.h \
  util/bip32.h \
  util/bytevectorhash.h \
  util/check.h \
  util/enumerate.h \
  util/epochguard.h \
  util/error.h \
  util/expected.h \
  util/fastrange.h \
  util/fees.h \
  util/golombrice.h \
  util/hasher.h \
  util/hash_type.h \
  util/irange.h \
  util/asmap.h \
  util/getuniquepath.h \
  util/macros.h \
  util/message.h \
  util/moneystr.h \
  util/overflow.h \
  util/ranges.h \
  util/readwritefile.h \
  util/underlying.h \
  util/serfloat.h \
  util/settings.h \
  util/ranges_set.h \
  util/sock.h \
  util/string.h \
  util/spanparsing.h \
  util/system.h \
  util/time.h \
  util/thread.h \
  util/threadnames.h \
  util/trace.h \
  util/translation.h \
  util/ui_change_type.h \
  util/url.h \
  util/vector.h \
  validation.h \
  validationinterface.h \
  versionbits.h \
  wallet/bdb.h \
  wallet/coincontrol.h \
  wallet/coinselection.h \
  wallet/context.h \
  wallet/crypter.h \
  wallet/db.h \
  wallet/fees.h \
  wallet/ismine.h \
  wallet/load.h \
  wallet/rpcwallet.h \
  wallet/salvage.h \
  wallet/scriptpubkeyman.h \
  wallet/sqlite.h \
  wallet/wallet.h \
  wallet/walletdb.h \
  wallet/wallettool.h \
  wallet/walletutil.h \
  walletinitinterface.h \
  warnings.h \
  zmq/zmqabstractnotifier.h \
  zmq/zmqnotificationinterface.h \
  zmq/zmqpublishnotifier.h \
  zmq/zmqrpc.h \
  zmq/zmqutil.h


obj/build.h: FORCE
	@$(MKDIR_P) $(builddir)/obj
	@$(top_srcdir)/share/genbuild.sh "$(abs_top_builddir)/src/obj/build.h" \
	  "$(abs_top_srcdir)"
libogva_util_a-clientversion.$(OBJEXT): obj/build.h

# server: shared between ogvad and ogva-qt
# Contains code accessing mempool and chain state that is meant to be separated
# from wallet and gui code (see node/README.md). Shared code should go in
# libogva_common or libogva_util libraries, instead.
libogva_server_a_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES) $(MINIUPNPC_CPPFLAGS) $(NATPMP_CPPFLAGS) $(EVENT_CFLAGS) $(EVENT_PTHREADS_CFLAGS)
libogva_server_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libogva_server_a_SOURCES = \
  addrdb.cpp \
  addressindex.cpp \
  addrman.cpp \
  banman.cpp \
  batchedlogger.cpp \
  blockencodings.cpp \
  blockfilter.cpp \
  chain.cpp \
  coinjoin/coinjoin.cpp \
  coinjoin/context.cpp \
  coinjoin/server.cpp \
  consensus/tx_verify.cpp \
  dbwrapper.cpp \
  deploymentstatus.cpp \
  dsnotificationinterface.cpp \
  evo/assetlocktx.cpp \
  evo/cbtx.cpp \
  evo/creditpool.cpp \
  evo/deterministicmns.cpp \
  evo/dmnstate.cpp \
  evo/evodb.cpp \
  evo/mnauth.cpp \
  evo/mnhftx.cpp \
  evo/providertx.cpp \
  evo/simplifiedmns.cpp \
  evo/specialtx.cpp \
  evo/specialtxman.cpp \
  flatfile.cpp \
  governance/classes.cpp \
  governance/exceptions.cpp \
  governance/governance.cpp \
  governance/object.cpp \
  governance/validators.cpp \
  governance/vote.cpp \
  governance/votedb.cpp \
  gsl/assert.cpp \
  httprpc.cpp \
  httpserver.cpp \
  i2p.cpp \
  index/base.cpp \
  index/blockfilterindex.cpp \
  index/coinstatsindex.cpp \
  index/txindex.cpp \
  init.cpp \
  llmq/quorums.cpp \
  llmq/blockprocessor.cpp \
  llmq/commitment.cpp \
  llmq/chainlocks.cpp \
  llmq/clsig.cpp \
  llmq/debug.cpp \
  llmq/dkgsessionhandler.cpp \
  llmq/dkgsessionmgr.cpp \
  llmq/dkgsession.cpp \
  llmq/context.cpp \
  llmq/instantsend.cpp \
  llmq/options.cpp \
  llmq/snapshot.cpp \
  llmq/signing.cpp \
  llmq/signing_shares.cpp \
  llmq/utils.cpp \
  mapport.cpp \
  masternode/node.cpp \
  masternode/meta.cpp \
  masternode/payments.cpp \
  masternode/sync.cpp \
  masternode/utils.cpp \
  messagesigner.cpp \
  miner.cpp \
  net.cpp \
  netfulfilledman.cpp \
  net_processing.cpp \
  node/blockstorage.cpp \
  node/coin.cpp \
  node/coinstats.cpp \
  node/context.cpp \
  node/interfaces.cpp \
  node/psbt.cpp \
  node/transaction.cpp \
  node/ui_interface.cpp \
  noui.cpp \
  policy/fees.cpp \
  policy/packages.cpp \
  policy/policy.cpp \
  policy/settings.cpp \
  pow.cpp \
  rest.cpp \
  rpc/blockchain.cpp \
  rpc/coinjoin.cpp \
  rpc/evo.cpp \
  rpc/masternode.cpp \
  rpc/governance.cpp \
  rpc/mining.cpp \
  rpc/misc.cpp \
  rpc/net.cpp \
  rpc/quorums.cpp \
  rpc/rawtransaction.cpp \
  rpc/server.cpp \
  script/sigcache.cpp \
  shutdown.cpp \
  spork.cpp \
  statsd_client.cpp \
  timedata.cpp \
  torcontrol.cpp \
  txdb.cpp \
  txmempool.cpp \
  validation.cpp \
  validationinterface.cpp \
  versionbits.cpp \
  $(BITCOIN_CORE_H)

if ENABLE_WALLET
libogva_server_a_SOURCES += wallet/init.cpp
endif
if !ENABLE_WALLET
libogva_server_a_SOURCES += dummywallet.cpp
endif

if ENABLE_ZMQ
libogva_zmq_a_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES) $(ZMQ_CFLAGS)
libogva_zmq_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libogva_zmq_a_SOURCES = \
  zmq/zmqabstractnotifier.cpp \
  zmq/zmqnotificationinterface.cpp \
  zmq/zmqpublishnotifier.cpp \
  zmq/zmqrpc.cpp \
  zmq/zmqutil.cpp
endif


# wallet: shared between ogvad and ogva-qt, but only linked
# when wallet enabled
libogva_wallet_a_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES) $(SQLITE_CFLAGS)
libogva_wallet_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libogva_wallet_a_SOURCES = \
  coinjoin/client.cpp \
  coinjoin/interfaces.cpp \
  coinjoin/util.cpp \
  hdchain.cpp \
  wallet/coincontrol.cpp \
  wallet/context.cpp \
  wallet/crypter.cpp \
  wallet/db.cpp \
  wallet/fees.cpp \
  wallet/interfaces.cpp \
  wallet/load.cpp \
  wallet/rpcdump.cpp \
  wallet/rpcwallet.cpp \
  wallet/scriptpubkeyman.cpp \
  wallet/wallet.cpp \
  wallet/walletdb.cpp \
  wallet/walletutil.cpp \
  wallet/coinselection.cpp \
  $(BITCOIN_CORE_H)

if USE_SQLITE
libogva_wallet_a_SOURCES += wallet/sqlite.cpp
endif
if USE_BDB
libogva_wallet_a_SOURCES += wallet/bdb.cpp wallet/salvage.cpp
endif

libogva_wallet_tool_a_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
libogva_wallet_tool_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libogva_wallet_tool_a_SOURCES = \
  wallet/wallettool.cpp \
  $(BITCOIN_CORE_H)

# crypto primitives library
crypto_libogva_crypto_base_a_CPPFLAGS = $(AM_CPPFLAGS) $(PIC_FLAGS)
crypto_libogva_crypto_base_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS) $(PIC_FLAGS)
crypto_libogva_crypto_base_a_SOURCES = \
  crypto/aes.cpp \
  crypto/aes.h \
  crypto/chacha_poly_aead.h \
  crypto/chacha_poly_aead.cpp \
  crypto/chacha20.h \
  crypto/chacha20.cpp \
  crypto/common.h \
  crypto/hkdf_sha256_32.cpp \
  crypto/hkdf_sha256_32.h \
  crypto/hmac_sha256.cpp \
  crypto/hmac_sha256.h \
  crypto/hmac_sha512.cpp \
  crypto/hmac_sha512.h \
  crypto/muhash.h \
  crypto/muhash.cpp \
  crypto/poly1305.h \
  crypto/poly1305.cpp \
  crypto/pkcs5_pbkdf2_hmac_sha512.cpp \
  crypto/pkcs5_pbkdf2_hmac_sha512.h \
  crypto/ripemd160.cpp \
  crypto/aes_helper.c \
  crypto/ripemd160.h \
  crypto/sha1.cpp \
  crypto/sha1.h \
  crypto/sha256.cpp \
  crypto/sha256.h \
  crypto/sha3.cpp \
  crypto/sha3.h \
  crypto/sha512.cpp \
  crypto/sha512.h \
  crypto/siphash.cpp \
  crypto/siphash.h

if USE_ASM
crypto_libogva_crypto_base_a_SOURCES += crypto/sha256_sse4.cpp
endif

crypto_libogva_crypto_sse41_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
crypto_libogva_crypto_sse41_a_CPPFLAGS = $(AM_CPPFLAGS)
crypto_libogva_crypto_sse41_a_CXXFLAGS += $(SSE41_CXXFLAGS)
crypto_libogva_crypto_sse41_a_CPPFLAGS += -DENABLE_SSE41
crypto_libogva_crypto_sse41_a_SOURCES = crypto/sha256_sse41.cpp

crypto_libogva_crypto_avx2_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
crypto_libogva_crypto_avx2_a_CPPFLAGS = $(AM_CPPFLAGS)
crypto_libogva_crypto_avx2_a_CXXFLAGS += $(AVX2_CXXFLAGS)
crypto_libogva_crypto_avx2_a_CPPFLAGS += -DENABLE_AVX2
crypto_libogva_crypto_avx2_a_SOURCES = crypto/sha256_avx2.cpp

# neoscrypt
crypto_libogva_crypto_base_a_SOURCES += \
  crypto/neoscrypt.c \
  crypto/neoscrypt.h


# x11
crypto_libogva_crypto_base_a_SOURCES += \
  crypto/blake.c \
  crypto/bmw.c \
  crypto/cubehash.c \
  crypto/echo.c \
  crypto/groestl.c \
  crypto/jh.c \
  crypto/keccak.c \
  crypto/luffa.c \
  crypto/shavite.c \
  crypto/simd.c \
  crypto/skein.c \
  crypto/sph_blake.h \
  crypto/sph_bmw.h \
  crypto/sph_cubehash.h \
  crypto/sph_echo.h \
  crypto/sph_groestl.h \
  crypto/sph_jh.h \
  crypto/sph_keccak.h \
  crypto/sph_luffa.h \
  crypto/sph_shavite.h \
  crypto/sph_simd.h \
  crypto/sph_skein.h \
  crypto/sph_types.h

crypto_libogva_crypto_x86_shani_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
crypto_libogva_crypto_x86_shani_a_CPPFLAGS = $(AM_CPPFLAGS)
crypto_libogva_crypto_x86_shani_a_CXXFLAGS += $(X86_SHANI_CXXFLAGS)
crypto_libogva_crypto_x86_shani_a_CPPFLAGS += -DENABLE_X86_SHANI
crypto_libogva_crypto_x86_shani_a_SOURCES = crypto/sha256_x86_shani.cpp

crypto_libogva_crypto_arm_shani_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
crypto_libogva_crypto_arm_shani_a_CPPFLAGS = $(AM_CPPFLAGS)
crypto_libogva_crypto_arm_shani_a_CXXFLAGS += $(ARM_SHANI_CXXFLAGS)
crypto_libogva_crypto_arm_shani_a_CPPFLAGS += -DENABLE_ARM_SHANI
crypto_libogva_crypto_arm_shani_a_SOURCES = crypto/sha256_arm_shani.cpp

# consensus: shared between all executables that validate any consensus rules.
libogva_consensus_a_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
libogva_consensus_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libogva_consensus_a_SOURCES = \
  amount.h \
  arith_uint256.cpp \
  arith_uint256.h \
  bls/bls.cpp \
  bls/bls.h \
  consensus/merkle.cpp \
  consensus/merkle.h \
  consensus/params.h \
  consensus/tx_check.cpp \
  consensus/validation.h \
  devfee_payment.h \
  hash.cpp \
  hash.h \
  prevector.h \
  primitives/block.cpp \
  primitives/block.h \
  primitives/transaction.cpp \
  primitives/transaction.h \
  pubkey.cpp \
  pubkey.h \
  script/bitcoinconsensus.cpp \
  script/interpreter.cpp \
  script/interpreter.h \
  script/script.cpp \
  script/script.h \
  script/script_error.cpp \
  script/script_error.h \
  serialize.h \
  span.h \
  streams.h \
  tinyformat.h \
  uint256.cpp \
  uint256.h \
  util/strencodings.cpp \
  util/strencodings.h \
  util/string.cpp \
  version.h

# common: shared between ogvad, and ogva-qt and non-server tools
libogva_common_a_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
libogva_common_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libogva_common_a_SOURCES = \
  base58.cpp \
  bech32.cpp \
  bip39.cpp \
  bloom.cpp \
  chainparams.cpp \
  coins.cpp \
  compressor.cpp \
  core_read.cpp \
  core_write.cpp \
  deploymentinfo.cpp \
  devfee_payment.cpp \
  devfee_payment.h \
  governance/common.cpp \
  key.cpp \
  key_io.cpp \
  merkleblock.cpp \
  netaddress.cpp \
  netbase.cpp \
  net_permissions.cpp \
  outputtype.cpp \
  policy/feerate.cpp \
  policy/policy.cpp \
  protocol.cpp \
  psbt.cpp \
  rpc/rawtransaction_util.cpp \
  rpc/util.cpp \
  saltedhasher.cpp \
  scheduler.cpp \
  script/descriptor.cpp \
  script/sign.cpp \
  script/signingprovider.cpp \
  script/standard.cpp \
  warnings.cpp \
  $(BITCOIN_CORE_H)

# util: shared between all executables.
libogva_util_a_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
libogva_util_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libogva_util_a_SOURCES = \
  bls/bls_batchverifier.h \
  bls/bls_ies.cpp \
  bls/bls_ies.h \
  bls/bls_worker.cpp \
  bls/bls_worker.h \
  coinjoin/common.cpp \
  coinjoin/options.cpp \
  support/lockedpool.cpp \
  chainparamsbase.cpp \
  clientversion.cpp \
  compat/strnlen.cpp \
  fs.cpp \
  interfaces/handler.cpp \
  logging.cpp \
  random.cpp \
  randomenv.cpp \
  rpc/request.cpp \
  stacktraces.cpp \
  support/cleanse.cpp \
  sync.cpp \
  threadinterrupt.cpp \
  util/asmap.cpp \
  util/bip32.cpp \
  util/bytevectorhash.cpp \
  util/check.cpp \
  util/error.cpp \
  util/fees.cpp \
  util/hasher.cpp \
  util/getuniquepath.cpp \
  util/sock.cpp \
  util/system.cpp \
  util/message.cpp \
  util/moneystr.cpp \
  util/readwritefile.cpp \
  util/settings.cpp \
  util/ranges_set.cpp \
  util/spanparsing.cpp \
  util/strencodings.cpp \
  util/time.cpp \
  util/serfloat.cpp \
  util/string.cpp \
  util/thread.cpp \
  util/threadnames.cpp \
  $(BITCOIN_CORE_H)

if USE_LIBEVENT
libogva_util_a_SOURCES += util/url.cpp
endif

# cli: shared between ogva-cli and ogva-qt
libogva_cli_a_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
libogva_cli_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libogva_cli_a_SOURCES = \
  compat/stdin.h \
  compat/stdin.cpp \
  rpc/client.cpp \
  $(BITCOIN_CORE_H)

nodist_libogva_util_a_SOURCES = $(srcdir)/obj/build.h
#

# ogvad & ogva-node binaries #
bitcoin_daemon_sources = bitcoind.cpp
bitcoin_bin_cppflags = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
bitcoin_bin_cxxflags = $(AM_CXXFLAGS) $(PIE_FLAGS)
bitcoin_bin_ldflags = $(LDFLAGS_WRAP_EXCEPTIONS) $(RELDFLAGS) $(AM_LDFLAGS) $(LIBTOOL_APP_LDFLAGS) $(PTHREAD_FLAGS)

if TARGET_WINDOWS
bitcoin_daemon_sources += ogvad-res.rc
endif

bitcoin_bin_ldadd = \
  $(LIBOGVA_WALLET) \
  $(LIBOGVA_COMMON) \
  $(LIBOGVA_UTIL) \
  $(LIBUNIVALUE) \
  $(LIBOGVA_ZMQ) \
  $(LIBOGVA_CONSENSUS) \
  $(LIBOGVA_CRYPTO) \
  $(LIBDASHBLS) \
  $(LIBLEVELDB) \
  $(LIBLEVELDB_SSE42) \
  $(LIBMEMENV) \
  $(LIBSECP256K1)

bitcoin_bin_ldadd += $(BACKTRACE_LIB) $(BOOST_LIBS) $(BDB_LIBS) $(MINIUPNPC_LIBS) $(NATPMP_LIBS) $(SQLITE_LIBS) $(EVENT_PTHREADS_LIBS) $(EVENT_LIBS) $(ZMQ_LIBS) $(GMP_LIBS)

ogvad_SOURCES = $(bitcoin_daemon_sources)
ogvad_CPPFLAGS = $(bitcoin_bin_cppflags)
ogvad_CXXFLAGS = $(bitcoin_bin_cxxflags)
ogvad_LDFLAGS = $(bitcoin_bin_ldflags)
ogvad_LDADD = $(LIBOGVA_SERVER) $(bitcoin_bin_ldadd)

ogva_node_SOURCES = $(bitcoin_daemon_sources)
ogva_node_CPPFLAGS = $(bitcoin_bin_cppflags)
ogva_node_CXXFLAGS = $(bitcoin_bin_cxxflags)
ogva_node_LDFLAGS = $(bitcoin_bin_ldflags)
ogva_node_LDADD = $(LIBOGVA_SERVER) $(bitcoin_bin_ldadd)

# ogva-cli binary #
ogva_cli_SOURCES = bitcoin-cli.cpp
ogva_cli_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES) $(EVENT_CFLAGS)
ogva_cli_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
ogva_cli_LDFLAGS = $(LDFLAGS_WRAP_EXCEPTIONS) $(RELDFLAGS) $(AM_LDFLAGS) $(LIBTOOL_APP_LDFLAGS) $(PTHREAD_FLAGS)

if TARGET_WINDOWS
ogva_cli_SOURCES += ogva-cli-res.rc
endif

ogva_cli_LDADD = \
  $(LIBOGVA_CLI) \
  $(LIBUNIVALUE) \
  $(LIBOGVA_UTIL) \
  $(LIBOGVA_CRYPTO)
ogva_cli_LDADD += $(BACKTRACE_LIB) $(BOOST_LIBS) $(EVENT_LIBS) $(GMP_LIBS)
#

# ogva-tx binary #
ogva_tx_SOURCES = bitcoin-tx.cpp
ogva_tx_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
ogva_tx_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
ogva_tx_LDFLAGS = $(LDFLAGS_WRAP_EXCEPTIONS) $(RELDFLAGS) $(AM_LDFLAGS) $(LIBTOOL_APP_LDFLAGS) $(PTHREAD_FLAGS)

if TARGET_WINDOWS
ogva_tx_SOURCES += ogva-tx-res.rc
endif

ogva_tx_LDADD = \
  $(LIBUNIVALUE) \
  $(LIBOGVA_COMMON) \
  $(LIBOGVA_UTIL) \
  $(LIBOGVA_CONSENSUS) \
  $(LIBOGVA_CRYPTO) \
  $(LIBDASHBLS) \
  $(LIBSECP256K1)

ogva_tx_LDADD += $(BACKTRACE_LIB) $(BOOST_LIBS) $(GMP_LIBS)
#

# ogva-wallet binary #
ogva_wallet_SOURCES = bitcoin-wallet.cpp
ogva_wallet_CPPFLAGS = $(bitcoin_bin_cppflags)
ogva_wallet_CXXFLAGS = $(bitcoin_bin_cxxflags)
ogva_wallet_LDFLAGS = $(bitcoin_bin_ldflags)
ogva_wallet_LDADD = $(LIBOGVA_WALLET_TOOL) $(bitcoin_bin_ldadd)

if TARGET_WINDOWS
ogva_wallet_SOURCES += ogva-wallet-res.rc
endif
#

# ogvaconsensus library #
if BUILD_BITCOIN_LIBS
include_HEADERS = script/bitcoinconsensus.h
libogvaconsensus_la_SOURCES = support/cleanse.cpp $(crypto_libogva_crypto_base_a_SOURCES) $(libogva_consensus_a_SOURCES)

libogvaconsensus_la_LDFLAGS = $(AM_LDFLAGS) -no-undefined $(RELDFLAGS)
libogvaconsensus_la_LIBADD = $(LIBDASHBLS) $(LIBSECP256K1) $(GMP_LIBS)
libogvaconsensus_la_CPPFLAGS = $(AM_CPPFLAGS) -I$(builddir)/obj -I$(srcdir)/secp256k1/include -DBUILD_BITCOIN_INTERNAL
libogvaconsensus_la_CPPFLAGS += -I$(srcdir)/dashbls/include -I$(srcdir)/dashbls/depends/relic/include -I$(srcdir)/dashbls/depends/minialloc/include
libogvaconsensus_la_CPPFLAGS += -I$(srcdir)/immer
libogvaconsensus_la_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)

endif
#

CTAES_DIST =  crypto/ctaes/bench.c
CTAES_DIST += crypto/ctaes/ctaes.c
CTAES_DIST += crypto/ctaes/ctaes.h
CTAES_DIST += crypto/ctaes/README.md
CTAES_DIST += crypto/ctaes/test.c

CLEANFILES = $(EXTRA_LIBRARIES)

CLEANFILES += *.gcda *.gcno
CLEANFILES += compat/*.gcda compat/*.gcno
CLEANFILES += consensus/*.gcda consensus/*.gcno
CLEANFILES += crc32c/src/*.gcda crc32c/src/*.gcno
CLEANFILES += crypto/*.gcda crypto/*.gcno
CLEANFILES += index/*.gcda index/*.gcno
CLEANFILES += interfaces/*.gcda interfaces/*.gcno
CLEANFILES += node/*.gcda node/*.gcno
CLEANFILES += policy/*.gcda policy/*.gcno
CLEANFILES += primitives/*.gcda primitives/*.gcno
CLEANFILES += rpc/*.gcda rpc/*.gcno
CLEANFILES += script/*.gcda script/*.gcno
CLEANFILES += support/*.gcda support/*.gcno
CLEANFILES += univalue/*.gcda univalue/*.gcno
CLEANFILES += util/*.gcda util/*.gcno
CLEANFILES += wallet/*.gcda wallet/*.gcno
CLEANFILES += wallet/test/*.gcda wallet/test/*.gcno
CLEANFILES += zmq/*.gcda zmq/*.gcno
CLEANFILES += obj/build.h

IMMER_DIST = immer/immer
LIBDASHBLS_DIST = dashbls

EXTRA_DIST = \
  $(CTAES_DIST)   \
  $(IMMER_DIST)   \
  $(LIBDASHBLS_DIST)

config/bitcoin-config.h: config/stamp-h1
	@$(MAKE) -C $(top_builddir) $(subdir)/$(@)
config/stamp-h1: $(top_srcdir)/$(subdir)/config/bitcoin-config.h.in $(top_builddir)/config.status
	$(AM_V_at)$(MAKE) -C $(top_builddir) $(subdir)/$(@)
$(top_srcdir)/$(subdir)/config/bitcoin-config.h.in:  $(am__configure_deps)
	$(AM_V_at)$(MAKE) -C $(top_srcdir) $(subdir)/config/bitcoin-config.h.in


clean-local:
	-$(MAKE) -C dashbls clean
	-$(MAKE) -C secp256k1 clean
	-rm -f leveldb/*/*.gcda leveldb/*/*.gcno leveldb/helpers/memenv/*.gcda leveldb/helpers/memenv/*.gcno
	-rm -f config.h
	-rm -rf test/__pycache__
	-rm -rf *.dSYM test/*.dSYM bench/*.dSYM qt/*.dSYM qt/test/*.dSYM

.rc.o:
	@test -f $(WINDRES)
	## FIXME: How to get the appropriate modulename_CPPFLAGS in here?
	$(AM_V_GEN) $(WINDRES) $(DEFS) $(DEFAULT_INCLUDES) $(INCLUDES) $(CPPFLAGS) -DWINDRES_PREPROC -i $< -o $@

check-symbols: $(bin_PROGRAMS)
	@echo "Running symbol and dynamic library checks..."
	$(AM_V_at) $(PYTHON) $(top_srcdir)/contrib/devtools/symbol-check.py $(bin_PROGRAMS)

check-security: $(bin_PROGRAMS)
if HARDEN
	@echo "Checking binary security..."
	$(AM_V_at) $(PYTHON) $(top_srcdir)/contrib/devtools/security-check.py $(bin_PROGRAMS)
endif


osx_debug: $(bin_PROGRAMS)
	for i in $(bin_PROGRAMS); do mkdir -p $$i.dSYM/Contents/Resources/DWARF && $(DSYMUTIL_FLAT) -o $$i.dSYM/Contents/Resources/DWARF/$$(basename $$i) $$i &> /dev/null ; done

include Makefile.crc32c.include
include Makefile.leveldb.include
include Makefile.test_util.include
include Makefile.test_fuzz.include

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

include Makefile.univalue.include
