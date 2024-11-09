![Ekran görüntüsü 2024-11-08 213042](https://github.com/user-attachments/assets/2d6ad775-bc70-4790-8130-70ccfa6851f2)

Humanode uygulamasını kontrol için bu scripti'de kurmalısınız. Bu script sadece sunucuları takip ediyor. Bu script diğer scripte göre tek bir sunucuya kurmanız yeterlidir. Yani sunucu ile bağlantı kurulamazsa ya da sunucu kapanırsa size mesaj gönderiyor telegram üzerinden. Bu scriptin önemi şöyle büyük, eğer sunucu kapanırsa ya da sunucu ile iletişim kesilirse o zaman <a href="https://github.com/Lorento34/humanode/blob/main/Humanode%20Uygulama%20Kontrol.md"><b>```Humanode Uygulama Kontrol scripti```</b></a> çalışmıyor. Bu sefer bu script devreye giriyor ve size mesaj gönderiyor telegram üzerinden. Eğer siz sunucuda ki problemi hemen hızlı bir şekilde çözebiliyorsanız Humanode uygulamasını kontrol etmeniz için size zaman kazandıracaktır.

Bu script için Telegram botunu kullanmanız gerekiyor. Ancak <a href="https://github.com/Lorento34/humanode/blob/main/Humanode%20Uygulama%20Kontrol.md"><b>```Humanode Uygulama Kontrol scripti```</b></a> için oluşturduğunuz Telegram botunu kullanabilirsiniz.



# Humanode Sunucu Kontrol Script Kurulumu

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
import time
import subprocess

BOT_TOKEN = "7558360014:AAEJ2aL56leL5g3PMQ0V_n8WgWeu5PSyVP4"
CHAT_ID = "1571936947"
CHECK_INTERVAL = 60  # Kontrol aralığı (saniye cinsinden)
RETRY_INTERVAL = 1200  # Bağlantı yoksa her 20 dakikada bir mesaj gönderme
PING_ATTEMPTS = 3  # Her sunucuya kaç kez ping atılacak

# İzlenecek sunucuların IP adresleri (yanlış IP adreslerini buradan kaldırın)
SERVER_IPS = [
	"149.102.146.63", "84.247.182.185", "100.42.185.170", "89.147.103.82", "149.102.131.11",
    "109.199.99.150", "185.182.187.103", "84.46.242.207", "62.146.226.48", "38.242.131.75",
    "149.102.142.164", "100.42.190.21", "38.242.231.233", "213.199.56.209", "185.202.239.66",
	"83.171.249.166", "84.46.245.29", "62.146.228.112", "62.146.226.46", "100.42.185.171",
    "100.42.185.167", "100.42.190.13", "62.146.226.44", "109.199.99.148", "154.12.227.191",
    "62.84.180.64", "62.146.226.47", "195.26.253.8", "157.173.101.10", "213.199.56.247",
	"66.94.119.68", "195.26.253.10", "213.199.56.202", "213.199.56.118", "149.102.157.55",
	"37.60.231.254", "38.242.216.210", "100.42.186.183", "100.42.186.178", "194.163.142.71",
    "149.102.131.59", "194.163.170.53", "149.102.131.144", "100.42.181.142", "37.60.235.4",
    "62.84.180.60", "62.84.180.58", "162.84.180.52", "62.84.180.46", "62.84.180.63",
	"638.242.244.189", "149.102.138.150", "185.215.166.5", "217.76.52.165", "38.242.251.103",
    "31.220.80.195", "158.220.115.1", "184.174.33.20", "38.242.136.153", "38.242.215.69",
	"157.173.101.9", "157.173.101.2", "157.173.101.4", "157.173.101.5", "157.173.101.21",
	"157.173.101.22", "157.173.101.23", "157.173.101.24", "157.173.101.26", "157.173.101.17",
    "1157.173.101.20", "157.173.100.255", "157.173.101.0", "62.84.180.55", "109.199.99.153",
    "62.84.180.46", "62.84.180.63", "638.242.244.189", "149.102.138.150", "185.215.166.5"
    
]

# Her sunucunun bağlantı durumunu ve son mesaj gönderim zamanını saklayan sözlük
server_status = {ip: {"down": False, "last_alert_time": 0} for ip in SERVER_IPS}

def send_telegram_message(message):
    url = f"https://api.telegram.org/bot{BOT_TOKEN}/sendMessage"
    data = {"chat_id": 1571936947, "text": message}
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
        if result.returncode == 0:  # Ping başarılıysa
            successful_pings += 1
    return successful_pings > 0  # En az bir ping başarılıysa bağlantı var sayılır

def check_server_status():
    current_time = time.time()  # Şu anki zamanı al
    for ip in SERVER_IPS:
        try:
            is_online = ping_server(ip)  # Sunucuya ping atarak bağlantı durumunu kontrol et
            if not is_online:  # Sunucuya ulaşılamıyorsa
                if not server_status[ip]["down"] or (current_time - server_status[ip]["last_alert_time"] >= RETRY_INTERVAL):
                    # İlk kez kapalı duruma geçtiyse veya son uyarıdan sonra 20 dakika geçtiyse
                    print(f"{ip} sunucusuna ulaşılamıyor, Telegram'a mesaj gönderiliyor...")
                    send_telegram_message(f"⚠️ {ip} sunucusuna ulaşılamıyor!")
                    server_status[ip]["down"] = True  # Sunucunun kapalı olduğunu işaretle
                    server_status[ip]["last_alert_time"] = current_time  # Son uyarı zamanını güncelle
            else:  # Sunucu tekrar çevrimiçi hale geldiyse
                if server_status[ip]["down"]:
                    print(f"{ip} sunucusu yeniden bağlandı.")
                    send_telegram_message(f"🔄 {ip} sunucusu online oldu.")
                    server_status[ip]["down"] = False  # Bağlantı sağlandığında durumu sıfırla
                    server_status[ip]["last_alert_time"] = 0  # Son uyarı zamanını sıfırla
        except Exception as e:
            print(f"{ip} sunucu durumu kontrol edilirken hata oluştu: {e}")

# İzlemeye başlama mesajı gönder
send_telegram_message("🖥️ Sunucu izleme başlatıldı...")

while True:
    check_server_status()
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
