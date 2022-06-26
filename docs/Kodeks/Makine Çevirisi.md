Ekip olarak makine çeviri teknolojilerine de göz attık. Proje yöneticilerimiz birkaç deneme yaptı ama büyük projelerde tüm çevirmenlerin kullanabileceği seviyede değil henüz bu teknoloji. Aşağıda birkaç tanesini detaylıca sıraladık ama tamamı için geçerli brkaç maddeyi önden yazalım:

Bulgular:

- **Hiçbir** sağlayıcının çevirisi, tecrübeli birinin kontrolünden geçmeden kullanılamıyor (Biz yalnızca proje yöneticileri ile iyi sonuç alabildik).
- Çevirmenler, **makine çevirisi denetimi sırasında**, kendi çevirilerinde yapmayacakları hatalara sahip cümleleri "sorun yok" diye işaretleyebiliyorlar.
- Yani gönüllü seviyedeki çevirmenin üstünden geçtiği makine çevirinin kalitesi bile **çevirmenin kendi yapacağı çeviriden daha düşük oluyor**. Boşuna zaman ve emek harcanmış oluyor.
- **Terimce desteği** çoğu sağlayıcıda yok. Olanlarda ise çok basit "Bul-Değiştir"den öteye geçemediği için kullanmamak daha iyi oluyor.
- Sağlayıcıların **tamamı**, İngilizce -> Türkçe yerine Türkçe -> İngilizce çevirilerde çok daha başarılı.

Makine çeviri kalitesini ölçmek için birkaç yol var, bunlardan biri de BLEU metriği. Google'ın yaptığı puanlama tablosu aşağıda. Söylediği de şu; 30-40 ve altı puan çeviriler "anlaşılamaz derecede kötü", 30-40 bandındaki çeviriler ise "akıcı olmayan ama çeviri addedilebilen" sonuçlar.

![[BLEU.webp]]

## Intento
[Intento](https://inten.to/), piyasadaki çeviri motorlarını dillere göre karşılaştıran raporlar yayınlıyor; biz de bunlardan fazlasıyla yararlandık. Ayrıca yine uluslararası alanda Intento, Türkiye'de ise [Lugath](https://www.lugath.com/); farklı sağlayıcıları bir araya getirerek size çevirinin en doğru kaynaktan gelmesini sağlıyor. İsterseniz bunlara da göz atabilirsiniz.

[2020 Raporu](https://drive.google.com/file/d/14NSJ2luPgdta9I3d9G-0uxJB0UJfluhQ/view?usp=sharing)  

[2021 Raporu](https://drive.google.com/file/d/1LHDeBUHJPVZ8sQiHNcrmCsyxjeKwDWGr/view?usp=sharing)

## [Amazon Translate](https://aws.amazon.com/tr/translate/)
**BLEU Puanı:** ~30

Amazon, dünyanın en büyük kitap satıcısı olduğu ve dijital formatları elinde bulunduğu için, özellikle didaktik metinlerde iyi sonuçlar veriyor. Ancak genel anlamda ve özellikle konuşma metinlerinde gelenler sonuçlar ortalamanın epey altında.

Bu durum maalesef kendini kaliteli diyalog çevirisi sağladığınız durumlarda da geçerli ki onu da yakın zamanda getirdiler. Terimce desteği ise şaka gibi çalışıyor.

## [Google Cloud Translation](https://cloud.google.com/translate)
**BLEU Puanı:** ~40

Google'ın düz çeviri motoru o kadar başarılı değil. Burada bahsettiğimiz, kendi çevirilerinizi sağlayıp oyuna göre özelleştirebildiğiniz Cloud Translation motoru. Düz siteyi kullanan arkadaşlar en azından zahmete girip bulut tabanlı olanı bir denesinler.

Google'ın kendi temel motoru zaten ortalama sonuçlar veriyor. Kendi çevirilerinizi sağladığınızda da oyun özelinde epey iyi sonuçlar verebiliyor yerine göre. Ancak eğitimi de sonrasında yaptığınız çeviriler de görece pahalı.

## [GPT-3](https://openai.com/api/)
**BLEU Puanı:** 10-20

Nedendir bilinmez makine çevirisi yapanların favorisi ama listedeki en kötü sonuçları bu motorla alıyorsunuz. Sanıyoruz kullananlar ne makaleyi okumuş ~~(İngilizce bilseler zaten...)~~ ne de istatistiklere bakmış.

Makale: https://arxiv.org/abs/2005.14165
İstatistikler: https://github.com/openai/gpt-3

Türkçeden 3 kat daha fazla kelime okuduğu Rumencede bile BLEU puanını 20 yapabilmiş.
![[GPT3_BLEU.webp]]
![[GPT3_Word.webp]]

## [ModernMT](https://www.modernmt.com/)
**BLEU Puanı:** ~40-50 (Listemizin birincisi)

ModernMT'nin üç farklı seçeneği var:

- Sitesinde tek seferde 5000 karaktere kadar ücretsiz çeviri yapabildiğiniz
- Kendinizin ücretsiz şekilde sıfırdan eğitip kullanabileceğiniz
- Var olan temel motorun üstüne kendiniz koyarak geliştirebildiğiniz

Biz üçüncü seçeneği tartışacağız ama isterseniz sıfırdan eğitilebilecek sürümün bağlantısı da aşağıda. Dikkat, sitesindeki aşamaya bile getirmek için *çok* uğraşmak gerekiyor. ~~Gerçi sitedeki motoru kullanıp anlaşılmadığını sananlar da var ama işte...~~

Github: https://github.com/modernmt/modernmt

ModernMT, özellikle de Trados eklentisi ile, siz çeviri yaptıkça kullandığınız dile yakınsayarak listedeki en iyi sonuçları çıkarıyor. Benzer cümlelerdeki değişikliklere de epey iyi uyum sağlayarak *bizce* Google'ın bir tık önüne geçiyor. Fiyat konusunda da makul seçenekler sunuyor.

## [Systran](https://www.systran.net/en/translate/)
**BLEU Puanı:** ~30

Systran, ülkemizde [Dragoman](https://www.dragoman.ist/tr/anasayfa/) ile iş birliği yaparak ortaklaşa bir motor oluşturmuş. Verdiği sonuçlar da göze çarpacak kadar kötü değil ama istikrarlı olarak iyi sonuçlar da sunmuyor.