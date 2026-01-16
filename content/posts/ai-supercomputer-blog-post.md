# 5 Mac Studio ile Dev Yapay Zeka Kümesi Kurmak: Ev Yapımı Bir Süper Bilgisayar

Llama 3.1 405B gibi devasa yapay zeka modellerini evde çalıştırmak mümkün mü? Genelde bu çapta modeller, evlerimizden daha pahalı sunucularda, devasa veri merkezlerinde çalıştırılır. Ancak bugün sınırları zorluyoruz: **5 adet Mac Studio'yu birbirine bağlayarak kendi yapay zeka kümemizi oluşturuyoruz.**

---

## Neden Bir Yapay Zeka Kümesi?

Pek çok kişi bilgisayarında Llama 3.2 1B gibi küçük modelleri çalıştırabilir. Ancak iş, ChatGPT kalitesinde akıl yürütebilen dev modellere geldiğinde (Llama 3.1 405B veya Llama 3.3 70B gibi), tek bir bilgisayarın gücü yetersiz kalır.

Buradaki anahtar kavram **Parametreler**dir.

- **1B (1 Milyar Parametre)**: Temel cümle tamamlama ve özetleme için iyidir.
- **70B (70 Milyar Parametre)**: Karmaşık mantık yürütme için gerekir.
- **405B (405 Milyar Parametre)**: Dünyanın en güçlü açık kaynaklı modeli. Çalıştırmak için yaklaşık **1 Terabayt VRAM** öneriliyor!

Tüketici sınıfı en güçlü GPU olan Nvidia RTX 4090 bile (24 GB VRAM) bu modeli tek başına çalıştıramaz. Teoride 42 adet 4090'a ihtiyacınız olurdu.

---

## Mac'in Gizli Silahı: Birleşik Bellek (Unified Memory)

Nvidia GPU'lar çok hızlıdır ancak bellek miktarı sınırlıdır. Apple'ın M serisi işlemcileri ise **Birleşik Bellek Mimarisi** sayesinde sistem RAM'ini GPU belleği olarak kullanabilir.

Her bir Mac Studio'muzda 64 GB RAM var. 5 tanesini birleştirdiğimizde toplamda **320 GB GPU için kullanılabilir bellek** elde ediyoruz. Bu, maliyet/performans açısından eşsiz bir avantaj sağlıyor.

---

## Yazılım ve Bağlantı: EXO Labs

Bu kümelemeyi sağlamak için **EXO Labs** adında, henüz beta aşamasında olan harika bir yazılım kullanıyoruz. EXO, ağ üzerindeki farklı cihazları (Mac, Raspberry Pi, PC) otomatik olarak keşfeder ve yapay zeka modelini bu cihazlar arasında paylaştırır.

### Bağlantı Seçenekleri:

1. **10 Gb Ethernet**: Kolay kurulum ancak büyük bir darboğaz. Yapay zeka ağlarında normalde saniyede 800 gigabitlik hızlar konuşulur.
2. **Thunderbolt Bridge**: Doğrudan PCIe bağlantısı sağlayarak 40 Gbps hıza ulaşır. Bu, performansın saniyede 10 tokendan 50 tokene (küçük modeller için) çıkmasını sağlar.

---

## Büyük Sınav: Llama 3.1 405B

Kümemiz hazır olduğunda en büyük hedefimize yöneldik: 405B parametreli Llama modelini çalıştırmak.

- **Sonuç**: Başardık!
- **Hız**: Saniyede yaklaşık 5-8 token.
- **Analiz**: Belki dünyanın en hızlısı değil ama evdeki donanımlarla, bir veri merkezine muhtaç kalmadan dünyanın en güçlü yapay zekasını yerel olarak çalıştırmayı başardık.

---

## Otomasyon ve Gelecek

Yapay zeka modelleri geliştikçe, donanım gereksinimleri de artıyor. Ancak EXO Labs gibi projeler ve Mac'in birleşik bellek mimarisi, bu gücü bireysel kullanıcıların ellerine teslim ediyor. Artık bulut devlerine (OpenAI, Google) verilerimizi teslim etmek zorunda değiliz; her şey yerel, her şey bizim kontrolümüzde.

> [!TIP]
> Kendi kümenizi kurmak isterseniz Python 3.12 ve `pip install mlx` kütüphanelerini kurarak başlayabilirsiniz.

---

**Kategori:** Yapay Zeka, Donanım, Rehber
**Etiketler:** #MacStudio #AI #Llama3 #EXOLabs #Supercomputer #MachineLearning
