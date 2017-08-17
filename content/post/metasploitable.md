---
title: "METASPLOITABLE"
date: 2017-08-17T17:08:37+03:00
draft: true
---

### Metasploitable ile Penetrasyon Test Pratiği

Metasploitable penetrasyon testlerinde ve güvenlik araçları üzerinde pratik yapmaya yarayan sanalda çalışan makinedir.Metasploitable sanal makinesini [buradan indirebilirsiniz.](https://www.vulnhub.com/entry/metasploitable-2,29/) 

Sanal makinelerin network ayarını ayarlayalım. Sanal kali üzerinden ilerleneceğinden kali tarafında internet bağlantısı olması için, Machine > settings > network > adapter1 sekmesinden bridge moduna alıyoruz. 

![](https://zehrabetulboynuegri.github.io/blog/img/kaliadpt1.png) 

Metasploitable sanalını görebilmesi için ise adapter2 yi de NAT moduna alıyoruz.

 ![](https://zehrabetulboynuegri.github.io/blog/img/kaliadpt2.png) 
 
Eğer name listesi boş ise file > preferences > network  sekmesinden ayarlar yapılandırabilir.

Metasploitable tarafında ise aynı şekilde adapter1 sekmesine geliyoruz ve NAT moduna alıyoruz.

![](https://zehrabetulboynuegri.github.io/blog/img/msfnetwork.png) 

Metasploitable tarafında <code>ifconfig</code> komutu ile ip adresini kontrol edelim.

![](https://zehrabetulboynuegri.github.io/blog/img/ifconfig.png) 


Kali tarafında internet bağlantısı yoksa <code>``dhclient <interface>``</code> (dhclient eth0) komutu ile yapılandırabilirsiniz.
 
Network ayarlarını tamamladığımıza göre hedef ip adres aralığını nmap ile taramaya başlayalım.

```bash
root@kali:~# nmap -sS 10.0.2.0/24

Starting Nmap 7.40 ( https://nmap.org ) at 2017-08-17 10:17 EDT
Nmap scan report for 10.0.2.3
Host is up (0.00078s latency).
All 1000 scanned ports on 10.0.2.3 are filtered
MAC Address: 08:00:27:E0:1A:90 (Oracle VirtualBox virtual NIC)

Nmap scan report for 10.0.2.4
Host is up (0.00081s latency).
Not shown: 977 closed ports
PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
23/tcp   open  telnet
25/tcp   open  smtp
53/tcp   open  domain
80/tcp   open  http
111/tcp  open  rpcbind
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
512/tcp  open  exec
513/tcp  open  login
514/tcp  open  shell
1099/tcp open  rmiregistry
1524/tcp open  ingreslock
2049/tcp open  nfs
2121/tcp open  ccproxy-ftp
3306/tcp open  mysql
5432/tcp open  postgresql
5900/tcp open  vnc
6000/tcp open  X11
6667/tcp open  irc
8009/tcp open  ajp13
8180/tcp open  unknown
MAC Address: 08:00:27:7E:10:2E (Oracle VirtualBox virtual NIC)

Nmap scan report for 10.0.2.5
Host is up (0.0000070s latency).
All 1000 scanned ports on 10.0.2.5 are closed

Nmap done: 256 IP addresses (3 hosts up) scanned in 32.61 seconds
```
Metasploitable için atanan 10.0.2.4 ip adresine ait portlar ve açık servisleri görmekteyiz. İncelen bazı portlar doğrultusunda tespit edilen güvenlik açıklarına bakalım.

#### **Güvenlik Açıkları**

* ***Uzaktan Erişim Zafiyeti*** 
  
  Tarama sonucu açık olduğunu bulduğumuz 512, 513, 514 numaralı portlar uzaktan erişimi sağlayan portlardır. Bu portta çalışan servisler kimlik doğrulaması olmaksızın erişim sağlanacak şekilde konfigüre edildiği için <code>``rlogin <target>``</code> komutu ile metasploitable sistemine erişebiliriz.
  
  ![](https://zehrabetulboynuegri.github.io/blog/img/rlogin1.jpg) 
  
  Gördüğünüz gibi şifre istiyor.  Kali Linux'da ssh-client kurulu değil. Rsh-client, kullanıcının uzaktaki makineye bağlanma iznini vererek, oturum açabilmesine yarayan bir programdır. 
  
  > <code>apt-get install rsh-client</code> 
  
  ![](https://zehrabetulboynuegri.github.io/blog/img/rsh-client.png) 
  
  Rsh-client kurulduktan sonra tekrar deneyelim.
  
  ![](https://zehrabetulboynuegri.github.io/blog/img/rlogin2.png) 
  
  Gördüğünüz üzere metasploitable makinesine root  yetkisiyle eriştik.
  
  
  

