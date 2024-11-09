
 

<b> Bu script birden fazla humanode düğümü (node) çalıştıran ya da tek düğüm çalıştıran ancak tüm gün boyunca ilgilenemeyen kişiler içindir. Humanode son güncelleme sonrasında bazı değişikllere gitti. Humanode uygulamasına (launcher) uzun süre bakılmadığı taktirde ve bu süre içinde uzun süre kapalı konumda olan Humanode uygulaması yüzünden düğüm (node) çok fazla blok kaçırdığı için halı hazırda yapılmış olan selfie'yi sistem otomatik olarak siliyor. Bunun sonucunda 2 haftalık ödül alma şansı kaybediliyor. Bunun önüne geçmek için aşağıda ki scripti kullanabilirsiniz. Scripti kullanmadan önce Telegram üzerinden mesajların size gelmesi için bir bot oluşturmalısınız. 

Bu scriptin özelliğine gelecek olursak şöyle sıralayabiliriz;

- Humanode uygulaması kapanırsa telegram üzerinden size bilgilendirme mesajı gönderiyor.
- Humanode uygulamasını tekrar aktif yaptığınızda da sistem bunu görüp aktif olduğu konusunda telegram üzerinden mesaj gönderiyor.
- Script servis dosyası olarak çalıştığı için sunucuda ki herhangi bir sorundan dolayı sunucu kapanırsa ya da belli bir süre iletişim kuralamazsa bile sunucu aktif olduktan sonra script çalışmaya devam ediyor. </b>

------

# Telegram Botu Oluşturma

1- Telegram arama yerine ```@BotFather``` yazın. Bota tıklayıp ve başlata basınız.

![Telegram bot 1](https://github.com/user-attachments/assets/0a7899d7-915b-4d6e-a01d-4ce0b743d4ab)

2- Menüye tıklayın ```/newbot``` yazan komuta tıklayınız. Botunuz için bir isim yazıp ve entere basınız. 

![Telegram bot 1](https://github.com/user-attachments/assets/1a0c882f-0387-4f6f-9e67-4690514a5bc0) ![Telegram bot 2](https://github.com/user-attachments/assets/de137183-b8cd-4e42-9aa8-8895a5c79788)

3- Bu kısımda botunuz için kullanıcı adı belirleyeceksiniz. Kullanıcı adınızın sonu bot ile bitmek zorunda.

![Telegram bot 3](https://github.com/user-attachments/assets/fabf269b-f33f-4992-8bb1-469ed3cac708)

4- Script için kullanacağınız ```BOT_TOKEN``` kodunu verecek. Bu kodu iyi saklayın başkalarıyla paylaşmayın. Botunuzun linkine tıklayıp, botunuzla sohbet başlatın. 

![Telegram bot 4](https://github.com/user-attachments/assets/d30ac842-6bd6-49ee-97d9-e6a6e7aed637) ![2](https://github.com/user-attachments/assets/c77c8df6-3413-4088-a215-a8464aa6993c)

5- ```https://api.telegram.org/bot{BOT_TOKEN}/getUpdates``` bu siteyi tarayıcınızda açın. ```{BOT_TOKEN}``` yazan yere bot tokeninizi yazacaksınız. Size bir çıktı verecek. Eğer çıktı çıkmazsa o zaman botunuza bir şeyler yazın ya da ```/start``` butonuna basın tekrardan. Aşağıda ki resimde ki gibi çıktı vermesi gerekiyor. Sayfayı yenileyin İşaretlediğim yerde ```CHAT_ID``` var onu da kaydedin. Bot için yapacaklarınız bu kadar. 

```Örnek link: https://api.telegram.org/bot7477533540:AAgg3dhSQe4M546BJKyufyWhakYS8Pg8p15Kc/getUpdates```

![5](https://github.com/user-attachments/assets/2bdd475f-f7c0-4751-9d51-8eaf1cbefc04)

------


# Humanode kontrol scripti

1- Sunucuda ki paket listelerini güncelleyiniz.

```
sudo apt update
```

2- Sunucuda yüklü paketleri en son sürümlerine yükseltiniz.
```
sudo apt upgrade
```

3- Python paket yöneticisi Pip'i kurunuz.
```
sudo apt install -y python3-pip
```

4- Aşağıda ki komut Python için ```requests``` ve ```python-telegram-bot``` kütüphanelerini kurar, bu kütüphaneler Telegram botu ile iletişim ve HTTP istekleri yapmak için gereklidir.
```
pip install requests python-telegram-bot
```

5- Script dosyası oluşturunuz.
```
sudo nano /usr/local/bin/humanode_monitor.py
```

6- Açtığınız boş script dosyasına aşağıda ki scripti kendi bilgilerinize göre düzenleyip yapıştırınız. Değiştireceğiz kısımlar aşağıda belirtilmiştir.

```Açıklama:``` örnek
```sudo:``` Dosya, kök dizin altında olduğundan süper kullanıcı yetkisi gerektirir.
```rm:``` Dosyayı silmek için kullanılan komut.
/usr/local/bin/server_monitor.py: Silmek istediğiniz dosyanın tam yolu.

Script 


Script içinde ki komutları kendi bilgilerinize göre düzenledikten sonra dosyaya yapıştırıp, ctrl + x ardından y basınız sonra enter'e basıp kaydedip çıkınız.


7- Script dosyasını çalıştırılabilir hale getiriniz.

sudo chmod +x /usr/local/bin/humanode_monitor.py


8- Scripti bir servis olarak çalıştırmak için bir systemd servis dosyası oluşturmanız gerekmektedir. Bunun için aşağıda ki komut ile boş bir systemd servis dosyası açınız.

sudo nano /etc/systemd/system/humanode_monitor.service


9- Systemd servis dosyası için açtığınız boş dosyaya aşağıdaki komutları değiştirmeden yapıştırınız. Bu komutlar, scriptin sistem başlatıldığında otomatik olarak başlamasını sağlar.

komutlar


Komutları dosyaya yapıştırıp, ctrl + x ardından y basınız sonra enter'e basıp kaydedip çıkınız.


10- Servisi yükleme, etkinleştirme ve başlatma komutları

sudo systemctl daemon-reload

sudo systemctl enable humanode_monitor.service

sudo systemctl start humanode_monitor.service


11- Scriptin durumunu kontrol etmek ve restart atmak için aşağıda ki komutları kullanınız.

sudo systemctl status humanode_monitor.service

sudo systemctl restart humanode_monitor.service


