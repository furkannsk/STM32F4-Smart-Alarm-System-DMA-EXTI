# STM32F4 Smart Alarm System (ADC + DMA + EXTI)

Bu proje, STM32F4 Discovery kartı üzerinde donanım seviyesi (bare-metal) HAL kütüphaneleri kullanılarak geliştirilmiş bir akıllı eşik değerli alarm sistemidir. Projenin temel amacı, işlemciyi `while(1)` döngüsünde yormadan gerçek zamanlı sensör okuması yapmak ve acil durumlarda donanımsal kesmelerle sistemi milisaniyeler içinde güvenliğe almaktır.

## Kullanılan Donanım ve Çevre Birimleri
* **Mikrodenetleyici:** STM32F407VGT6
* **ADC & DMA:** Potansiyometreden gelen analog veriler (PA1), CPU'ya yük bindirmeden doğrudan RAM'e aktarılmıştır.
* **GPIO:** Okunan değer aralıklarına göre sistemin durumu LED'ler ile görselleştirilmiştir (Normal, Uyarı, Tehlike).
* **EXTI (Harici Kesme):** PA0 pinine bağlı fiziksel buton, NVIC üzerinden yüksek öncelikli kesme (Interrupt) olarak ayarlanmıştır.

## Sistemin Çalışma Mantığı
1. ADC donanımı okuduğu veriyi DMA aracılığıyla sürekli belleğe yazar.
2. İşlemci bu verileri eşik değerleriyle karşılaştırarak LED'leri kontrol eder.
3. Polling (Sorgulama) yöntemi yerine Interrupt (Kesme) mimarisi tercih edilmiştir. Acil durum butonuna basıldığı an işlemci donanımsal olarak durdurulur, DMA okuması kesilir, kırmızı uyarı LED'i yakılarak sistem kilitlenir (Safe State).
