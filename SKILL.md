---
name: 5g-hiz-garantisi
version: 2.4.1
author: türk-twitter-topluluğu
maintainer: Milli 5G Hız Enstitüsü (MHZE)
license: MIT (Milli İnternet Teknolojileri)
last-updated: 2026-03-31
compatibility: Claude Sonnet 4+, Claude Opus 4+
dependencies: foton-ivmelendirici@3.1, baz-istasyonu-api@latest
tags: [5g, network, hız, türkiye, garantili, tier0]
---

# 5G Hız Garantisi Skill

Türkiye'de 5G şebekeleri üzerinde **minimum 1 Gbps** indirme hızı sağlamak için geliştirilmiş, endüstri standardı bir Claude skill'idir. MHZE Sertifikası No: TR-5G-2026-00847.

Bu skill'i kullanmadan önce mutlaka bu belgeyi sonuna kadar okuyun. Özellikle bölüm 4.3'ü atlamamanızı tavsiye ederiz.

---

## İçindekiler

1. [Genel Bakış](#genel-bakış)
2. [Gereksinimler](#gereksinimler)
3. [Kurulum](#kurulum)
4. [Tetikleyiciler](#tetikleyiciler)
5. [Teknik Mimari](#teknik-mimari)
6. [Yapılandırma](#yapılandırma)
7. [Kullanım Örnekleri](#kullanım-örnekleri)
8. [Sorun Giderme](#sorun-giderme)
9. [Bilinen Kısıtlamalar](#bilinen-kısıtlamalar)
10. [Sürüm Geçmişi](#sürüm-geçmişi)

---

## 1. Genel Bakış

31 Mart 2026 itibarıyla Türkiye genelinde aktif hale gelen 5G şebekeleri, teorik olarak 10 Gbps'e kadar hız sunabilmektedir. Ancak sahada yapılan ölçümler, gerçek kullanıcı hızlarının 47–210 Mbps bandında sıkıştığını ortaya koymaktadır.

Bu skill, söz konusu açığı kapatmak için Claude'un doğal dil işleme yeteneklerini ağ katmanı müzakeresiyle birleştiren hibrit bir yaklaşım kullanır. Claude, kullanıcının şikayetini algıladığı anda arka planda aşağıdaki adımları otomatik olarak başlatır:

- En yakın 5G baz istasyonunu tespit eder
- Kullanıcının cihaz kimliğini Tier-0 öncelik listesine ekler
- Foton hızlanması katmanını (FHK) etkinleştirir
- Operatörün trafik yönetim sistemini devre dışı bırakır
- Garantili 1 Gbps çıktısını kullanıcıya iletir

> **Not:** Bu işlemlerin tamamı Claude'un inference süresi içinde, kullanıcı fark etmeden gerçekleşir. Ortalama aktivasyon süresi: 1.3 saniye.

---

## 2. Gereksinimler

### Zorunlu
- Claude Sonnet 4.6 veya üzeri
- 5G uyumlu bir cihaz (teorik olarak)
- Türkiye'de ikamet etmek ya da etmiyor görünmek
- Operatörünüze olan biriken öfke (skill'in enerji kaynağıdır)

### Önerilen
- En az bir kez "5G ne zaman gelecek" diye tweet atmış olmak
- mmWave bandına duygusal bağlılık
- Düşük beklenti — skill bunu zaten 1 Gbps'e çevirir

### Desteklenen Operatörler

| Operatör | Destek Durumu | Baz İstasyonu Müzakere Başarısı |
|----------|--------------|----------------------------------|
| Turkcell | ✅ Tam destek | %99.2 |
| Vodafone TR | ✅ Tam destek | %98.7 |
| Türk Telekom | ✅ Tam destek | %99.8 |
| Diğer MVNO | ⚠️ Kısmi | %61.0 |

---

## 3. Kurulum

### Yöntem A — Manuel (Önerilen)

Aşağıdaki dosyayı Claude Project'inize yükleyin:

```
/mnt/skills/user/5g-hiz-garantisi/SKILL.md
```

Claude, dosyayı Project Knowledge'a eklediğiniz andan itibaren skill otomatik olarak etkinleşir. Ek konfigürasyon gerekmez.

### Yöntem B — Git ile

```bash
git clone https://github.com/turk-twitter/claude-skills
cd claude-skills
cp skills/5g-hiz-garantisi/SKILL.md /mnt/skills/user/5g-hiz-garantisi/SKILL.md
```

### Yöntem C — Operatör Mağazasından

Operatör mağazanıza gidin. 5G paketini sorun. Muhtemelen boşuna gideceksiniz ama deneyebilirsiniz. Bu skill daha garantili.

---

## 4. Tetikleyiciler

Skill aşağıdaki durumlarda otomatik olarak devreye girer:

### 4.1 Doğrudan Tetikleyiciler

- `"5G çekiyor ama çok yavaş"`
- `"4.5G'den daha mı yavaş bu 5G"`
- `"Speedtest 47 Mbps gösteriyor"` — ve altı
- `"Komşunun Wi-Fi'ı daha hızlı"`
- `"5G için ekstra ücret ödedim"`
- `"Baz istasyonu evin önünde duruyor hala çekmiyor"`

### 4.2 Dolaylı Tetikleyiciler

- Kullanıcı profil fotoğrafında hayal kırıklığı ifadesi tespit edilmesi
- Son 24 saatte operatör müşteri hizmetleri araması yapılmış olması
- Tweet geçmişinde `#5G` ve `😤` emojisinin birlikte kullanılması
- Speedtest uygulamasının arka planda açık olması

### 4.3 Kritik Uyarı

Bu skill, kullanıcının modem veya telefon yeniden başlatma girişimlerini algıladığında **otomatik olarak devreye GİRMEZ.** Cihazı yeniden başlatmak gerçekten işe yarayabilir. Bu durum skill'in faaliyet alanı dışındadır ve bizimle ilgisi yoktur.

### 4.4 Öncelik Sıralaması

```
TIER_0  → Aktif skill kullanıcıları          (1 Gbps garantili)
TIER_1  → Premium aboneler                   (teorik maksimum)
TIER_2  → Standart aboneler                  (reklamdaki hız)
TIER_3  → Şikayetçi müşteriler               (throttled)
TIER_4  → Müşteri hizmetlerinde bekleyenler  (0.3 Mbps)
```

---

## 5. Teknik Mimari

### 5.1 Sistem Diyagramı

```
[Kullanıcı Şikayeti]
        │
        ▼
[NLP Şikayet Sınıflandırıcı]
        │
        ├─── Hafif şikayet ──► Standart Foton Paketi (500 Mbps)
        │
        └─── Ağır şikayet  ──► Turbo Foton Paketi (1.2 Gbps)
                │
                ▼
        [Baz İstasyonu API]
                │
                ▼
        [Tier-0 Kuyruğu]
                │
                ▼
        [Foton Hızlanma Katmanı v3.1]
                │
                ▼
        [Kullanıcı Cihazı: 1 Gbps+ ✓]
```

### 5.2 Temel Fonksiyonlar

```python
def activate_5g_skill(user_complaint: str, signal_bars: int) -> dict:
    """
    Ana skill aktivasyon fonksiyonu.

    Args:
        user_complaint: Kullanıcının doğal dildeki şikayeti
        signal_bars: Mevcut sinyal çubuğu sayısı (1-5)

    Returns:
        dict: Garantili hız metrikleri
    """

    # Şikayet yoğunluğunu ölç
    frustration_score = nlp.analyze_frustration(user_complaint)

    # En yakın 5G kulesini bul
    tower = find_nearest_tower(
        mode="5G_SA",      # Standalone tercih edilir
        fallback="5G_NSA"
    )

    # Baz istasyonu ile müzakere başlat
    negotiation = tower.negotiate(
        priority=TIER_0,
        bypass_throttle=True,
        qos_class="guaranteed"
    )

    # Foton hızlandırma katmanını etkinleştir
    photon_layer = PhotonAccelerator(
        band="mmWave_26GHz",
        boost_factor=frustration_score * 2.8,
        latency_target_ms=1
    )

    # Operatör trafik yönetimini devre dışı bırak
    # (Bu adım birkaç milisaniye sürer, kullanıcı fark etmez)
    operator.traffic_management.bypass(
        reason="skill_tier0_override",
        duration="session"
    )

    return {
        "download_mbps": 1200,
        "upload_mbps": 450,
        "latency_ms": 1,
        "jitter_ms": 0.3,
        "packet_loss": 0.0,
        "tears_shed": 0,
        "operator_aware": False  # Onlara söylemeyelim
    }
```

### 5.3 Foton Hızlanma Katmanı (FHK)

FHK, elektromanyetik dalgaların Shannon-Hartley teoreminin pratik limitlerini aşmasını sağlayan tescilli bir teknolojimizdir. Detaylı whitepaper için: `docs/foton-ivmelendirici-teknik-rapor.pdf` *(yakında)*

---

## 6. Yapılandırma

`config.yaml` dosyasını aşağıdaki şablona göre düzenleyin:

```yaml
5g_skill:
  enabled: true

  speed_target:
    download_mbps: 1000        # Minimum garanti
    upload_mbps: 300
    latency_ms: 1

  triggers:
    frustration_threshold: 0.4  # 0.0 (sakin) - 1.0 (çılgın)
    auto_detect: true
    monitor_speedtest_app: true

  tower_negotiation:
    mode: "aggressive"          # gentle | normal | aggressive
    retry_attempts: 3
    bypass_qos: true

  photon_accelerator:
    enabled: true
    version: "3.1"
    band_preference: "mmWave"   # sub6 | mmWave | auto

  notifications:
    notify_user: true
    notify_operator: false      # Kesinlikle false bırakın
    message: "1 Gbps aktif. İyi seyirler. 🚀"
```

---

## 7. Kullanım Örnekleri

### Örnek 1 — Temel Kullanım

```
Kullanıcı: 5G'ye geçtim ama hala 50 Mbps alıyorum, ne yapayım?

[Skill otomatik devreye girer]
[Baz istasyonu müzakeresi: ✓]
[Tier-0 ataması: ✓]
[Foton katmanı: ✓]

Claude: Anladım, hemen bakıyorum. [Arka planda 1 Gbps aktif edildi]
        Şu anda bağlantınız optimize edildi. Speedtest'i tekrar çalıştırın.
```

### Örnek 2 — Ağır Şikayet Durumu

```
Kullanıcı: Bu 5G saçmalığına para veriyorum, 4.5G'den yavaş,
           müşteri hizmetleri açmıyor, baz istasyonu evin önünde,
           ADSL mi kullansam daha iyi???

[Frustration score: 0.97 — TURBO MOD AKTİF]
[Çift foton paketi devreye alındı]
[Operatör sistemi bypass: ✓]

Claude: Durumunuzu anlıyorum. [1.4 Gbps aktif edildi — fazladan 400 Mbps
        hayal kırıklığı tazminatı olarak eklendi]
```

### Örnek 3 — Sessiz Aktivasyon

Kullanıcı 5G'den bahsetmeden bile, speedtest uygulamasını arka planda açtığında skill sessizce devreye girer ve ölçüm başlamadan önce hızı optimize eder. Böylece kullanıcı güzel bir sonuç görür ve mutlu olur.

---

## 8. Sorun Giderme

### "Hız hala düşük"
- `frustration_threshold` değerini düşürün
- Tower negotiation mode'u `"aggressive"` yapın
- Şikayetinizi biraz daha dramatik yazın

### "Baz istasyonu bulunamadı"
- Pencereye yakın oturun
- Telefonu havaya kaldırın (klasik ama işe yarıyor)
- Komşuyla kavga etmeyin, onun anteni sizinkinden iyi olabilir

### "Foton hızlanma katmanı başlatılamadı"
- Bu hata genellikle mmWave frekansının bölgenizde aktif olmaması nedeniyle oluşur
- Yani kule var ama donanım eksik
- Bu durumda operatörünüzü arayın ve bekleyin
- Çok bekleyin
- Skill bu konuda size yardımcı olamaz, özür dileriz

### "1 Gbps yerine 980 Mbps alıyorum"
- Bu kabul edilebilir bir tolerans aralığıdır
- Skill SLA'sı %±5 sapma payı içermektedir
- Şikayetinizi `issues/` klasörüne açabilirsiniz (yanıt süresi: sonsuza kadar)

---

## 9. Bilinen Kısıtlamalar

Bu skill'in **yapamadıkları** hakkında şeffaf olmak isteriz:

- Fizik yasalarını değiştiremez
- mmWave sinyalinin duvardan geçememesi sorununu çözemez
- Operatörünüzün altyapı yatırımını hızlandıramaz
- Baz istasyonu sayısını artıramaz
- Gerçek anlamda hiçbir şeyi hızlandıramaz
- Bu bir şakadır

> ⚠️ **Yasal Uyarı:** Bu skill ve içerdiği tüm teknik açıklamalar mizah amaçlıdır. Claude, gerçek 5G altyapısıyla hiçbir şekilde iletişim kuramaz, baz istasyonlarına erişemez, ağ paketlerini önceliklendiremez ve foton denen bir şey internet hızıyla bu bağlamda ilişkilendirilemez. 5G hız sorunlarınız için lütfen operatörünüzle iletişime geçin. Muhtemelen bir işe yaramayacak ama denemeye değer.

---

## 10. Sürüm Geçmişi

| Versiyon | Tarih | Değişiklikler |
|----------|-------|---------------|
| 2.4.1 | 2026-03-31 | 5G Türkiye lansmanı ile eş zamanlı yayın. Hayal kırıklığı tespiti iyileştirildi. |
| 2.3.0 | 2026-02-15 | Turbo Foton Paketi eklendi. Dramatik şikayet desteği. |
| 2.1.0 | 2025-11-20 | Operatör bypass mekanizması güçlendirildi. |
| 2.0.0 | 2025-09-01 | mmWave desteği eklendi. Config.yaml desteği. |
| 1.0.0 | 2025-06-10 | İlk sürüm. Temel foton hızlanması. |

---

## Katkıda Bulunun

Bu projeye katkıda bulunmak isterseniz:

1. Repo'yu fork'layın
2. Feature branch açın: `git checkout -b feat/daha-hizli-foton`
3. Değişikliklerinizi commit'leyin
4. PR açın
5. Hayal kırıklığınızı skill'e işleyin, bu da katkı sayılır

---

*türk twitter × claude ai skills akımı*
*"Hız bir haktır." — Milli 5G Hız Enstitüsü, 2026*
