
![Ekran görüntüsü 2024-11-08 213042](https://github.com/user-attachments/assets/9b2192ee-ccda-492f-a311-74402131daed)

<b> Bu script birden fazla Humanode düğümü (node) çalıştıran ya da tek düğüm çalıştıran ancak tüm gün boyunca ilgilenemeyen kişiler içindir. Scripti her sunucu için ayrı ayrı kurmak gerekiyor. Scripti kullanmadan önce Telegram üzerinden mesajların size gelmesi için bir bot oluşturmalısınız. Eğer sunucu kapanırsa ve geri açılmazsa o zaman bu script çalışmamaktadır. Ancak sunucunun çalışmadığını öğrenmek için ise <a href="https://github.com/Lorento34/humanode/blob/main/Humanode%20Sunucu%20Kontrol.md"><b>```Humanode Sunucu Kontrol Scripti```</b></a> kurmanızda fayda var. Bunun nedeni sunucun çalışmadığından haberiniz olursa Humanode uygulamasınında çalışmadığından haberiniz olacaktır. Eğer sunucu tarafında 

<b> Bu scriptin özelliğine gelecek olursak şöyle sıralayabiliriz; </b>
- Humanode uygulaması kapanırsa telegram üzerinden size bilgilendirme mesajı gönderiyor.
- Humanode uygulamasını manuel olarak tekrar aktif duruma getirdiğinizde de telegram üzerinden mesaj gönderiyor.
- Bir problemden dolayı Humanode düğümünün kurulu olduğu sunucu kapandıktan sonra sunucu tekrar aktif olduğunda Humanode uygulaması çalışmayı durduracaktır. Ancak script çalışmaya devam ettiği için ugulamanın çalışmadığını bildiren mesajı gönderecektir. </b>

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


# Humanode Uygulama Kontrol Scripti

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

> <b> Açıklama: </b> Scripte değiştireceğiniz kısımlar sırasıyla, ```YOUR_BOT_TOKEN``` ve ```YOUR_CHAT_ID``` bunun dışında telegram için size gelecek mesajlarıda düzenleyebilirsiniz. Ekran görüntüsünde de işaretledim değiştireceğiniz kısımları. Tırnak işaretlerini  silmeyin.

![Ekran görüntüsü 2024-11-08 210810](https://github.com/user-attachments/assets/ab04a66a-eef3-4146-b1ea-b52bd6988269)


```Sieve
import requests
import subprocess
import time

BOT_TOKEN = "YOUR_BOT_TOKEN"
CHAT_ID = "YOUR_CHAT_ID"
CHECK_INTERVAL = 60  # Kontrol aralığı (saniye cinsinden)
RETRY_INTERVAL = 120  # Launcher kapalıyken tekrar mesaj göndermek için bekleme süresi
LAUNCHER_TIMEOUT = 10  

launcher_is_down = False  
last_message_time = 0

def send_telegram_message(message):
    url = f"https://api.telegram.org/bot{BOT_TOKEN}/sendMessage"
    data = {"chat_id": YOUR_CHAT_ID, "text": message}
    try:
        response = requests.post(url, data=data)
        if response.status_code != 200:
            print(f"Mesaj gönderilemedi. Hata kodu: {response.status_code}")
        else:
            print("Mesaj gönderildi.")
    except Exception as e:
        print(f"Telegram mesajı gönderilemedi: {e}")

def check_launcher_status():
    global launcher_is_down, last_message_time
    try:
        result = subprocess.run(["pgrep", "-f", "humanode-peer"], capture_output=True, text=True, timeout=LAUNCHER_TIMEOUT)
        if result.returncode != 0:
            current_time = time.time()
            if not launcher_is_down or (current_time - last_message_time >= RETRY_INTERVAL):
                print("Humanode launcher çalışmıyor, Telegram'a mesaj gönderiliyor...")             # Bu kısımları kendinize göre düzenleyin
                send_telegram_message("🛠️ 1903 IP Numaralı, Test kullanıcısının humanode uygulaması çalışmıyor!")
                last_message_time = current_time
            launcher_is_down = True
        else:
            if launcher_is_down:
                print("Humanode launcher çalışıyor.")                       # Bu kısımları kendinize göre düzenleyin
                send_telegram_message("✅ 1903 IP Numaralı, Test kullanıcısının humanode uygulaması çalışmaya başladı!(Bu kısımları kendinize göre düzenleyin)")
                launcher_is_down = False
    except subprocess.TimeoutExpired:
        print("Launcher kontrolü zaman aşımına uğradı.")                    # Bu kısımları kendinize göre düzenleyin
        send_telegram_message("⚠️ 1903 IP Numaralı, Test kullanıcısının humanode uygulaması kontrolü zaman aşımına uğradı!(Bu kısımları kendinize göre düzenleyin)")
        launcher_is_down = True
    except Exception as e:
        print(f"Launcher kontrolünde bir hata oluştu: {e}")                 # Bu kısımları kendinize göre düzenleyin
        send_telegram_message(f"1903 IP Numaralı, Test kullanıcısının humanode uygulaması kontrolü sırasında bir hata oluştu(Bu kısımları kendinize göre düzenleyin): {e}")
        launcher_is_down = True
    return launcher_is_down

def check_server_connection():
    try:
        response = requests.get("https://api.telegram.org", timeout=5)
        if response.status_code == 200:
            return True
    except requests.exceptions.RequestException:
        pass
    return False
                                                                             # Bu kısımları kendinize göre düzenleyin
send_telegram_message("🖥️ 1903 IP Numaralı, Test kullanıcısının sunucu ve humanode uygulaması izlenmeye başlandı...")

while True:
    if not check_server_connection():
        time.sleep(CHECK_INTERVAL)
        continue
    check_launcher_status()
    time.sleep(CHECK_INTERVAL)
```

Script içinde ki komutları kendi bilgilerinize göre düzenledikten sonra dosyaya yapıştırıp, ```ctrl + x``` ardından ```y``` basınız sonra ```enter```'e basıp kaydedip çıkınız.


7- Script dosyasını çalıştırılabilir hale getiriniz.
```
sudo chmod +x /usr/local/bin/humanode_monitor.py
```

8- Scripti bir servis olarak çalıştırmak için bir systemd servis dosyası oluşturmanız gerekmektedir. Bunun için aşağıda ki komut ile boş bir systemd servis dosyası açınız.
```
sudo nano /etc/systemd/system/humanode_monitor.service
```

9- Systemd servis dosyası için açtığınız boş dosyaya aşağıdaki komutları değiştirmeden yapıştırınız. Bu komutlar, scriptin sistem başlatıldığında otomatik olarak başlamasını sağlar.
```
[Unit]
Description=Humanode Launcher İzleme Scripti
After=network.target

[Service]
ExecStart=/usr/bin/python3 /usr/local/bin/humanode_monitor.py
Restart=always
RestartSec=10
User=root

[Install]
WantedBy=multi-user.target
```

Komutları dosyaya yapıştırıp, ```ctrl + x``` ardından ```y``` basınız sonra ```enter```'e basıp kaydedip çıkınız.


10- Servisi yükleme, etkinleştirme ve başlatma komutları
```
sudo systemctl daemon-reload
```
```
sudo systemctl enable humanode_monitor.service
```
```
sudo systemctl start humanode_monitor.service
```

11- Scriptin durumunu kontrol etmek ve restart atmak için aşağıda ki komutları kullanınız.
```
sudo systemctl status humanode_monitor.service
```
```
sudo systemctl restart humanode_monitor.service
```


