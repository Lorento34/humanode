![Ekran görüntüsü 2024-11-08 213042](https://github.com/user-attachments/assets/2d6ad775-bc70-4790-8130-70ccfa6851f2)

Humanode uygulamasını kontrol için bu scripti'de kurmalısınız. Çünkü eğer Humanode düğümünü kurduğunuz sunucu çalışmıyorsa o zaman <a href="https://github.com/Lorento34/humanode/blob/main/Humanode%20Uygulama%20Kontrol.md"><b>```Humanode Uygulama Kontrol scripti```</b></a> de çalışmaz ve size telegram üzerinden mesaj gönderemez. Bu script sadece Humanode düğümünün kurulu olduğu sunucuları takip ediyor. Yani Humanode düğümünün kurulu olduğu sunucu ile bağlantı kurulamazsa ya da sunucu kapanırsa size mesaj gönderiyor telegram üzerinden. Ayrıca Humanode Sunucu Kontrol Script'ni sadece bir sunucuya kurmanız yeterlidir. Eğer istersenis sorun yaşamadığınız başka bir sunucuya kurabilirsiniz ya da Humanode düğümü olan bir sunucuyada kurabilirsiniz. Bu script için Telegram botunu kullanmanız gerekiyor. Ancak <a href="https://github.com/Lorento34/humanode/blob/main/Humanode%20Uygulama%20Kontrol.md"><b>```Humanode Uygulama Kontrol scripti```</b></a> için oluşturduğunuz Telegram botunu bilgilerini kullanabilirsiniz.



# Humanode Sunucu Kontrol Script Kurulumu

1- Sunucuda ki paket listelerini güncelleyin.

```
sudo apt update
```

2- Sunucuda yüklü paketleri en son sürümlerine yükseltin.
```
sudo apt upgrade
```

3- Python paket yöneticisi Pip'i kurun.
```
sudo apt install -y python3-pip
```

4- Aşağıda ki komut Python için ```requests``` ve ```python-telegram-bot``` kütüphanelerini kurar, bu kütüphaneler Telegram botu ile iletişim ve HTTP istekleri yapmak için gereklidir.
```
pip install requests python-telegram-bot
```

5- Script dosyası oluşturun.
```
sudo nano /usr/local/bin/server_monitor.py
```

6- Açtığınız boş script dosyasına aşağıda ki scripti kendi bilgilerinize göre düzenleyip yapıştırın. Değiştireceğiz kısımlar aşağıda belirtilmiştir.

> <b> Açıklama: </b> Scripte değiştireceğiniz kısımlar sırasıyla, ```YOUR_BOT_TOKEN``` ve ```YOUR_CHAT_ID``` bunun dışında takip edilmesi gereken IP adreslerini değiştirin. Aşağıda ki IP adresleri örnektir, siz kendi ip adresleri ile değiştireceksiniz. Ekran görüntüsünde de işaretledim değiştireceğiniz kısımları. Tırnak işaretlerini  silmeyin.

![www](https://github.com/user-attachments/assets/a4278284-8e33-47e2-9051-3d2368238d30)



```Sieve
import requests
import time
import subprocess

BOT_TOKEN = "YOUR_BOT_TOKEN"
CHAT_ID = "YOUR_CHAT_ID"
CHECK_INTERVAL = 60  # Kontrol aralığı (saniye cinsinden)
RETRY_INTERVAL = 1200  # Bağlantı yoksa her 20 dakikada bir mesaj gönderme
PING_ATTEMPTS = 3  # Her sunucuya kaç kez ping atılacak

# İzlenecek sunucuların IP adresleri
SERVER_IPS = [
	"149.102.146.13", "84.241.122.185", "100.41.185.170", "89.141.103.82", "149.101.111.11",
        "62.84.181.46", "62.82.180.63", "638.242.241.189", "141.102.138.150", "185.225.166.1"
    
]

server_status = {ip: {"down": False, "last_alert_time": 0} for ip in SERVER_IPS}

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

def ping_server(ip):
    """Sunucuya belirli sayıda ping atar ve bağlantı durumunu döndürür."""
    successful_pings = 0
    for _ in range(PING_ATTEMPTS):
        result = subprocess.run(["ping", "-c", "1", ip], capture_output=True)
        if result.returncode == 0:  
            successful_pings += 1
    return successful_pings > 0  

def check_server_status():
    current_time = time.time()  
    for ip in SERVER_IPS:
        try:
            is_online = ping_server(ip)  
            if not is_online:  
                if not server_status[ip]["down"] or (current_time - server_status[ip]["last_alert_time"] >= RETRY_INTERVAL):
                    # İlk kez kapalı duruma geçtiyse veya son uyarıdan sonra 20 dakika geçtiyse
                    print(f"{ip} sunucusuna ulaşılamıyor, Telegram'a mesaj gönderiliyor...")
                    send_telegram_message(f"⚠️ {ip} sunucusuna ulaşılamıyor!")
                    server_status[ip]["down"] = True  
                    server_status[ip]["last_alert_time"] = current_time  
            else:  
                if server_status[ip]["down"]:
                    print(f"{ip} sunucusu yeniden bağlandı.")
                    send_telegram_message(f"🔄 {ip} sunucusu online oldu.")
                    server_status[ip]["down"] = False  
                    server_status[ip]["last_alert_time"] = 0  
        except Exception as e:
            print(f"{ip} sunucu durumu kontrol edilirken hata oluştu: {e}")

# İzlemeye başlama mesajı gönder
send_telegram_message("🖥️ Sunucu izleme başlatıldı...")

while True:
    check_server_status()
    time.sleep(CHECK_INTERVAL)
```

Script içinde ki komutları kendi bilgilerinize göre düzenledikten sonra scripti terminale yapıştırıp, ```ctrl + x``` ardından ```y``` basın, sonra ```enter```'e basıp kaydedip çıkın.


7- Script dosyasını çalıştırılabilir hale getirin.

```Sieve
sudo chmod +x /usr/local/bin/server_monitor.py
```

8- Scripti bir servis olarak çalıştırmak için bir systemd servis dosyası oluşturmanız gerekmektedir. Bunun için aşağıda ki komut ile boş bir systemd servis dosyası açın.

```Sieve
sudo nano /etc/systemd/system/server_monitor.service
```

9- Systemd servis dosyası için açtığınız boş dosyaya aşağıdaki komutları değiştirmeden yapıştırın. Bu komutlar, scriptin sistem başlatıldığında otomatik olarak başlamasını sağlar.

```Sieve
[Unit]
Description=Server Monitoring Script
After=network.target

[Service]
ExecStart=/usr/bin/python3 /usr/local/bin/server_monitor.py
Restart=always
RestartSec=10
User=root

[Install]
WantedBy=multi-user.target
```

Komutları terminale yapıştırıp, ```ctrl + x``` ardından ```y``` basın, sonra ```enter```'e basıp kaydedip çıkın.


10- Servisi yükleme, etkinleştirme ve başlatma komutları.

```Sieve
sudo systemctl daemon-reload
```
```Sieve
sudo systemctl enable server_monitor.service
```
```Sieve
sudo systemctl start server_monitor.service
```

11- Scriptin durumunu kontrol etmek ve restart atmak için aşağıda ki komutları kullanın.

```Sieve
sudo systemctl status server_monitor.service
```
```Sieve
sudo systemctl restart server_monitor.service
```
