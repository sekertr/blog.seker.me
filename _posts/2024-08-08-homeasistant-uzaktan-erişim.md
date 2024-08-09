---
title: "Home Assistant Uzaktan Erişim"
date: 2024-08-08 17:34:00 +0300
description: "Cloudflare Tüneli ile Home Asistant'a uzaktan bağlanma."
categories: ["Akıllı Evler","Home Assistant","Cloudflared"]
tags: ["Akıllı Ev","Home Assistant","Cloudflare","Uzaktan Bağlanma","Remote"]

---
Kendi özel domain adresinizi kullanarak, Home Assistant’a Cloudflare Tunnel bağlantısı üzerinden bağlanabilir ve ağınızdaki tüm cihazlara uzaktan erişim sağlayabilirsiniz.

## Cloudflare Tunnel nedir?

Cloudflare Tunnel, güvenli ve kolay bir şekilde uzaktan erişim sağlamak için kullanılan güçlü bir araçtır. Geleneksel port yönlendirme veya VPN yapılandırmalarına gerek kalmadan, internet üzerindeki herhangi bir cihazdan Home Assistant gibi yerel ağınızdaki servislere erişmenizi sağlar. Cloudflare’in dünya çapındaki ağ altyapısı üzerinde çalışan bu çözüm, yalnızca istenilen servise erişim sağlarken, dış tehditlere karşı güvenliğinizi de artırır. Hem yeni başlayanlar hem de deneyimli kullanıcılar için Cloudflare Tunnel, uzaktan erişim sorunlarını çözmek için ideal bir seçenektir.

## Gereksinimler
- Özel bir domain adresi
- Cloudflare hesabı
- Home Asistant için cloudflared eklentisi

# Uzaktan Erişim İçin Yapılandırma Adımları:

1. **Domain Adresi Edinme:**  
  İlk olarak bir domain adresi edinin. Örnek olarak "domainadresiniz.com" kullanacağız.
2. **Cloudflare Hesabı Oluşturma:**
  [Cloudflare](https://www.cloudflare.com/)'den ücretsiz bir hesap açın ve e-posta doğrulamasını gerçekleştirin. Hesap açtıktan sonra web sitesi ekle diyerek domain adresinizi girin ve ücretsiz planı seçin.
3. **Nameserver Değişikliği:**
  Cloudflare, domain adresinizin nameserver'larını kendi sunucularına taşımanızı isteyecektir. Domain sağlayıcınıza giriş yaparak bu değişikliği yapın. Değişiklik yaklaşık 30 dakika sürebilir. İşlem tamamlandığında Cloudflare size bir bildirim gönderecektir.
4. **Home Assistant Yapılandırması**
  Home Assistant'a girin ve **configuration.yaml** dosyasına aşağıdaki satırları ekleyin:

```bash
http:
use_x_forwarded_for: true
trusted_proxies:
- 172.30.33.0/24
```
- Ardından **Geliştirici araçlarına** gidiniz ve **yapılandırmayı kontrol ediniz**.
- Bir hata ile karşılaşmadıysanız Home Assistant'ı **yeniden başlatınız**.
- Home Assistant yeniden başlayınca **Ayarlar > Eklentiler > Eklenti Mağazası** yolunu izleyiniz ve sağ üstte bulunan üç noktaya tıklayıp **Depolar**'ı seçiniz.
- Aşağıdaki adresi yazıp **Ekle** butonuna basınız:

```bash
https://github.com/brenner-tobias/ha-addons
```
- **Eklenti Mağazas**ı'na geri dönün ve sayfayı yenileyin.
- **Cloudflared** eklentisini aratınız.
- Cloudflared eklentisine tıklayın ve **Yükle** butonuna basın.
- Yükleme tamamlandıktan sonra, eklentinin **Yapılandırma** sayfasına gidin.
- Cloudflare hesabınıza girdiğiniz domain adınızı **External Home Assistant Hostname** kısmına giriniz.[^1]
- Eğer ağdaki diğer cihazlara da erişmek istiyorsanız Additional Hosts kısmına aşağıdaki şekilde bilgileri giriniz, sadece Home Assistant için yapılandırıyorsanız boş bırakabilirsiniz:

```bash
## Bu koddaki örnekleri kendi ağınıza göre düzenleyiniz.
- hostname: modem.domainadresiniz.com # Modem'e uzaktan erişmek istediğiniz adres
  service: http://192.168.0.1 # Modem Yerel IP
- hostname: adh.domainadresiniz.com # Adguard Home uzak adres
  service: http://192.168.0.200:5353 # Adguard Home IP:Port
- hostname: watch.domainadresiniz.com
  service: http://192.168.0.201:8088
- hostname: pass.domainadresiniz.com
  service: http://192.168.0.200:7277
- hostname: tv.domainadresiniz.com
  service: http://192.168.0.203
- hostname: nginx.domainadresiniz.com
  service: http://192.168.0.200:81
```
- **Cloudflare Tunnel Name**'e bir ad veriniz. 
- Yapılandırmayı kaydedin.
- Eklentinin ana sayfasına dönün ve **Start** butonuna tıklayarak eklentiyi başlatın.
- Eklentinin **Günlük** kısmına geliniz ve **Yenile**'ye basınız.
- Bir adet web adresine bağlanmanızı isteyecek, o adrese tarayıcıdan giriniz ve onay veriniz.
- Yaklaşık 1 dk sonra tünelin başarılı ile kurulması gerekir. Tünel kurulduktan sonra girmiş olduğunuz adreslerden herhangi bir ağdan bağlanabilirsiniz.

[^1]: Bu adımları uygulamadan önce, Cloudflare hesabınızda nameserverların aktif olduğundan ve e-posta doğrulaması yaptığınızdan emin olunuz.