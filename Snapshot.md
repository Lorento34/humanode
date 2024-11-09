![Ekran görüntüsü 2024-11-08 213042](https://github.com/user-attachments/assets/a500a951-735e-4356-b76a-0ab0fc9b8afb)

<b>
Merhaba arkadaşlar. Sizlere en güncel bloğa yakın olacak şekilde haftalık yedek ```(snapshot)``` paylaşımı yapacağım. Bu yedeği yeni düğüm ```(node)``` kurulumu yaparken, düğüm taşıması yaparken ya da sorunlu, çalışmayan bir sunucudan düğümünüzü acil bir şekilde taşımak isterseniz kullanabilirsiniz.
</b>

<b>
Kurulumu çok basit. İster yeni kurulum yapın, ister taşıma yapın farketmez ilk önce düğümünüzü kurun. Humanode uygulamasında ```(launcher)``` log kısmında gidin bir kaç blok geçtikten sonra uygulamada ki ```stop``` butonuna basarak düğümünüzü durdurun. Konsolunuzu açın ve aşağıda ki komutlardan devam edin yedeği kurmaya.
</b>
------

1- Bu komut ile tüm dosyaları silin.
```
rm -rf /root/.humanode/workspaces/default/substrate-data/chains/humanode_mainnet/db/full
```

2- Bu komut ile yedeğin kurulumuna başlayabilirsiniz. Bu yedek sizi 10123906 Bloktan başlatacaktır.
```
curl -L http://100.42.185.156/full_snapshot.tar.gz | tar -xz -C /root/.humanode/workspaces/default/substrate-data/chains/humanode_mainnet/db/
```
![Ekran görüntüsü 2024-11-09 052419](https://github.com/user-attachments/assets/977c976e-0454-49b9-8045-22be6a32e5e8)

3- Kurulum tamamlandıktan sonra tekrardan Humanode uygulamasına giriş yapın ve ```start``` butonuna basın. Bu arada yedek dosyasının büyüklüğü ve sunucunuzun internet hızına bağlı olarak bekleme süreniz değişkenlik gösterebilir. Ancak unutmayın ki yedek kurulumu tamamlanmadan konsolu kapatmamalısınız.

![Ekran görüntüsü 2024-11-09 053456](https://github.com/user-attachments/assets/e1ed2d62-07f2-4d33-8516-d2c4788e86f5)

