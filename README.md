# Quickstart for OSX

Instructions for building and running the NSS test server and client with TLS 1.3; cribbed from: https://github.com/bifurcation/mint.

```
# Fetch and build NSS
export USE_64=1
export ENABLE_TLS_1_3=1
NSS_ROOT=`pwd`
hg clone https://hg.mozilla.org/projects/nss
hg clone https://hg.mozilla.org/projects/nspr

# In my environment: hostname == 'Lotus.local'
export HOST=Lotus
export DOMSUF=local

# Fetch and build NSS
cd nss
make nss_build_all

# Check $NSS_ROOT/dist/ for the PLATFORM
# Mine was: 
export PLATFORM=Darwin15.6.0_64_DBG.OBJ
export DYLD_LIBRARY_PATH=$NSS_ROOT/dist/$PLATFORM/lib
export LD_LIBRARY_PATH=dist/$PLATFORM/lib

# Run NSS tests (this creates data for the server to use)
cd tests/ssl_gtests
./ssl_gtests.sh
```

For simplicity, all the environment values as they appear on my platform:
```
export USE_64=1
export ENABLE_TLS_1_3=1
export NSS_ROOT=~/tls13
export PLATFORM=Darwin15.6.0_64_DBG.OBJ
export DYLD_LIBRARY_PATH=$NSS_ROOT/dist/$PLATFORM/lib
export LD_LIBRARY_PATH=dist/$PLATFORM/lib
export HOST=Lotus
export DOMSUF=local
```

### Run test server
Run from NSS_ROOT:
```
dist/$PLATFORM/bin/selfserv -d tests_results/security/$HOST.1/ssl_gtests/ -n rsa -p 4430
```

### Run test client
```
dist/$PLATFORM/bin/tstclnt -d tests_results/security/$HOST.1/ssl_gtests/ -V tls1.3:tls1.3 -h localhost -p 4430 -o
```

Issue a dummy HTTP and the server will echo it with a simple HTTP response:
```
GET /index.html HTTP/1.1


```
(Include two blank lines.)
