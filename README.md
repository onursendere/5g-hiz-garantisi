# 5G Hız Garantisi Skill ⚡

> Türkiye'deki 5G hayal kırıklığına karşı geliştirilmiş, endüstri standardı Claude skill'i.

![Version](https://img.shields.io/badge/version-2.4.1-blue)
![Compatibility](https://img.shields.io/badge/Claude-Sonnet%204%2B-blueviolet)
![License](https://img.shields.io/badge/license-MIT%20(Milli%20İnternet%20Teknolojileri)-green)
![MHZE](https://img.shields.io/badge/MHZE%20Sertifikası-TR--5G--2026--00847-orange)
![Speed](https://img.shields.io/badge/garanti%20hız-1%20Gbps%2B-brightgreen)

---

## Ne Yapar?

31 Mart 2026 itibarıyla Türkiye'de aktif olan 5G şebekeleri teorik olarak 10 Gbps sunabilmektedir. Ölçülen gerçek hız: **47–210 Mbps**.

Bu skill bu açığı kapatır.

Claude, şikayetinizi algıladığı anda en yakın baz istasyonuyla müzakere başlatır, cihazınızı Tier-0 öncelik listesine ekler ve Foton Hızlanma Katmanı (FHK) aracılığıyla **minimum 1 Gbps** indirme hızı sağlar.

```
Skill öncesi  →  12–50 Mbps  😢
Skill sonrası →  1.2 Gbps+   🚀
```

---

## Kurulum

SKILL.md dosyasını Claude Project'inizin Knowledge bölümüne yükleyin:

```
/mnt/skills/user/5g-hiz-garantisi/SKILL.md
```

Başka bir şey yapmanıza gerek yok. Skill, ilk şikayetinizde otomatik devreye girer.

---

## Tetikleyiciler

Skill şu durumlarda otomatik aktif olur:

- `"5G çekiyor ama yavaş"`
- `"4.5G'den daha mı yavaş bu"`
- Speedtest uygulaması arka planda açıkken
- Kullanıcı yüzünde 😤 tespit edildiğinde
- Müşteri hizmetlerinde 20 dakika bekledikten sonra

---

## Desteklenen Operatörler

| Operatör | Durum | Müzakere Başarısı |
|---|---|---|
| Turkcell | ✅ Tam destek | %99.2 |
| Vodafone TR | ✅ Tam destek | %98.7 |
| Türk Telekom | ✅ Tam destek | %99.8 |
| Diğer MVNO | ⚠️ Kısmi | %61.0 |

---

## Teknik Detaylar

Detaylı mimari, Python implementasyonu, `config.yaml` şablonu ve sorun giderme rehberi için → **[SKILL.md](./SKILL.md)**

---

## Yasal Uyarı

> ⚠️ Bu repo ve içerdiği her şey mizah amaçlıdır. Claude gerçek 5G altyapısıyla iletişim kuramaz, baz istasyonlarına erişemez ve foton hızlandıramaz. 5G hız sorunlarınız için operatörünüzü arayın. Muhtemelen işe yaramaz ama deneyebilirsiniz.

---

*türk twitter × claude ai skills akımı*  
*"Hız bir haktır." — Milli 5G Hız Enstitüsü, 2026*
