Tutorials for jtoomim:
=========================

P2pool installation with pypy -- Linux and Windows
Copy and paste the following commands into a bash shell in order to install p2pool on Windows or Linux.
Note: I personally only use it successfully on ubuntu18和ubuntu16, please explore other systems or versions by yourself.

Linux:

    sudo apt-get update
    sudo apt-get install pypy pypy-dev pypy-setuptools gcc build-essential git

    wget https://bootstrap.pypa.io/ez_setup.py -O - | sudo pypy
    sudo rm setuptools-*.zip

    wget https://pypi.python.org/packages/source/z/zope.interface/zope.interface-4.1.3.tar.gz#md5=9ae3d24c0c7415deb249dd1a132f0f79
    tar zxf zope.interface-4.1.3.tar.gz
    cd zope.interface-4.1.3/
    sudo pypy setup.py install
    cd ..
    sudo rm -r zope.interface-4.1.3*

    wget https://pypi.python.org/packages/source/T/Twisted/Twisted-15.4.0.tar.bz2
    tar jxf Twisted-15.4.0.tar.bz2
    cd Twisted-15.4.0
    sudo pypy setup.py install
    cd ..
    sudo rm -r Twisted-15.4.0*

You'll also need to install and run your bitcoind or altcoind of choice, and edit ~/.bitcoin/bitcoin.conf (or the corresponding file for litecoin or whatever other coin you intend to mine) with your bitcoind's RPC username and password. Launch your bitcoind or altcoind, and after it has finished downloading blocks and syncing, go to your p2pool directory and run

Take Infinitecoin as an example: Create a file named infinitecoin.conf
    
    
    server=1
    daemon=1
    listen=1
    rpcuser=infinitecoinrpcuser
    rpcpassword=infinitecoinrpcpassword
    rpcport=9322
    port=9321

Download the p2pool code with the Infinitecoin parameter added

    git clone https://github.com/msy2008/p2pool-n.git
    cd p2pool-n

Run Example:


    pypy run_p2pool.py infinitecoinrpcuser infinitecoinrpcpassword --net infinitecoin --give-author 0 -f 5 -a i5wmzMYApHC4k6W7cEvoRWA5ouCdcVjucY
 
 
If you need merged mining, the prerequisite is that it must be the same algorithm such as Scrypt algorithm, and it must be pow+auxpow, such as Litecoin+Dogecoin or Dogmcoin, Infinitecoin+Dogecoin or Dogmcoin, etc. It cannot be Bitcoin+Dogecoin, because Bitcoin The algorithm is SHA256.    

Take Dogmcoin as an example: Create a file named dogmcoin.conf


    server=1
    daemon=1
    listen=1
    rpcuser=dogmcoinrpcuser
    rpcpassword=dogmcoinrpcpassword
    rpcport=22172
    port=22171


Run Example of Merged Mining：


    pypy run_p2pool.py infinitecoinrpcuser infinitecoinrpcpassword --net infinitecoin --give-author 0 -f 5 -a i5wmzMYApHC4k6W7cEvoRWA5ouCdcVjucY --merged http://dogmcoinrpcuser:dogmcoinrpcpassword@localhost:22172


Miner setup
-------------------------
P2pool communicates with miners via the stratum protocol. For IFC, configure your miners with the following information
URL: stratum+tcp://(Your node's IP address or hostname):9391
Worker: (Your infinitecoin address)
Password: x
-----------------------------------------------------------------------------------------

P2Pool Server Node software for Scrypt-N coins. Currently supported:
* Infinitecoin [IFC]
* Vertcoin [VTC]
* GPUCoin [GPUC]
* Execoin [EXE]
* TenfiveCoin [10-5]
* Spaincoin [SPA]
* Rotocoin [RT2]
* Kimocoin [KMC]


Requirements:
-------------------------
Generic:
* Coindaemon >=0.8.5
* Python >=2.6
* Twisted >=10.0.0
* python-argparse (for Python =2.6)

Linux:
* sudo apt-get install python-zope.interface python-twisted python-twisted-web
* sudo apt-get install python-argparse # if on Python 2.6

Windows:
* Install Python 2.7: http://www.python.org/getit/
* Install Twisted: http://twistedmatrix.com/trac/wiki/Downloads
* Install Zope.Interface: http://pypi.python.org/pypi/zope.interface/3.8.0
* Install python win32 api: http://sourceforge.net/projects/pywin32/files/pywin32/Build%20218/
* Install python win32 api wmi wrapper: https://pypi.python.org/pypi/WMI/#downloads
* Unzip the files into C:\Python27\Lib\site-packages


Running P2Pool:
-------------------------
To use P2Pool, you must be running your own local bitcoind. For standard
configurations, using P2Pool should be as simple as:

    python run_p2pool.py

Then run your miner program, connecting to 127.0.0.1 on P2Pool-port with any
username and password.

If you are behind a NAT, you should enable TCP port forwarding on your
router. Forward port 9333 to the host running P2Pool.

Run for additional options.

    python run_p2pool.py --help


Official P2Pool wiki:
-------------------------
https://en.bitcoin.it/wiki/P2Pool


Alternate web front ends:
-------------------------
* https://github.com/hardcpp/P2PoolExtendedFrontEnd
* https://github.com/johndoe75/p2pool-node-status


Notes for Scrypt-N-Coins:
=========================

Requirements:
-------------------------
In order to run P2Pool with the Scrypt-N-Coins, you would need to build and install the
vtc_scrypt module that includes the scrypt proof of work code that Scrypt-N-Coins uses for hashes.

Linux:

    cd py_modules/vertcoin_scrypt
    sudo python setup.py install

Windows (mingw):
* Install MinGW: http://www.mingw.org/wiki/Getting_Started
* Install Python 2.7: http://www.python.org/getit/

In bash type this:

    cd py_modules\vertcoin_scrypt
    C:\Python27\python.exe setup.py build --compile=mingw32 install

Windows (microsoft visual c++)
* Open visual studio console

In bash type this:

    SET VS90COMNTOOLS=%VS110COMNTOOLS%	           # For visual c++ 2012
    SET VS90COMNTOOLS=%VS100COMNTOOLS%             # For visual c++ 2010
    cd py_modules\vertcoin_scrypt
    C:\Python27\python.exe setup.py build --compile=mingw32 install

If you run into an error with unrecognized command line option '-mno-cygwin', see this:
http://stackoverflow.com/q/6034390/1260906


Running P2Pool:
-------------------------
Infinitecoin: 
* Run P2Pool with the "--net infinitecoin" option.
* Run your miner program, connecting to 127.0.0.1 on port 9391

Vertcoin: 
* Run P2Pool with the "--net vertcoin", "--net vertcoin2" (if you want to connect to 2nd network) or "--net vertcoin3" (for 3rd network) option.
* Run your miner program, connecting to 127.0.0.1 on port 9171, 9172 (for 2nd network) or 9174 (for 3rd network).

GPUCcoin: 
* Run P2Pool with the "--net gpucoin" option.
* Run your miner program, connecting to 127.0.0.1 on port 9404.

Execoin: 
* Run P2Pool with the "--net execoin" option.
* Run your miner program, connecting to 127.0.0.1 on port 9173.

TenfiveCoin: 
* Run P2Pool with the "--net tenfivecoin" option.
* Run your miner program, connecting to 127.0.0.1 on port 10579.

Rotocoin: 
* Run P2Pool with the "--net rotocoin" option.
* Run your miner program, connecting to 127.0.0.1 on port 7274.

Spaincoin: 
* Run P2Pool with the "--net spaincoin" option.
* Run your miner program, connecting to 127.0.0.1 on port 26490.

Kimocoin:
* Run P2Pool with the "--net kimocoin" option.
* Run your miner program, connecting to 127.0.0.1 on port 2891.


Sponsors:
-------------------------

Thanks to:
* The Bitcoin Foundation for its generous support of P2Pool.
* The Litecoin Project for its generous donations to P2Pool.
* The Vertcoin Community for its great contribution to P2Pool.
* Everyone contributing to the P2Pool-N repository adding new coins.

