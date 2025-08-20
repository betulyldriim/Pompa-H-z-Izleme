Bu depo, endüstriyel otomasyon alanında bir uygulama olan, bir pompanın veya döner ekipmanın hızını gerçek zamanlı olarak izlemek ve kontrol etmek için tasarlanmış bir PLC (Programlanabilir Mantık Denetleyici) ve HMI (İnsan-Makine Arayüzü) projesini içermektedir. Proje, özellikle Siemens TIA Portal V16 yazılımı kullanılarak SIMATIC S7-1200 PLC ve TP700 Comfort HMI paneli için geliştirilmiştir.

1. Proje Amacı ve Kullanılan Teknolojiler
Projenin temel amacı, bir yakınlık sensöründen alınan darbe sinyalleri ile bir makinenin hızını doğru bir şekilde ölçmektir. Bu hız verileri, farklı birimlere (RPS, RPM, RPH) dönüştürülür ve operatörün sistemi kolayca yönetebileceği bir HMI ekranında görselleştirilir.

PLC: Siemens SIMATIC S7-1200 CPU 1214C DC/DC/DC

HMI: Siemens SIMATIC HMI KTP700 Comfort

Sensör: Hız ölçümü için kullanılan bir endüktif yakınlık sensörü.

Yazılım: Siemens TIA Portal V16 (veya üstü)

2. PLC Programlama Mantığı (Main [OB1])
Ladder Logic (LAD) programı, projenin beyin fonksiyonlarını üstlenir ve aşağıdaki temel ağlardan oluşur:

Network 1: Hız Darbelerinin Sayılması ve Zamanlama

Bir CTU (Up Counter) bloğu, "Speed_Detected" etiketinden gelen her darbe sinyalini sayar. Sayıcı, artan değerini "Actual_Counting" etiketine kaydeder.

Bir TON (On-Delay Timer) bloğu, sürekli ("AlwaysTRUE") aktif bir bit ile her 180 milisaniyede bir (PT=T#180ms) bir "bellek 4" bitini tetikler. Bu zamanlayıcı, hız hesaplamalarının belirli aralıklarla güncellenmesi için bir zaman referansı sağlar.

Network 2: Hız Birimlerinin Hesaplanması

Bir MOVE bloğu, sayıcıdan gelen "Actual_Counting" değerini doğrudan "RPS" (Revolutions Per Second - Saniyedeki Devir) etiketine taşır. Bu, sayılan darbe sayısının doğrudan saniyedeki devir olarak kabul edildiğini gösterir.

İki adet MUL (Multiply) bloğu, farklı hız birimlerini hesaplar:

Birinci çarpma bloğu, "RPS" değerini 60 ile çarparak "RPM" (Revolutions Per Minute - Dakikadaki Devir) değerini elde eder.

İkinci çarpma bloğu, "RPM" değerini tekrar 60 ile çarparak "RPH" (Revolutions Per Hour - Saatteki Devir) değerini elde eder.

Network 3: Başlatma/Durdurma Kontrolü

Bu ağ, pompanın Start ve Stop butonları ile kontrol edilmesini sağlayan klasik bir mandallama (latch) devresini içerir.

"Start" butonu (%M0.0) basıldığında, "Pump" çıkışı (%Q0.1) aktif olur.

"Pump" çıkışının kendi kontağı, buton bırakıldıktan sonra devrenin enerjili kalmasını sağlar.

"Stop" butonu (%M0.1) basıldığında, normalde kapalı kontağı açılarak devreyi keser ve pompayı durdurur.

3. HMI Ekran Tasarımı ve İşlevselliği
HMI paneli, operatörün projeyle etkileşimini kolaylaştıran bir arayüz sunar.

Görselleştirme: Ekranın üst kısmında projenin adını belirten başlıklar ("Speed Measurement using Proximity Sensor", "PUMP SPEED MONITORING AND CONTROL") yer alır. Alt kısımda ise bir motor ve pompa sembolü bulunur.

Veri Alanları: Üç farklı hız birimi için sayısal görüntüleme alanları mevcuttur. Bu alanlar, PLC'de hesaplanan "RPS", "RPM" ve "RPH" etiketlerine bağlanmıştır.

Kontrol Butonları: Ekranın sol tarafında, pompa kontrolü için Start ve Stop butonları bulunur. Bu butonlar, PLC programındaki %M0.0 ve %M0.1 bellek bitlerini kontrol eder.
