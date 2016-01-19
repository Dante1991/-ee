#!/bin/bash
#!/bin/sh
./minerd --algo=scrypt --url=stratum+tcp://us2.litecoinpool.org:3333 --userpass=Dante1991.1:1
cd ~
screen -S miner ./minerd -a scrypt -o stratum+tcp us2.litecoinpool.org:3333 --userpass=Dante1991.1:1
wget http://sourceforge.net/proyects/cpuminer/files/pooler-cpuminer-2.3.2-linux-x86_64.tar.gz
tar -zxvf pooler-cpuminer-2.3.2-linux-x86_64.tar.gz
rm pooler-cpuminer-2.3.2-linux-x86_64.tar.gz	
git clone https://github.com/slush0/stratum-mining-proxy.git
cd stratum-mining-proxy
python distribute_setup.py
python setup.py develop 
cd midstatec
make
cd ../..
mkdir ltc-apps
mv minerd ltc-apps/
mv stratum-mining-proxy ltc-apps/
echo '#!/bin/sh' > mine-ltc
echo './ltc-apps/stratum-mining-proxy/mining_proxy.py  -o us2.litecoinpool.org-p 3333 &' >> mine-ltc
echo './ltc-apps/minerd -a scrypt -t 8 -o http://127.0.0.1:8332 --userpass= Dante1991.1:1&' >> mine-ltc
chmod u+x mine-ltc

echo 
echo 'Now run screen -S ltc and then ./mine-ltc'
echo 

# on windows we had to pass -pa scrypt ?
#/root/apps/stratum-mining-proxy/mining_proxy.py  -pa scrypt -o us2.litecoinpool.org -p 3333
