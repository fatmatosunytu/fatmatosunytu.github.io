---
title: "NCAR/miles-credit: Yapay Zeka ile Hava Tahmini — Derinlemesine Analiz"
date: 2026-06-03
draft: false
tags: ["yapay-zeka", "iklim-bilimi", "nwp", "pytorch", "açık-kaynak", "biyoinformatik", "hugo-rehber"]
categories: ["repo-analiz"]
description: "NSF NCAR tarafından geliştirilen MILES CREDIT platformunun teknik analizi: AI destekli küresel hava ve iklim tahmin modelleri için açık kaynak araştırma altyapısı."
---

> **Repo:** [github.com/NCAR/miles-credit](https://github.com/NCAR/miles-credit)  
> **Yayın:** *npj Climate and Atmospheric Science*, 2025  
> **Lisans:** Apache-2.0  
> **Sürüm:** v2026.1.1 (aktif geliştirme)

---

## 📋 PROJE ANALİZİ

## Proje Nedir?

**MILES CREDIT** (*Community Research Earth Digital Intelligence Twin*), NSF Ulusal Atmosfer Araştırma Merkezi (NCAR) tarafından geliştirilen açık kaynaklı bir makine öğrenmesi platformu. Amacı tek ve net: geleneksel sayısal hava tahmin modellerinin (NWP) yerini alabilecek ya da onları destekleyebilecek **AI tabanlı küresel atmosfer modellerini** eğitmek, çalıştırmak ve değerlendirmek.

Projeyi özel kılan şey; sadece bir model değil, **tüm araştırma altyapısını** bir arada sunması. Veri işleme, model eğitimi, çıkarım (inference), ensemble belirsizlik analizi ve operasyonel gerçek zamanlı tahmin — hepsi tek bir entegre sistemde.

---

## Hangi Problemi Çözüyor?

Geleneksel NWP modelleri (ECMWF, GFS) çok güçlü ama aynı zamanda **son derece pahalı**: yüz binlerce CPU saati, petabayt veri, günler süren hesaplama. Son yıllarda Google DeepMind (GraphCast), NVIDIA (FourCastNet), Huawei (Pangu-Weather) gibi büyük oyuncular AI ile bu engeli aşmaya başladı.

Ama bu modeller kapalı veya kısmi açık kaynak. CREDIT ise:

- Tamamen açık kaynak (Apache-2.0)
- Birden fazla mimariyi destekleyen esnek yapı
- Fizik kısıtlı (mass, energy, water conservation)
- Peer-reviewed bilimsel makaleyle desteklenmiş

```bash
# Kurulum tek satır:
pip install miles-credit

# Hemen çalıştır:
credit ask "neden kayıp değerim düşmüyor?"
```

---

## Teknik Altyapı

### Desteklenen Model Mimarileri

| Model | Açıklama | Weights |
|-------|----------|---------|
| **WXFormer 6h** | Transformer tabanlı, küresel 6 saatlik tahmin | HuggingFace |
| **WXFormer 1h** | Yüksek çözünürlük, saatlik adım | HuggingFace |
| **FuXi 6h** | FuXi mimarisi implementasyonu | HuggingFace |
| **Swin Transformer** | Pencere dikkat mekanizmalı | Built-in |
| **UNet** | Encoder-decoder yapısı | Built-in |
| **Graph NN** | Graf tabanlı atmosfer modeli | Built-in |
| **Diffusion** | Ensemble için DDPM tabanlı | Built-in |

### Veri Akışı

```
ERA5 Zarr (Globus)          GFS/GEFS (Google Cloud)
       │                            │
       └──────────┬─────────────────┘
                  │
           PreBlock katmanı
        (normalize, regrid, log)
                  │
          PyTorch Model
       (DDP/FSDP multi-GPU)
                  │
          PostBlock katmanı
     (fizik fixers, ölçek geri)
                  │
        NetCDF çıktı / API
```

### Physics-Constrained Post-processing

Bu, projenin en güçlü teknik katkılarından biri. Model her tahmin adımında kütle, enerji ve su korunumunu bozmayacak şekilde **fizik düzeltici katmanlar** uygular:

```python
# config.yml içinde etkinleştirme
post_conf:
  global_mass_fixer: true
  global_water_fixer: true  
  global_energy_fixer: true
  tracer_fixer: true          # Negatif su içeriğini önler
```

### Ensemble Belirsizlik Analizi

Tek bir deterministic tahmin yerine, **olasılıksal tahmin** için üç farklı yöntem:

1. **Noise Injection** — girdi pertürbasyonu
2. **Bred Vectors** — atmosferik büyüme vektörleri
3. **SKEBS** — Stochastic Kinetic Energy Backscatter
4. **Diffusion Models** — DDPM tabanlı ensemble

---

## `credit ask` — Dahili AI Asistanı

Bu özellik benim için en ilgi çekici nokta. Platform, Claude (Anthropic) veya Groq/Gemini üzerinden çalışan **yerleşik bir AI hata ayıklama asistanı** içeriyor:

```bash
pip install "miles-credit[ask]"

export ANTHROPIC_API_KEY=sk-ant-...

# Ajan modu: dosyaları okur, komut çalıştırır, iteratif çözer
credit ask -c my_run.yml "eğitimim neden çöktü?"
credit ask "kayıp neden sallanıyor?"
```

Asistan config dosyanızı okuyabilir, hata loglarını analiz edebilir ve çözüm önerebilir. Bu, araştırma altyapısına AI asistanı entre etmenin güzel bir örneği.

---

## Dokümantasyon Kalitesi

ReadTheDocs üzerinde kapsamlı bir dokümantasyon mevcut:

| Kriter | Puan | Yorum |
|--------|------|-------|
| Kurulum rehberi | 9.5/10 | Derecho HPC + lokal kurulum detaylı |
| Quickstart | 9/10 | 10 dakikada çalışan model |
| API referansı | 9/10 | Otomatik üretilmiş, eksiksiz |
| Mimari açıklaması | 8/10 | Config kılavuzu çok iyi |
| Görseller | 7/10 | Daha fazla diyagram olabilir |
| Katkı rehberi | 8/10 | PR süreci tanımlanmış |

**Genel: 8.5/10** — Araştırma yazılımı standartlarında olağandışı iyi.

---

## Rakip Ekosistemi

```
Kapalı/Kısmi Açık          Tamamen Açık
━━━━━━━━━━━━━━━━━━          ━━━━━━━━━━━━━
GraphCast (Google)          CREDIT (NCAR)  ← Bu
Pangu-Weather (Huawei)      FourCastNet v1
Aurora (Microsoft)          
NeuralGCM (Google)          
```

CREDIT'in en büyük avantajı: **çoklu mimari desteği + fizik kısıtları + ensemble + AI asistan** kombinasyonu başka hiçbir açık kaynak projede bu kadar entegre değil.

---

## Güçlü ve Zayıf Yönler

**Güçlü:**
- Peer-reviewed bilimsel temel (npj Climate, 2025)
- Gerçek operasyonel kullanım kapasitesi (realtime GFS/GEFS)
- Olağanüstü modüler mimari (preblock/postblock)
- Built-in AI asistan

**Geliştirilmesi Gereken:**
- Bireysel araştırmacı için HPC bağımlılığı ağır
- Küçük örnek dataset yok (hızlı deneme için)
- Web arayüzü / dashboard eksik
- Topluluk büyüklüğü (85 ⭐) konuya göre küçük

---

## Kimler Kullanmalı?

- **Atmosfer / iklim araştırmacıları** — AI-NWP modellerini karşılaştırmak isteyenler
- **ML mühendisleri** — büyük ölçekli zaman serisi tahmin mimarileri öğrenmek isteyenler  
- **HPC kullanıcıları** — NCAR Derecho/Casper sistemlerine erişimi olanlar
- **Biyoinformatikçiler** — zamansal dizilimli fiziksel sistem modellemesi metodolojisi için (ilginç transfer!)

---

## Sonuç

CREDIT, "AI hava tahmini" söylemini gerçek bir araştırma altyapısına dönüştüren nadir projelerden. Büyük şirketlerin kapalı modellerine karşı bilim topluluğunun yanıtı. Henüz erken aşamada ama bilimsel ciddiyeti ve mühendislik kalitesiyle göz ardı edilemez.

```bash
# Hemen deneyin:
pip install miles-credit
credit quickstart
```

**Referans:** Schreck, J.S., Sha, Y., Chapman, W. et al. *Community Research Earth Digital Intelligence Twin: a scalable framework for AI-driven Earth System Modeling.* npj Clim Atmos Sci 8, 239 (2025).

---
