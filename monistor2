arch=$(uname -a)

physical_cpus=$(lscpu -p | egrep -v '^#' | wc -l)
logical_cpus=$(lscpu -p | egrep -v '^#' | sort -u -t, -k 2,4 | wc -l)

total_mem=$(free -m | awk '{print $2}' | sed -n '2p')
used_mem=$(free -m | awk '{print $3}' | sed -n '2p')
available_mem=$(($used_mem*100/$total_mem))

total_disk=$(df -Bm | grep '^/dev' | awk '{s += $2} END { printf "%.0f", s }')
used_disk=$(df -Bm | grep '^/dev' | awk '{s += $3} END { printf "%.0f", s }')
available_disk=$(($used_disk*100/$total_disk))

cpu_load=$(top -bn1 | grep "Cpu(s)" | sed "s/.*, *\([0-9.]*\)%* id.*/\1/" | awk '{print 100 - $1"%"}')

last_boot=$(who -b | awk '{print $3" "$4}')

lvm_use=$(if [ $(lsblk | grep "lvm" | wc -l) -eq 0 ]; then echo no; else echo yes; fi)

active_connections=$(ss -s | grep '^TCP' | grep 'estab' | awk '{print $4}' | sed 's/.$//')

user_log=$(who | wc -l)

ip=$(hostname -I)
mac=$(ip a | grep "link/ether" | awk '{print $2}')

sudo_num=$(journalctl _COMM=sudo | grep COMMAND | wc -l)

echo "	#Architecture: $arch"
echo "	#CPU physical: $physical_cpus"
echo "	#vCPU: $logical_cpus"
echo "	#Memory Usage: $used_mem/${total_mem}MB ($available_mem%)"
echo "	#Disk Usage: $used_disk/${total_disk}MB ($available_disk%)"
echo "	#CPU load: $cpu_load"
echo "	#Last boot: $last_boot"
echo "	#LVM use: $lvm_use"
echo "	#Connexions TCP: $active_connections ESTABLISHED"
echo "	#User log: $user_log"
echo "	#Network: IP $ip ($mac)"
echo "	#Sudo : $sudo_num cmd"
