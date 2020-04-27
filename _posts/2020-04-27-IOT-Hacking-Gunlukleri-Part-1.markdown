---
layout: post
title:  "IOT Hacking Günlükleri - Part 1"
date:   2020-04-27
---
### <strong><center>IOT Hacking Günlükleri - Part 1</center>

IOT Hacking için belkide en önemli yapılarından biri olan Firmware hacking’i biraz yakından inceleyip kullanılan tool’lar hakkında bilgi vereceğim.

Firmware Hacking basit olarak 5 Adımda inceleyebiliriz.

1. Firmware’i elde etmek
2. Firmware’i inceleme
3. Filesystem’i çıkarma
4. Filesystem’i inceleme
5. Filesystem’i emule etme

Bu 5 adımın sonunda elde etmek istediğimiz ve bu süreçte arayacağımız şeylere örnek vermek gerekirse;

1. Koda Gömülü Parolalar
2. API Token’ları
3. EndPoint’ler
4. Zafiyetli servisler ( Hak Yükseltme vs..)
5. Backdoor’lar
6. Yapılandırma dosyaları
7. Cihaz Üzerinde Olabilecek tüm zafiyetler

Peki gelelim asıl soruya Firmware Nedir? Nasıl Hack Edilir?

<strong>Firmware:</strong> Donanımın kendisinden beklenen tüm fonksiyonları yerine getirmesi için kullanılan yazılımlara verilen addır.

Firmware’i elde etmek için genel olarak kullanılan 4 metottan bahsedebiliriz.

#### <strong>A. Üretici firmaların kendi sitelerinden</strong> 

En yaygın yöntemlerden biri diyebiliriz. Birçok üretici Firmware güncellemelerini kendi sistemleri üzerinden bu şekilde paylaşıyor. Özellikle encrypt olmayan bu tarz paylaşımlar sayesinde oldukça kolay şekilde cihaz üzerinde koşan yazılıma sahip olabiliyoruz.

Genellikle router üreticilerinin tercihi, internet sitelerinde ilgili güncellemeyi paylaşmak oluyor.

#### <strong>B. Arama motorlarını kullanarak</strong>

Burada örnek vermeye gerek görmüyorum. Bu tarz firmware’leri direkt olarak üretici sitesinden değil fakat bize firmware’i sunan başka bir endpoint’i aramak diyebiliriz.

#### <strong>C. Cihaz güncelleme alırken MiTM yaparak </strong>

Günümüzde oldukça yaygın bir yöntem örnek olarak akıllı TV’lerinizi internete bağladığınızda size yeni bir yazılım bulundu uyarısı verir. Basitçe çalışma prensibinden bahsedecek olursak uygulama çalışmaya başlayıp internete bağlandığında bir endpointe istek atarak güncel versiyon bilgisini alır daha sonra alınan bu versiyon bilgisi cihaz üzerindeki ile karşılaştırılır. Üst bir versiyon var ise cihaz o endpointe giderek kendine firmware’i indirir ve kurar. Modem arayüzüne girdiğinizde firmware güncelleme adımının otomatize edilmiş hali diyebiliriz.

Örnek olarak bir Akıllı TV düşünelim güncelleme sırasında harici bir sisteme istek atıyor ve oradan firmware’ini alıyor olsun.

Yapılabileceklerin başında cihaza proxy tanımlayıp giden istekler eğer okunabilirse (http/HTTPS) burp veya owasp zap ile gittiği endpointi alabilir o isteği yaparak kendi makinemize firmware’i indirebiliriz

Fakat bu her zaman o kadar basit işlemiyor çoğu IOT cihaz üzerinde proxy tanımlama imkanımız bulunmuyor, tanımlasak bile BURP veya Zap gibi proxy tooların yakalayamayacağı socket isteği ile haberleşebilir. Bu durumda mitm yapmak daha mantıklı görünüyor. Çoğumuzun bildiği standart bir MITM dışında aşağıda size SSL üzerinden MITM nasıl gerçekleştirilir onu gösteriyor olacağım. Bu sayede ilgili cihaz/uygulama SSL bağlantı açmak istediğinde paketlerimiz yarıda kalmayacak.

Bunu diğer yazımızda anlatmıştık.

Bu sayede cihazın iletişimi kesilmiyor giden tüm istekleri görebiliyoruz. Kritik olabilecek bir nokta ise SSL pinning olan bir uygulama için bunu yaptığımızı varsayalım, firmware’i elde ettiniz giden paketleri incelemek istiyorsunuz. Elinizde firmware olduğu için bir sertifika ve private key bulunuyor. Siz yukarıdaki adımlarda belirtilen key üretme ve sertifika üretme adımı yerine bu sertifika ve keyi kullanarak SSL pinning’i de atlatmış oluyorsunuz ve giden paketleri rahatlıkla okuyabilirsiniz.

#### <strong>D. Direk olarak cihaz üzerinden</strong>

Bu aslında sonraki yazılarımda değineceğim, Hardware hacking olarak geçen ve cihaz donanımında bulunan, bırakılmış veya unutulmuş iletişim pinleri sayesinde veya cihaz hafızasından firmware’i elde etmeyi hedefleyen adım. Detaylarını daha sonraki yazılarımda uzun uzun anlatıyor olacağım.

Firmware’i elde ettik peki ya şimdi ?

