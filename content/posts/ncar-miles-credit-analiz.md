---
title: "NCAR/miles-credit: Yapay Zeka ile Hava Tahmini — Derinlemesine Analiz"
date: 2026-06-03
draft: false
tags: ["yapay-zeka", "iklim-bilimi", "nwp", "pytorch", "açık-kaynak", "biyoinformatik"]
categories: ["repo-analiz"]
description: "NSF NCAR tarafından geliştirilen MILES CREDIT platformunun teknik analizi: AI destekli küresel hava ve iklim tahmin modelleri için açık kaynak araştırma altyapısı."
---

> **Repo:** [github.com/NCAR/miles-credit](https://github.com/NCAR/miles-credit)  
> **Yayın:** *npj Climate and Atmospheric Science*, 2025  
> **Lisans:** Apache-2.0  
> **Sürüm:** v2026.1.1 (aktif geliştirme)

---

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
