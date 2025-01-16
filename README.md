# ChestX-ray8 Veri Seti Üzerinde Görüntü İşleme ve Segmentasyon

Bu proje kapsamında, ChestX-ray8 veri seti kullanılmıştır. ChestX-ray8, göğüs hastalıklarının teşhisine yönelik olarak hazırlanmış bir röntgen görüntü veri setidir. Veri setinde göğüs röntgeni görüntüleri ve bu görüntülere ait çeşitli hastalık etiketleri bulunmaktadır (örneğin, atelectasis, effusion, pneumonia). Görseller genellikle düşük çözünürlüklü olup, tıbbi teşhis için hem makine öğrenimi modelleri hem de görüntü işleme tekniklerinin uygulanması için uygundur.

## Veri İnceleme ve Görselleştirme

- Görsellerin ve hastalık etiketlerinin incelenmesi, istatistiksel analizler.
- Rastgele Görüntü Seçimi: Veri setinden rastgele 9 görüntü seçilerek görselleştirme.

## Pre-processing (Ön İşleme)
1. Akciğer Bölgesi Odaklı Görüntü Kırpma

Görsellerde akciğer bölgesine odaklanılarak, gereksiz alanların etkisini azaltmak amacıyla kırpma işlemi uygulanmıştır.
2. Görüntü Geliştirme ve Kontrast İyileştirme

Bu adımda, akciğer bölgesine odaklanılmış kırpılmış görüntülerin kontrastını iyileştirmek amacıyla kontrast germe işlemi uygulanmıştır. Bu işlem, görüntülerdeki parlaklık aralığını genişleterek detayların daha belirgin hale gelmesini sağlamıştır.
3. Gürültü Azaltma İşlemi

Görüntüdeki yüksek frekanslı gürültülerin etkisini azaltmak için Gaussian Blur (Gauss Bulanıklaştırma) işlemi uygulanmıştır.
4. Eşik Sayısı Belirleme

Eşik sayısı belirleme adımında, görüntülerin piksel yoğunlukları analiz edilerek üç farklı bölge (düşük, orta ve yüksek yoğunluk) için eşik değerleri belirlenmiştir. Ortalama piksel yoğunluğu ve standart sapma kullanılarak alt ve üst eşik değerleri dinamik bir şekilde hesaplanmış, bu sayede yalnızca tek bir eşik yerine üç yoğunluk bölgesi ayrıştırılmıştır. Düşük yoğunluk, orta yoğunluk ve yüksek yoğunluk bölgeleri için maskeler oluşturulmuş ve bu maskeler üzerinden görselleştirme yapılmıştır.
5. Thresholding (Eşikleme)

Thresholding işlemi sırasında, sabit eşikleme, Otsu yöntemi ve try_all_threshold fonksiyonu kullanılarak görüntüler ikili forma dönüştürülmüştür. Sabit eşikleme, tüm pikseller için aynı değeri kullanırken, Otsu yöntemi eşik değerini otomatik olarak belirlemiştir. Ayrıca, try_all_threshold ile farklı eşikleme algoritmalarının sonuçları karşılaştırılmış ve uygun yöntem seçimi yapılmıştır. Bu işlem, görüntüde nesne ve arka planın etkili bir şekilde ayrılmasını sağlamıştır.

## Post-processing (Son İşleme)

1. Morfolojik Operatörlerin ve Yapısal Elemanların Seçimi ve Uygulanması

- Closing (Kapama) İşlemi: Akciğer bölgesindeki boşlukları doldurmak ve yapısal bütünlüğü sağlamak için kapama işlemi uygulanmıştır.
- Küçük nesneler, analizden çıkarılmak üzere remove_small_objects fonksiyonu ile temizlenmiştir.
2. Connected Component Labeling (CCL) Analizi

Görüntülerdeki her bir bağlantılı bölge tespit edilip etiketlenmiştir. Bu işlem sonucunda her bölgeye benzersiz bir etiket atanmıştır.
3. Bölgesel Özniteliklerin Çıkarılması ve Analizi

Etiketlenen bölgelerden, alan, eksantriklik, kompaktlık ve merkez noktaları gibi öznitelikler çıkarılmıştır. Bu öznitelikler, segmentasyon sonuçlarının değerlendirilmesinde kullanılmıştır.
4. Filtrelenmiş Görüntülerde Seçilen Etiketler Üzerine Morfolojik İşlemlerin Uygulanması ve Görselleştirilmesi:

Etiketlenen bölgelerden anlamlı alanlar seçilmiş ve görselleştirilmiştir. Her etiket, görselde farklı renklerle ifade edilmiştir.

## Sonuç Analizi
Sonuç analizi aşamasında, görüntü işleme sonrası elde edilen maskeler, bağlantılı bileşen analiziyle oluşturulmuş ve orijinal görüntüyle çarpılarak görselleştirilmiştir.

### Colab Notebook

[Notebook]([https://colab.research.google.com/drive/1aiLwjmFpj0WsouVB7ZxG9eI7nyoTl3ps?usp=sharing])
