---
title: "Home Assistant Uzaktan Erişim"
date: 2024-08-08 17:34:00 +0300
description: "Cloudflare Tüneli ile Home Asistant'a uzaktan bağlanma."
categories: ["Akıllı Evler","Home Assistant","Cloudflared"]
tags: ["Akıllı Ev","Home Assistant","Cloudflare","Uzaktan Bağlanma","Remote"]

---

# Home Assistant'a Cloudflared Eklentisi Yükleme Adımları

1. Home Assistant'ınızda Supervisor bölümüne gidin.
2. Sol menüden "Add-on Store" seçeneğine tıklayın.
3. Sağ üst köşedeki üç nokta menüsüne tıklayın ve "Repositories" seçeneğini seçin.
4. Açılan pencerede şu adresi ekleyin:
```https://github.com/brenner-tobias/addon-cloudflared```

5. "Add" butonuna tıklayarak repository'yi ekleyin.
6. Add-on Store'a geri dönün ve sayfayı yenileyin.
7. Yeni eklenen repository'den "Cloudflared" eklentisini bulun.
8. Cloudflared eklentisine tıklayın ve "Install" butonuna basın.
9. Yükleme tamamlandıktan sonra, eklentinin konfigürasyon sayfasına gidin.
10. Cloudflare hesabınızdan aldığınız tunnel token'ı ilgili alana girin.[^1]
11. Diğer ayarları ihtiyacınıza göre yapılandırın.
12. Konfigürasyonu kaydedin.
13. Eklentinin ana sayfasına dönün ve "Start" butonuna tıklayarak eklentiyi başlatın.
14. Eklentinin loglarını kontrol ederek başarılı bir şekilde çalıştığından emin olun.

[^1]: Bu adımları uygulamadan önce, Cloudflare hesabınızda bir tunnel oluşturmuş ve gerekli token'ı almış olmanız gerekmektedir.