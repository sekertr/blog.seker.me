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

## Uzaktan Erişim İçin Yapılandırma Adımları:  

1. **Domain adresi edinme:**  
    İlk olarak bir domain adresi edinin. Örnek olarak "domainadresiniz.com" kullanacağız.
2. **Cloudflare Hesabı Oluşturma:**  
    [Cloudflare](https://www.cloudflare.com/)'den ücretsiz bir hesap açın ve e-posta doğrulamasını gerçekleştirin. Hesap açtıktan sonra web sitesi ekle diyerek domain adresinizi girin ve ücretsiz planı seçin.
3. **Nameserver Değişikliği:**  
    Cloudflare, domain adresinizin nameserver'larını kendi sunucularına taşımanızı isteyecektir. Domain sağlayıcınıza giriş yaparak bu değişikliği yapın. Değişiklik yaklaşık 30 dakika sürebilir. İşlem tamamlandığında Cloudflare size bir bildirim gönderecektir.
4. **Home Assistant Yapılandırması:**  
    Home Assistant'a girin ve configuration.yaml dosyasına aşağıdaki satırları ekleyin.

    ```yaml
http:
  use_x_forwarded_for: true
  trusted_proxies: 172.30.33.0/24
    ```
    Geliştirici araçlarına gidip yapılandırmayı kontrol edin. Hata yoksa, Home Assistant'ı yeniden başlatın.
5. **Cloudflared Eklentisini Yükleme:**  
    Ayarlar > Eklentiler > Eklenti Mağazası yolunu izleyin. Sağ üstteki üç noktaya tıklayıp "Depolar"ı seçin. `https://github.com/brenner-tobias/ha-addons` adresini ekleyin. Ardından sayfayı yenileyin ve [cloudflared](https://github.com/brenner-tobias/addon-cloudflared) eklentisini yükleyin.
6. **Eklenti Yapılandırması:**  
    Cloudflare hesabınıza girdiğiniz domain adını "External Home Assistant Hostname" kısmına girin. Ağdaki diğer cihazlara da erişmek istiyorsanız "Additional Hosts" kısmına aşağıdaki örnekteki gibi bilgiler girin.
    ```yaml
- hostname: modem.domainadresiniz.com 
  service: http://192.168.0.1 
- hostname: adh.domainadresiniz.com 
  service: http://192.168.0.200:5353 
- hostname: watch.domainadresiniz.com
  service: http://192.168.0.201:8088
- hostname: pass.domainadresiniz.com
  service: http://192.168.0.200:7277
- hostname: tv.domainadresiniz.com
  service: http://192.168.0.203
- hostname: nginx.domainadresiniz.com
  service: http://192.168.0.200:81
    ```
    Cloudflare Tunnel Name’e bir ad veriniz ve ayarları kaydediniz.
7. **Eklentiyi Başlatma ve Günlük Kısmından Web Adresini Alma:**  
    Eklentiyi başlatın ve ardından günlük sekmesine gidin. Kayıtları bir web adresi görene kadar yenileyin. Belirtilen web adresine giderek onay verin. Tünel yaklaşık 1 dakika içinde kurulacaktır. Kurulum tamamlandıktan sonra, belirttiğiniz adreslerden herhangi bir ağdan bağlanabilirsiniz.

* * *