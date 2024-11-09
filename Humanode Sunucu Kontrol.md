![Ekran görüntüsü 2024-11-08 213042](https://github.com/user-attachments/assets/2d6ad775-bc70-4790-8130-70ccfa6851f2)

Humanode uygulamasını kontrol için bu scripti'de kurmalısınız. Bu script sadece sunucuları takip ediyor. Yani sunucu ile bağlantı kurulamazsa ya da sunucu kapanırsa size mesaj gönderiyor telegram üzerinden. Bu scriptin önemi şöyle büyük, eğer sunucu kapanırsa ya da iletişim kesilirse sunucu ile o zaman Humanode Uygulama Kontrol scripti çalışmıyor. Bu script ile size mesaj gelecek sunucunun çalışmadığına dair. Bunun anlamı Humanode uygulamasıda çalışmıyor demektir. Hemen sunucuda ki problemi çözebiliyorsanız hızlı bir şekilde kontrol etmeniz için size zaman kazandıracaktır. Böylelikle Humanode uygulamasını tekrardan aktif hale getirebilirsiniz selfieniz silinmeden.



### Humanode Sunucu Kontrol Scripti

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
