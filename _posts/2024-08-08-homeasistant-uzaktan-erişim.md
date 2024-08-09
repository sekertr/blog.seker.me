---
title: "Home Assistant Uzaktan Erişim"
date: 2024-08-08 17:34:00 +0300
description: "Cloudflare Tüneli ile Home Asistant'a uzaktan bağlanma."
categories: ["Akıllı Evler","Home Assistant","Cloudflared"]
tags: ["Akıllı Ev","Home Assistant","Cloudflare","Uzaktan Bağlanma","Remote"]

---

# Home Assistant'a Cloudflared Eklentisi Yükleme Adımları

- Home Assistant'ınızda Supervisor bölümüne gidin.
- Sol menüden "Add-on Store" seçeneğine tıklayın.
- Sağ üst köşedeki üç nokta menüsüne tıklayın ve "Repositories" seçeneğini seçin.
- Açılan pencerede şu adresi ekleyin:
```bash
https://github.com/brenner-tobias/addon-cloudflared
```

- "Add" butonuna tıklayarak repository'yi ekleyin.
- Add-on Store'a geri dönün ve sayfayı yenileyin.
- Yeni eklenen repository'den "Cloudflared" eklentisini bulun.
- Cloudflared eklentisine tıklayın ve "Install" butonuna basın.
- Yükleme tamamlandıktan sonra, eklentinin konfigürasyon sayfasına gidin.
- Cloudflare hesabınızdan aldığınız tunnel token'ı ilgili alana girin.[^1]
- Diğer ayarları ihtiyacınıza göre yapılandırın.
- Konfigürasyonu kaydedin.
- Eklentinin ana sayfasına dönün ve "Start" butonuna tıklayarak eklentiyi başlatın.
- Eklentinin loglarını kontrol ederek başarılı bir şekilde çalıştığından emin olun.

[^1]: Bu adımları uygulamadan önce, Cloudflare hesabınızda bir tunnel oluşturmuş ve gerekli token'ı almış olmanız gerekmektedir.