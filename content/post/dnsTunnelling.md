---
title: "DNS TÜNELLEME"

tags: [ "dnstünelleme", "iodine", "dnstunnelling"]
date: "2017-08-03"
---


### Iodine Aracı Kullanarak Dns Tünelleme

Iodine aracı kullanarak dns tünelleme nasıl yapılır inceleyelim.
Bize kontrolümüzde olan bir sunucu lazım bunun için ben digitalocean da oluşturduğum sunucuyu kullanacağım. Ve bu sunucuya bağlı bir domain adına ihtiyacımız var. Domain adını ücretsiz olarak edinebileceğiniz bir çok seçenek mevcut. Alan adını alırken dns ayarlarında "use your own DNS" sekmesinden nameserver kısmına digitalocean nameserver adlarını yazıyoruz ve devam ettikten sonra onaylıyoruz.</p>

<img src="https://zehrabetulboynuegri.github.io/blog/img/domain.png" >


Digitalocean hesabımızda networking sekmesinden domains sekmesine tıklıyoruz. Oraya aldığımız domain adını ekliyoruz.

<img src="https://zehrabetulboynuegri.github.io/blog/img/adddomain.png" >

Sonrasında hostname verip sunucu ip sini giriyoruz.


<img src="https://zehrabetulboynuegri.github.io/blog/img/dns.png" >


Gerekli olan sunucu dns ayarlaması yapıldıktan sonra aldığımız test.dnstunnelling.cf adresine ping atıp bakabiliriz.


<img src="https://zehrabetulboynuegri.github.io/blog/img/7.png" >


Gelelim dns tünellemede bize eşlik edecek olan iodine aracına. Iodine aracını hem server tarafına hem de client tarafına kuruyoruz. Benim her ikisi de ubuntu işletim sistemi olduğu için direkt aşağıdaki komut ile iki tarafın da kurulumunu gerçekleştirdim.


```
sudo apt-get install iodine
```

Şimdi ***server*** tarafında aşağıdaki komutu yazalım.


```
iodined -f -P password 10.0.0.1 test.dnstunnelling.cf
```

Burada yazan <code>password</code> oluşturulan bağlantıya atanacak olan parola. Client tarafında bağlanılacağı zaman bu parola kullanılır.
10.0.0.1 yerine ise sunucunun dns0 bacağına atamak istediğiniz ip verilir.
test.dnstunnelling.cf yerinede sunucuya eklenen domain adı gelir.


![](https://zehrabetulboynuegri.github.io/blog/img/1.png) 


***Client*** tarafında ise 


```
iodine -fP password 37.139.26.240 test.dnstunnelling.cf
```

komutu çalıştırılır. (37.139.26.240-->server ip)


![](https://zehrabetulboynuegri.github.io/blog/img/2.png) 
Dns tünel bağlantısı tamamlandı. İnternete bağlı olduğunuz ağ arayüzünü wireshark ile dinlemeye aldığınızda dns protokollü paketleri görebirsiniz.


![](https://zehrabetulboynuegri.github.io/blog/img/3.png) 

Aynı zamanda iki tarafta da ping atıp, dns0 arayüzünü dinleyelim.
![](https://zehrabetulboynuegri.github.io/blog//img/9.png) 



 ![](https://zehrabetulboynuegri.github.io/blog/img/8.png) 
 
 











