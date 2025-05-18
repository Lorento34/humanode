

🚀 Snapshot Loader Humanode ağı için kolay, hızlı ve güvenli bir şekilde yedek ```(snapshot)``` indirme ve kurulum işlemlerini gerçekleştiren bir bash scriptidir. Bu araç, Humanode düğümünüzü ```(node)``` en güncel bloğa yakın olacak şekilde başlatmanızı sağlar ve kurulum sürecini basitleştirir.

### Özellikler 
- 🌟 **Kolay Kurulum**: Tek bir komut ile yedek dosyasını indirin, yükleyin ve düğümünüzü başlatın.
- 📦 **Gereksinim Kontrolü**: İhtiyaç duyulan tüm bağımlılıkları otomatik kontrol eder ve yükler.
- ⏱️ **Modern İlerleme Çubuğu**: İndirme hızını, geçen süreyi görselleştirir.
- 🛡️ **Güvenilirlik**: Özel tasarımıyla Humanode ağı için optimize edilmiştir.

### Nasıl Çalışır? 
İster yeni kurulum yapın, ister düğüm taşıması yapın farketmez bu araç tam size göre! ilk önce düğümünüzü kurun normal bir şekilde. Humanode uygulamasında ```(launcher)``` bulunan ```log``` kısmına gidin. Bir kaç blok geçtikten sonra uygulamada ki ```stop``` butonuna basarak düğümünüzü durdurun. Humanode'nun kurulu olduğu sunucuya giriş yapın terminal üzerinden. Yedek kurulumuna aşağıda ki komutlarla devam edin...

### Katkı
Bu repoya katkıda bulunmak isterseniz, lütfen bir pull request gönderin veya bir issue oluşturun!

------

# Yedek Kurulum Adımları

1- Eski düğümün dosyalarını silin.
```4D
rm -rf /root/.humanode/workspaces/default/substrate-data/chains/humanode_mainnet/db/full
```

2- Aşağıda ki komut ile kurulumu başlatın.

```Sieve
curl -L http://89.116.25.136/19052025/snapshot.tar.gz | tar -xz -C /root/.humanode/workspaces/default/substrate-data/chains/humanode_mainnet/db/
```

3- Kurulum tamamlandıktan sonra tekrardan Humanode uygulamasına giriş yapın ve ```Start The Node``` butonuna basın. 

> [!CAUTION]
> :warning: Dikkat: Yedek kurulumu tamamlanmadan terminali kapatmayın! Aksi takdirde işlem yarım kalacaktır!
> 
> :warning: Yedeği yükledikten sonra eğer uygulama çalışmazsa o zaman ```Node Validator Key``` yeşil olana kadar uygulamayı bir kaç kere kapatıp geri açın. Genelde uygulamayı 2-3 kere katıp açtıktan sonra düzeliyor.

![Ekran görüntüsü 2024-11-09 053456](https://github.com/user-attachments/assets/15d1ae14-4eeb-4afc-bf3e-159fb12ec4a1)![gggg](https://github.com/user-attachments/assets/03814192-f9d3-43bc-bd65-47558ad7c4af)


---



