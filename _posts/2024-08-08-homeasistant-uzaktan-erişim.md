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
4. **Home Assistant Yapılandırması:**  
  Home Assistant'a girin ve configuration.yaml dosyasına aşağıdaki satırları ekleyin, ardından geliştirici araçlarına gidip yapılandırmayı kontrol edin. Hata yoksa, Home Assistant'ı yeniden başlatın:

  ```yaml
  http:
  use_x_forwarded_for: true
  trusted_proxies:
  - 172.30.33.0/24
  ```   
5. **Cloudflared Eklentisini Yükleme:**  
Ayarlar > Eklentiler > Eklenti Mağazası yolunu izleyin. Sağ üstteki üç noktaya tıklayıp "Depolar"ı seçin. Aşağıdaki adresi ekleyin:  
`https://github.com/brenner-tobias/ha-addons`  
Sayfayı yenileyin ve Cloudflared eklentisini yükleyin.
6. **Eklenti Yapılandırması:**
  Cloudflare hesabınıza girdiğiniz domain adını "External Home Assistant Hostname" kısmına girin. Ağdaki diğer cihazlara da erişmek istiyorsanız "Additional Hosts" kısmına aşağıdaki bilgileri girin ve Cloudflare Tunnel Name’e bir ad veriniz:  

```yaml
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
7. **Tünel Kurulumu:**  
Eklentiyi başlatın ve günlük kısmında belirtilen web adresine giderek onay verin. Tünel yaklaşık 1 dakika içinde kurulacaktır. Kurulum tamamlandıktan sonra, belirttiğiniz adreslerden herhangi bir ağdan bağlanabilirsiniz.
* * *
