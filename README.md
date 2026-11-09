# OSTA — Otonom Sebze Toplama Aracı

<p align="center"><img src="gorseller/arac/osta_genel.jpg" width="760" alt="OSTA kara aracı ve robot kollar"></p>

## Proje Hakkında

OSTA, sera ortamında hasat ve budama işlemlerini otonom olarak gerçekleştirmek amacıyla geliştirilen bir tarım robotudur. Sistem; sera koridorlarında ilerleyen bir kara aracı, araç üzerine yerleştirilmiş dört adet 6 eksenli robot kol ve ürünleri tespit eden yapay zekâ tabanlı bir görüntü işleme altyapısından oluşmaktadır.

Proje 2024–2026 yılları arasında Byte Spark Robotik bünyesinde geliştirilmiştir. Kara aracı, robot kollar, kontrol kartları, gömülü yazılımlar, görüntü işleme algoritmaları ve kullanıcı arayüzleri proje kapsamında tasarlanmış ve üretilmiştir.

Bu repo projenin tanıtımı amacıyla hazırlanmıştır; proje kodları paylaşılmamaktadır.

| | |
|---|---|
| **Kara aracı** | 4 tekerlekli, alüminyum şasili, bağımsız süspansiyonlu otonom platform |
| **Robot kollar** | 4 adet × 6 eksen (toplam 24 eklem), planet dişli redüktörlü |
| **Uç efektör** | Ürünün sapını tutup keserek hem hasat hem budama yapabilen mekanizma |
| **Algılama** | 2 adet stereo kamera, LiDAR |
| **Görüntü işleme** | Olgun/ham ürün ayrımı, sap kesim noktası tespiti, stereo derinlik |
| **Navigasyon** | LiDAR ile haritalama, konum belirleme ve otonom yol planlama |
| **Arayüzler** | Masaüstü yer istasyonu, mobil izleme uygulaması, mobil etiketleme uygulaması |
| **Geliştirme süresi** | 2024 – 2026 |

## Sistem Mimarisi

<p align="center"><img src="gorseller/diyagram/sistem_mimarisi.svg" width="900" alt="Sistem mimarisi"></p>

Kara aracı üzerindeki görev bilgisayarı, kameralardan ve LiDAR'dan gelen verileri işleyerek hedef ürünleri belirlemekte, aracın navigasyonunu ve robot kolların hareketlerini planlamaktadır. Robot kollara gönderilen komutlar haberleşme kartları üzerinden her kolun kendi kontrol kartına iletilmektedir. Operatör, sistemi masaüstü yer istasyonu ve mobil uygulama üzerinden izleyip yönetebilmektedir.

### Hasat görev akışı

<p align="center"><img src="gorseller/diyagram/gorev_akisi.svg" width="900" alt="Hasat görev akışı"></p>

Kara aracı sera sırası boyunca ilerleyip hasat noktasına ulaştığında stereo kameralardan görüntü alınmakta ve olgun ürünler ile sap kesim noktaları tespit edilmektedir. Her hedef için kesim noktasının 3B konumu hesaplanmakta, robot kol için engellerden kaçınan bir yol planlanmakta ve bu yoldan eklem yörüngeleri oluşturulmaktadır. Kol sapı tutup keserek ürünü almakta, hedefler tamamlandığında araç bir sonraki konuma ilerlemektedir.

## Kara Aracı

<p align="center">
  <img src="gorseller/arac/kara_araci_cad.png" width="49%" alt="Kara aracı CAD">
  <img src="gorseller/arac/osta_yan.jpg" width="27%" alt="Kara aracı">
</p>

Kara aracı, alüminyum sigma profil şasi üzerine kurulmuş dört tekerlekli bir platformdur. Şasinin üst kısmına robot kolları taşıyan kolonlar yerleştirilmiş, tekerlek süspansiyonları artan yüke göre güçlendirilmiştir. Motor sürücüler, görev bilgisayarı ve kol sürücüleri için araç üzerinde erişilebilir montaj bölmeleri oluşturulmuştur.

Araç, LiDAR ile sera ortamının haritasını çıkarmakta (SLAM), harita üzerinde kendi konumunu belirlemekte ve sera koridorları arasında otonom olarak ilerlemektedir. Tekerlek motorları PID kontrollü olarak sürülmekte, odometri enkoderlerle hesaplanmaktadır.

<p align="center">
  <img src="gorseller/navigasyon/haritalama_testi.jpg" width="22%" alt="Haritalama testi">
  <img src="gorseller/navigasyon/slam_haritasi.png" width="24%" alt="SLAM haritası">
  <img src="gorseller/navigasyon/yol_planlama_sim.png" width="38%" alt="Yol planlama simülasyonu">
</p>
<p align="center"><i>Haritalama testi, LiDAR ile çıkarılan ortam haritası ve sera düzeninde yol planlama simülasyonu</i></p>

## Robot Kollar

<p align="center">
  <img src="gorseller/robot_kol/robot_kol_cad.png" width="32%" alt="Robot kol CAD">
  <img src="gorseller/robot_kol/robot_kollar_1.jpg" width="25%" alt="Robot kollar">
  <img src="gorseller/robot_kol/robot_kollar_2.jpg" width="25%" alt="Robot kollar">
</p>

Robot kollar 6 serbestlik derecesine sahiptir ve 3B yazıcı ile üretilmeye uygun olarak tasarlanmıştır. Eklemlerde proje kapsamında tasarlanan planet dişli redüktörler kullanılmaktadır. Motorların sıcaklığı sürekli izlenmekte, kritik değerlere yaklaşıldığında motor akımı sınırlanarak sistem güvenli moda alınmaktadır.

<p align="center">
  <img src="gorseller/robot_kol/planet_reduktor.png" width="30%" alt="Planet redüktör">
  <img src="gorseller/robot_kol/eklem_reduktor.jpg" width="30%" alt="Eklem redüktörü">
  <img src="gorseller/robot_kol/uc_efektor.jpg" width="25%" alt="Uç efektör">
</p>

<p align="center">
  <img src="gorseller/robot_kol/uc_efektor_cad_1.png" width="30%" alt="Uç efektör CAD">
  <img src="gorseller/robot_kol/uc_efektor_cad_2.png" width="30%" alt="Uç efektör CAD">
  <img src="gorseller/robot_kol/uc_efektor_tahrik.jpg" width="22%" alt="Uç efektör tahrik mekanizması">
</p>
<p align="center"><i>Uç efektör tasarımı ve tahrik dişli mekanizması</i></p>

Uç efektör belirli bir ürüne özel olarak değil, farklı sebzelerde ortak kullanılabilecek şekilde tasarlanmıştır. Mekanizma ürünün kendisini değil, ürünü bitkiye bağlayan sapı tutmakta ve kavrama sırasında sapı keserek ürünü almaktadır. Bu sayede aynı uç efektör ile hem hasat hem de budama yapılabilmektedir.

Robot kolların hareketleri, ortamdaki engeller dikkate alınarak planlanmaktadır. Planlanan yol, her eklem için yumuşak hızlanma ve yavaşlama içeren yörüngelere dönüştürülmekte; eklem motorları enkoder geri beslemeli PID kontrolüyle bu yörüngeleri takip etmektedir.

<p align="center">
  <img src="gorseller/robot_kol/kinematik_sema.png" width="16%" alt="Kinematik şema">
  <img src="gorseller/robot_kol/yorunge_hiz_profili.png" width="30%" alt="Yörünge hız profili">
</p>
<p align="center"><img src="gorseller/robot_kol/motor_kontrol_dongusu.png" width="70%" alt="Motor kontrol döngüsü"></p>
<p align="center"><i>Robot kolun kinematik yapısı, eklem yörüngesi hız profili ve motor kontrol döngüsü</i></p>

<p align="center">
  <img src="gorseller/navigasyon/kol_hareket_planlama.png" width="45%" alt="Kol hareket planlama">
  <img src="gorseller/navigasyon/kol_hareket_planlama_2.png" width="45%" alt="Kol hareket planlama">
</p>

## Elektronik

<p align="center">
  <img src="gorseller/elektronik/robot_kol_kontrol_karti.jpg" width="38%" alt="Robot kol kontrol kartı">
  <img src="gorseller/elektronik/robot_kol_kontrol_karti_pcb.png" width="36%" alt="Robot kol kontrol kartı PCB">
</p>
<p align="center"><i>Robot kol kontrol kartı</i></p>

Sistemin elektronik kartları proje kapsamında tasarlanmış ve üretilmiştir:

- **Robot kol kontrol kartı:** Her kolun eklem motorlarını sürmekte, enkoder ve sıcaklık verilerini toplamaktadır.
- **Enkoder taşıyıcı kartı:** Her eklemde motorun arkasına yerleştirilen, manyetik enkoderi yerinde okuyan küçük bir karttır. Mutlak eklem açısını hesaplayıp gürültüye dayanıklı bir hat üzerinden kontrol kartına iletmektedir.
- **Haberleşme kartları:** Görev bilgisayarından gelen komutları ilgili robot kola dağıtmaktadır.
- **Kara aracı motor sürücü ünitesi:** Tekerlek motorlarını akım, gerilim ve sıcaklık izlemeli olarak süren, aktif soğutmalı sürücü bloğudur.
- **Görev bilgisayarı muhafazası:** Robot kolların görev bilgisayarı ve bağlantıları için tasarlanıp üretilen koruyucu kutudur.

<p align="center">
  <img src="gorseller/elektronik/enkoder_karti_pcb.png" width="26%" alt="Enkoder taşıyıcı kartı PCB">
  <img src="gorseller/elektronik/enkoder_kartlari.jpg" width="60%" alt="Enkoder taşıyıcı kartları">
</p>
<p align="center">
  <img src="gorseller/elektronik/haberlesme_karti.jpg" width="28%" alt="Haberleşme kartı">
  <img src="gorseller/elektronik/kontrol_kartlari.jpg" width="40%" alt="Kontrol kartları">
</p>
<p align="center">
  <img src="gorseller/elektronik/motor_surucu_kutusu.jpg" width="42%" alt="Motor sürücü ünitesi">
  <img src="gorseller/elektronik/gorev_bilgisayari_kutusu.jpg" width="36%" alt="Görev bilgisayarı kutusu">
</p>
<p align="center"><i>Kara aracı motor sürücü ünitesi (solda) ve görev bilgisayarı muhafazası (sağda)</i></p>

## Görüntü İşleme

<p align="center">
  <img src="gorseller/goruntu_isleme/stereo_kamera.jpg" width="40%" alt="Stereo kamera">
  <img src="gorseller/goruntu_isleme/nokta_bulutu.jpg" width="50%" alt="Nokta bulutu">
</p>
<p align="center"><i>Proje kapsamında geliştirilen stereo kamera ve test ortamından elde edilen 3B nokta bulutu</i></p>

<p align="center">
  <img src="gorseller/goruntu_isleme/stereo_rektifikasyon.png" width="38%" alt="Stereo kalibrasyon">
  <img src="gorseller/goruntu_isleme/test_serasi.jpg" width="38%" alt="Test serası">
</p>
<p align="center"><i>Stereo kamera kalibrasyonu (solda) ve kamera testleri için kurulan yapay sera düzeneği (sağda)</i></p>

Ürünlerin konumu, proje kapsamında geliştirilen stereo kameralarla belirlenmektedir. Kameraların ilk testleri, yaprak ve dal gibi küçük ve karmaşık yapıların bulunduğu yapay bir sera düzeneğinde yapılmıştır. Görüntüler görev bilgisayarında gerçek zamanlı olarak işlenmekte; yapay zekâ modeli ile tespit edilen ürünlerin 3B konumu stereo derinlik bilgisinden hesaplanmaktadır.

<p align="center">
  <img src="gorseller/goruntu_isleme/olgunluk_tespiti.jpg" width="33%" alt="Olgunluk tespiti">
  <img src="gorseller/goruntu_isleme/kesim_noktasi_tespiti.jpg" width="45%" alt="Kesim noktası tespiti">
</p>
<p align="center"><i>Olgun ve ham ürünlerin ayrımı (solda), her domates için sap kesim noktasının tespiti (sağda)</i></p>

Uç efektör sapı tuttuğu için yalnızca ürünün yerini bilmek yeterli değildir; her ürüne ait sapın kesileceği noktanın da belirlenmesi gerekmektedir. Bu amaçla farklı etiketleme ve tespit yaklaşımları karşılaştırılmış, her domatesi kesim noktalarıyla birlikte tek bir nesne olarak tespit eden anahtar nokta (keypoint) tabanlı bir model seçilmiştir. Bu yöntemde kesim noktaları, ayrı bir eşleştirme adımına gerek kalmadan ait oldukları ürünle birlikte üretilmektedir; bu da birbirine yakın sapların bulunduğu salkımlarda belirleyici bir avantaj sağlamaktadır. Test setinde kesim noktalarının %87'si 20 piksel hata sınırı içinde tespit edilmiştir.

Modellerin eğitiminde kullanılan veri setleri, proje kapsamında geliştirilen mobil etiketleme uygulamasıyla hazırlanmıştır.

## Arayüzler

<p align="center"><img src="gorseller/arayuzler/yer_istasyonu.png" width="900" alt="Yer istasyonu"></p>
<p align="center"><i>Yer istasyonu: robot kol sayfasında her eklemin açı, hız, akım, gerilim ve sıcaklık verileri izlenmekte, eklem açısı veya hedef koordinat komutları gönderilebilmektedir.</i></p>

Operatör, masaüstü yer istasyonu ve mobil uygulama üzerinden kara aracını ve robot kolları izleyebilmekte, görev komutları gönderebilmektedir.

<p align="center">
  <img src="gorseller/arayuzler/mobil_acilis.png" width="18%" alt="Mobil uygulama">
  <img src="gorseller/arayuzler/mobil_robot_kol.png" width="18%" alt="Mobil uygulama robot kol">
  <img src="gorseller/arayuzler/mobil_kara_araci.png" width="18%" alt="Mobil uygulama kara aracı">
  <img src="gorseller/arayuzler/mobil_kontrol.png" width="18%" alt="Mobil uygulama kontrol">
</p>
<p align="center"><i>Mobil izleme ve kontrol uygulaması</i></p>

<p align="center">
  <img src="gorseller/arayuzler/etiketleme_1.png" width="18%" alt="Etiketleme uygulaması">
  <img src="gorseller/arayuzler/etiketleme_2.png" width="18%" alt="Etiketleme uygulaması">
  <img src="gorseller/arayuzler/etiketleme_3.png" width="18%" alt="Etiketleme uygulaması">
</p>
<p align="center"><i>Mobil görsel etiketleme uygulaması: birden fazla kullanıcı aynı veri seti üzerinde eş zamanlı etiketleme yapabilmekte, sonuçlar eğitime hazır biçimde dışa aktarılabilmektedir.</i></p>

## Geliştirme Süreci

| Dönem | Öne çıkan çalışmalar |
|---|---|
| **2024 – 2. yarı** | Gereksinim analizi; seraya özgü veri seti ve ilk ürün tespit modeli; stereo mesafe ölçüm cihazı; kara aracı navigasyonu ve robot kol yol planlaması için simülasyon çalışmaları |
| **2025 – 1. yarı** | İlk kara aracı prototipi; planet redüktörlü ilk robot kol uzuvları; kara aracı motor sürücü ünitesi; stereo kamera ile yapay sera testleri; yer istasyonu ve mobil izleme uygulaması |
| **2025 – 2. yarı** | Olgun/ham ayrımı yapan tespit modeli; görev bilgisayarında gerçek zamanlı görüntü işleme; 2. prototip robot kol ve bağımsız süspansiyonlu araç; uzaktan haritalama ve navigasyon |
| **2026 – 1. yarı** | Sap kesim noktası tespit modeli ve mobil etiketleme uygulaması; yeni LiDAR ile navigasyon iyileştirmeleri; 4 robot kolun araç üzerine montajı; enkoder taşıyıcı kartı ve robot kol kontrol kartı |

<p align="center">
  <img src="gorseller/arac/kara_araci_cad_ilk.png" width="40%" alt="İlk kara aracı tasarımı">
  <img src="gorseller/arac/osta_genel.jpg" width="40%" alt="Son hali">
</p>
<p align="center"><i>Kara aracının ilk tasarımı (solda) ve robot kollarla birlikte son hali (sağda)</i></p>

---

Bu proje, TÜBİTAK TEYDEB 1501 Sanayi Ar-Ge Projeleri Destekleme Programı kapsamında desteklenmiştir.

**Deniz Can Aykurt** · Elektrik Elektronik Mühendisi
