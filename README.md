# network

#/bin/bash
echo "###input ip you want change to ### "
ipfront=172.17.130.
read ip1
ip="${ipfront}${ip1}"
echo "###change ip as====" $ip "###"
eth=`find /etc/sysconfig/network-scripts/ -name "ifcfg-e*" |sort |head -1 |awk -F/ '{print $5}'`
ethfile="/etc/sysconfig/network-scripts/$eth"
echo "${ethfile}"
sed -i 's/BOOTPROTO="dhcp"/BOOTPROTO="static"/g' "${ethfile}"
sed -i 's/^ONBOOT="no"/ONBOOT="yes"/g' "${ethfile}"
#testfile="/etc/sysconfig/network-scripts/test"
echo "IPADDR=${ip}" >>${ethfile}
echo 'PREFIX=24' >>${ethfile}
echo 'NETMASK=255.255.255.0' >>${ethfile}
echo 'NETWORK=172.17.130.0' >>${ethfile}
echo 'GATEWAY=172.168.130.254' >>${ethfile}
echo 'DNS1=8.8.8.8' >>${ethfile}
echo 'PEERDNS="yes"' >>${ethfile}
service network restart
