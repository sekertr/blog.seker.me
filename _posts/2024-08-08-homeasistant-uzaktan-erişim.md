---
title: "Home Assistant Uzaktan Erişim"
date: 2024-08-08 17:34:00 +0300
description: "Cloudflare Tüneli ile Home Asistant'a uzaktan bağlanma."
categories: ["Akıllı Evler","Home Assistant","Cloudflared"]
tags: ["Akıllı Ev","Home Assistant","Cloudflare","Uzaktan Bağlanma","Remote"]

---
Kendi özel domain adresinizi kullanarak, Home Assistant’a Cloudflare Tunnel bağlantısı üzerinden bağlanabilir ve ağınızdaki tüm cihazlara <ins>CGNAT havuzunda olsanız dahi</ins> uzaktan erişim sağlayabilirsiniz.  
## Cloudflare Tunnel nedir?  
Cloudflare Tunnel, güvenli ve kolay bir şekilde uzaktan erişim sağlamak için kullanılan güçlü bir araçtır. Geleneksel port yönlendirme veya VPN yapılandırmalarına gerek kalmadan, internet üzerindeki herhangi bir cihazdan Home Assistant gibi yerel ağınızdaki servislere erişmenizi sağlar. Cloudflare’in dünya çapındaki ağ altyapısı üzerinde çalışan bu çözüm, yalnızca istenilen servise erişim sağlarken, dış tehditlere karşı güvenliğinizi de artırır. Hem yeni başlayanlar hem de deneyimli kullanıcılar için Cloudflare Tunnel, uzaktan erişim sorunlarını çözmek için ideal bir seçenektir.  
* * *
## Gereksinimler  
- Özel bir domain adresi  
- Cloudflare hesabı  
- Home Asistant için cloudflared eklentisi  

* * *
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
## Başka Uzaktan Erişim Türleri Var Mı?
Home Assistant'a uzaktan erişim sağlamak için Cloudflare Tünel dışında da birkaç yöntem bulunmaktadır:
 
### Home Asistant Uzaktan Erişim Türleri:
1. **Home Assistant Cloud**:  
    Home Assistant kurucusu [Nabu Casa](https://www.nabucasa.com/) firması tarafından sağlanan ücretli bu hizmet, kullanıcıların Home Assistant'a kolayca ve güvenli bir şekilde uzaktan erişim sağlamasını mümkün kılar. Bu yöntem, kullanıcıların karmaşık ağ yapılandırmalarına girmeden hızlıca bağlantı kurmalarını sağlar. Ayrıca, Google Assistant ve Amazon Alexa sesli komut entegrasyonlarını da destekler.
2. **Sabit IP**:  
    Evinizdeki internet hizmet sağlayıcınızdan belli bir ücret karşılığında sabit bir IP adresi talep ederek, Home Assistant'a doğrudan erişim sağlayabilirsiniz. Sabit IP kullanarak Home Assistant'ı internete açabilir, modeminizden 8123 portunu Home Asistant'ın IP adresine yönlendirme yaparak dışarıdan erişimi mümkün kılabilirsiniz. Ancak, bu yöntemde güvenlik önlemlerini almak çok önemlidir; VPN veya güçlü şifreleme kullanımı önerilir.
3. **DNS Servisleri**:  
    Dinamik DNS (DDNS) servisleri, sabit bir IP adresine sahip olmayan kullanıcılar için iyi bir alternatiftir. Bu servisler, evinizdeki cihazlara atanmış dinamik IP adresini sürekli güncelleyerek, sabit bir alan adı üzerinden erişim sağlar. DuckDNS gibi hizmetler, Home Assistant'a uzaktan erişim için kullanılabilir. Bu servislerin çalışması için IP adresinizin CGNAT Havuzunda olmaması lazım.
4. **Modem çözümleri**:  
    Bazı modemler, kendi içinde yerleşik bir bulut hizmeti barındırabilir. Bu DNS özelliğini kullanarak, uzaktan Home Assistant'a güvenli bir şekilde erişim sağlayabilirsiniz.

5. **Cloudflare Tunnel**

* * * 
## Kıyaslama

| Yöntem                | Kolaylık   | Güvenlik | Diğer                                     |
| :-------------------- | :--------: | :------: | --------------------------------------- : |
| Cloudflare Tunnel     | Zor        | Yüksek   | Sadece domain maliyeti                    |
| Home Assistant Cloud  | Çok Kolay  | Yüksek   | Aylık yüksek maliyet                      |
| Sabit IP              | Kolay      | Düşük    | Aylık maliyet                             |
| DNS Servisleri        | Orta       | Orta     | Maliyetsiz fakat CGNAT havuzunda çalışmaz |
| Modem Çözümleri       | Kolay      | Yüksek   | Tek seferlik donanım maliyeti             |

* * * 