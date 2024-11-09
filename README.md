
![hnn](https://github.com/user-attachments/assets/71179806-baa7-48e7-9d80-93654d8504c2)

<b> Bu script birden fazla humanode düğümü (node) çalıştıran ya da tek düğüm çalıştıran ancak tüm gün boyunca ilgilenemeyen kişiler içindir. Humanode son güncelleme sonrasında bazı değişikllere gitti. Humanode uygulamasına (launcher) uzun süre bakılmadığı taktirde ve bu süre içinde uzun süre kapalı konumda olan Humanode uygulaması yüzünden düğüm (node) çok fazla blok kaçırdığı için halı hazırda yapılmış olan selfie'yi sistem otomatik olarak siliyor. Bunun sonucunda 2 haftalık ödül alma şansı kaybediliyor. Bunun önüne geçmek için aşağıda ki scripti kullanabilirsiniz. Scripti kullanmadan önce Telegram üzerinden mesajların size gelmesi için bir bot oluşturmalısınız. 

Bu scriptin özelliğine gelecek olursak şöyle sıralayabiliriz;

- Humanode uygulaması kapanırsa telegram üzerinden size bilgilendirme mesajı gönderiyor.
- Humanode uygulamasını tekrar aktif yaptığınızda da sistem bunu görüp aktif olduğu konusunda telegram üzerinden mesaj gönderiyor.
- Script servis dosyası olarak çalıştığı için sunucuda ki herhangi bir sorundan dolayı sunucu kapanırsa ya da belli bir süre iletişim kuralamazsa bile sunucu aktif olduktan sonra script çalışmaya devam ediyor. </b>

------

Telegram Botu Oluşturma

1- Telegram arama yerine @BotFather yazın. Bota tıklayın ve başlata basın.

![Telegram bot 1](https://github.com/user-attachments/assets/0a7899d7-915b-4d6e-a01d-4ce0b743d4ab)

2- Menüye tıklayın /newbot yazan komuta basın. Botunuz için bir isim yazın ve entere basın. 


![Telegram bot 1](https://github.com/user-attachments/assets/1a0c882f-0387-4f6f-9e67-4690514a5bc0) ![Telegram bot 2](https://github.com/user-attachments/assets/de137183-b8cd-4e42-9aa8-8895a5c79788)

3- Bu kısımda botunuz için kullanıcı adı belirleyeceksiniz. Kullanıcı adınızın sonu bot ile bitmek zorunda.

![Telegram bot 3](https://github.com/user-attachments/assets/fabf269b-f33f-4992-8bb1-469ed3cac708)

4- Script için kullanacağınız bot token verecek. Bu kodu iyi saklayın başkalarıyla paylaşmayın. Botunuzun linkine tıklayıp, botunuzla sohbet başlatın. 

![Telegram bot 4](https://github.com/user-attachments/assets/d30ac842-6bd6-49ee-97d9-e6a6e7aed637) ![2](https://github.com/user-attachments/assets/c77c8df6-3413-4088-a215-a8464aa6993c)

5- https://api.telegram.org/bot{BOT_TOKEN}/getUpdates bu siteyi tarayıcınızda açın. {BOT_TOKEN} yazan yere bot tokeninizi yazacaksınız. Size bir çıktı verecek. Eğer çıktı çıkmazsa o zaman botunuza bir şeyler yazın ya da /start'a basın. Aşağıda ki resimde ki gibi çıktı vermesi gerekiyor. İşaretlediğim yerde CHAT_ID var onu da kaydedin. Bot için yapacaklarınız bu kadar. 

Örnek:
https://api.telegram.org/bot7477914640:AAE3dhSQeiMu46ddPUfyWhakYS8Pg8p15Kc/getUpdates

![5](https://github.com/user-attachments/assets/2bdd475f-f7c0-4751-9d51-8eaf1cbefc04)






