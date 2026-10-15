# Deprem etkisinde devrilme önleyici dolap bağlantılarında vida tutma kapasitesi

*Screw-holding capacity of anti-tip cabinet restraints under seismic loading*

Bu depo, *Mobilya ve Ahşap Malzeme Araştırmaları Dergisi*'ne gönderilen makalenin analiz kodunu, girdi verilerini ve çıktılarını içerir. Çalışma, duvara bağlanmış boy dolaplarında devrilme önleyici bağlantının mobilya gövdesindeki vida–levha birleşiminde göçme olasılığını Monte Carlo benzetimiyle hesaplar.

## İçerik

```
├── data/
│   ├── README.md                                     veri kaynakları, sütunlar ve lisanslar
│   └── raw/
│       ├── gwdd_v2_wsg_estimates.csv                 GWDD v.2 odun yoğunluğu (20 332 satır)
│       └── vassiliou_barboutis_2005_face_withdrawal.csv  doğrulama deney verisi (27 satır)
├── src/
│   ├── models.py                                     kapasite ve talep eşitlikleri
│   └── run_analysis.py                               benzetim, çizelgeler ve şekiller
├── scripts/
│   └── download_gwdd.py                              GWDD verisini kaynağından yeniden üretir
├── tests/
│   └── test_models.py                                eşitlik kontrolleri
├── results/
│   ├── figures/                                      Şekil 1–7 (300 dpi PNG)
│   ├── tables/                                       makaledeki çizelgelerin CSV kaynakları
│   └── summary.json                                  metinde kullanılan özet sayılar
├── requirements.txt
├── CITATION.cff
└── LICENSE
```

## Çalıştırma

Python 3.11 veya üzeri gerekir (geliştirme ortamı: Python 3.13).

```bash
python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install -r requirements.txt
python src/run_analysis.py         # data/raw -> results (yaklaşık 15 saniye)
pytest                             # isteğe bağlı: eşitlik testleri
```

Farklı klasörler için: `python src/run_analysis.py --data data/raw --out results`.

Rastgele sayı üreteci sabit bir tohumla (20261015) başlatılır; aynı girdilerle aynı sonuçlar elde edilir.

## Yöntem özeti

| Adım | Açıklama | Kaynak |
| --- | --- | --- |
| Yoğunluk dönüşümü | Temel özgül ağırlıktan %12 rutubetteki özgül ağırlığa | Wood Handbook, Bölüm 4, Eş. 4-13 |
| Levha kapasitesi | Yonga levha ve MDF, yüz ve kenar vida sıyrılma | Eckelman (1975); Wood Handbook, Bölüm 8, Eş. 8-12 |
| Masif ahşap kapasitesi | Yan yüzey vida sıyrılma | Wood Handbook, Bölüm 8, Eş. 8-10a |
| Tek vida kapasitesi | min(sıyrılma × model belirsizliği, vida çeliği çekme kapasitesi) | — |
| Yatay ivme | a/g = 0.4 a_p S_DS (1 + 2z/H); 0.3 S_DS ≤ a/g ≤ 1.6 S_DS | ASCE/SEI 7-16, Eş. 13.3-1..3 |
| Bağlantı talebi | T = m (a z_cg − g d/2) / h_r | Ön alt kenar çevresinde moment dengesi |
| Göçme olasılığı | P_f = P(T > n C_1), 4 tipoloji × 6 bağlantı × 4 kat × 36 S_DS, hücre başına 40 000 örnek | — |

Varsayımlar (levha yoğunlukları, vida çeliği çekme kapasitesi, dolap boyut aralıkları, a_p dağılımı) `src/run_analysis.py` ve `src/models.py` içinde adlandırılmış sabitler olarak tanımlanmıştır.

## Çıktılar ve makale karşılıkları

| Dosya | Makalede |
| --- | --- |
| `results/figures/Sekil1_serbest_cisim.png` … `Sekil7_duyarlilik.png` | Şekil 1–7 |
| `results/tables/T2_validation_summary.csv` | Çizelge 4 |
| `results/tables/T3_turkish_species.csv` | Çizelge 5 |
| `results/tables/T4_Pf_n2_roof.csv` | Çizelge 6 |
| `results/tables/T5_min_screws.csv` | Şekil 6 |
| `results/tables/T6_sensitivity.csv` | Şekil 7 |
| `results/tables/T7_scenario_sensitivity.csv` | Çizelge 7 |
| `results/tables/T_demand_summary.csv` | Bölüm 3.3 |
| `results/tables/montecarlo_full.csv` | Bütün benzetim hücreleri (3 456 satır) |
| `results/tables/gwdd_species_capacity.csv` | 16 882 tür için tek vida kapasitesi |

## Veriler ve lisans

- Kod: MIT Lisansı (`LICENSE`).
- GWDD v.2 verisi: CC BY 4.0 — Fischer ve ark. (2026), https://doi.org/10.5281/zenodo.18262736
- Doğrulama verisi: Vassiliou ve Barboutis (2005), https://doi.org/10.1007/s10086-005-0708-9

Ayrıntılar için `data/README.md`.

## Atıf

Makale yayımlandığında künye buraya eklenecektir. Depoya atıf için `CITATION.cff` dosyasını kullanınız.
