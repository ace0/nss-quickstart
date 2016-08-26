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

`ssl_gtests` creates a series of DB files in `$NSS_ROOT/tests_results/security/$HOST.1/ssl_gtests1` that we'll use for the test client and server.

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
export DBDIR=$NSS_ROOT/tests_results/security/$HOST.1/ssl_gtests

# Enables debug tracing
export TRACE=1
```

### Run test server
Run from NSS_ROOT:
```
dist/$PLATFORM/bin/selfserv -d $DBDIR -n rsa -p 4430
```

### Run test client
```
dist/$PLATFORM/bin/tstclnt -d $DBDIR -V tls1.3:tls1.3 -h localhost -p 4430 -o
```

Issue an HTTP request and the server will echo it with a simple HTTP response:
```
GET /index.html HTTP/1.1


```
(Include two blank lines.)


# Quickstart for Ubuntu

Instructions to build NSS with TLS 1.3 on Ubuntu.

## Prerequisites
There may be others, but at least:
```
sudo apt-get -y install zlib1g-dev
```
## Fetch and build NSS
```
export USE_64=1
export ENABLE_TLS_1_3=1
#NSS_ROOT=`pwd`
hg clone https://hg.mozilla.org/projects/nss
hg clone https://hg.mozilla.org/projects/nspr

# In my environment: hostname == 'Lotus.local'
#export HOST=Lotus
#export DOMSUF=local

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

`ssl_gtests` creates a series of DB files in `$NSS_ROOT/tests_results/security/$HOST.1/ssl_gtests1` that we'll use for the test client and server.

For simplicity, all the environment values as they appear on my platform:
```
#export USE_64=1
#export ENABLE_TLS_1_3=1
export NSS_ROOT=~/tls13
export PLATFORM=Darwin15.6.0_64_DBG.OBJ
export DYLD_LIBRARY_PATH=$NSS_ROOT/dist/$PLATFORM/lib
export LD_LIBRARY_PATH=dist/$PLATFORM/lib
export HOST=Lotus
export DOMSUF=local
export DBDIR=$NSS_ROOT/tests_results/security/$HOST.1/ssl_gtests

# Enables debug tracing
export TRACE=1
```

### Run test server
Run from NSS_ROOT:
```
dist/$PLATFORM/bin/selfserv -d $DBDIR -n rsa -p 4430
```

### Run test client
```
dist/$PLATFORM/bin/tstclnt -d $DBDIR -V tls1.3:tls1.3 -h localhost -p 4430 -o
```
