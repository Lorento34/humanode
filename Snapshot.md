![Ekran görüntüsü 2024-11-08 213042](https://github.com/user-attachments/assets/a500a951-735e-4356-b76a-0ab0fc9b8afb)


Merhaba arkadaşlar, sizlere en güncel bloğa yakın olacak şekilde her hafta Cuma günü yedek ```(snapshot)``` paylaşımı yapacağım. Bu yedeği, yeni düğüm ```(node)``` kurulumu yaparken, düğüm taşıması yaparken ya da sorunlu, çalışmayan bir sunucudan düğümünüzü acil bir şekilde taşımak isterseniz kullanabilirsiniz.

Kurulumu çok basit. İster yeni kurulum yapın, ister taşıma yapın farketmez ilk önce düğümünüzü kurun normal bir şekilde. Humanode uygulamasında ```(launcher)``` bulunan ```log``` kısmına gidin. Bir kaç blok geçtikten sonra uygulamada ki ```stop``` butonuna basarak düğümünüzü durdurun. Humanode'nin kurulu olduğu sunucuya giriş yapın terminal üzerinden. Yedek kurulumuna aşağıda ki komutlarla devam edin...

------

# Yedek Kurulumu

1- Eski düğümün dosyalarını silin.
```4D
rm -rf /root/.humanode/workspaces/default/substrate-data/chains/humanode_mainnet/db/full
```

2- Aşağıda ki komut ile kurulumunu başlatın. Bu yedek sizi ```#10953728. bloktan``` başlatacaktır. Bu arada yedek dosyasının büyüklüğü ve sunucunuzun internet hızına bağlı olarak bekleme süreniz değişkenlik gösterebilir. Ancak unutmayın ki yedek kurulumu tamamlanmadan terminali kapatmamalısınız!

> [!CAUTION]
> ```03.01.2025``` tarihinde güncellenmiştir:exclamation::exclamation::exclamation:

```Sieve
curl -L http://89.116.25.136/03012025/snapshot.tar.gz | tar -xz -C /root/.humanode/workspaces/default/substrate-data/chains/humanode_mainnet/db/
```

3- Kurulum tamamlandıktan sonra tekrardan Humanode uygulamasına giriş yapın ve ```Start The Node``` butonuna basın. Eğer uygulama çalışmazsa o zaman ```Node Validator Key``` yeşil olana kadar uygulamayı bir kaç kere kapatıp geri açın. İşlemler bu kadar.

![Ekran görüntüsü 2024-11-09 053456](https://github.com/user-attachments/assets/15d1ae14-4eeb-4afc-bf3e-159fb12ec4a1)![gggg](https://github.com/user-attachments/assets/03814192-f9d3-43bc-bd65-47558ad7c4af)



