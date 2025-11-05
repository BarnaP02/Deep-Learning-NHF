# BirdCLEF 2024 Audio Classification - Milestone 1

## Csapat Információk

**Csapat neve:** [Your Team Name]

**Csapattagok:**
- [Pribék Barnabás] - [ETWSGU]
- [Csernák Gergő] - [TVEXOV]
- [Molnár Bence] - [JINVFB]

## Projekt Ismertetése

Ez a projekt a BirdCLEF 2024 versenyadatbázisát használja madárfajok azonosítására hangfelvételek alapján. A megoldás Keras és TensorFlow segítségével készült, mely az alábbi főbb lépéseket tartalmazza:

1. **Adatforrás**: BirdCLEF 2024 versenyadatbázis (182 madárfaj, ~24,459 hangfelvétel)
2. **Adatfeldolgozás**: OGG formátumú hangfájlok konvertálása mel-spektrogrammá
3. **Modell**: EfficientNetV2-B2 alapú képosztályozó (ImageNet előtanítással)
4. **Augmentáció**: MixUp, time-masking, frequency-masking technikák

### Adatbázis Letöltése

Az adatbázis a [BirdCLEF 2024 Kaggle versenyről](https://www.kaggle.com/competitions/birdclef-2024) tölthető le. A projekt Google Drive-ban tárolt adatokkal dolgozik a könnyebb hozzáférés érdekében.

## Repo Fájlok és Funkcióik

### `BirdCLEF24_NHF.ipynb`
Az elsődleges notebook, amely tartalmazza a teljes data pipeline-t:

**1. Adatforrás és letöltés:**
- Google Drive integráció a BirdCLEF 2024 adatbázis eléréséhez
- Adatbázis ellenőrzés és validálás
- Metaadat betöltés (`train_metadata.csv`)

**2. Adatok feltárása és vizualizáció:**
- Audio fájlok betöltése és lejátszása
- Hullámforma (waveform) megjelenítés
- Mel-spektrogram vizualizáció
- Fajok eloszlásának statisztikái

**3. Adatok előkészítése:**
- **Train/Validation/Test split**: 80/20 arányú felosztás
- **Audio preprocessing**:
  - 15 másodperces egységes hosszra vágás/kitöltés
  - 32,000 Hz mintavételezési frekvencia
  - Mel-spektrogram generálás (128x384 pixel)
- **Augmentáció** (csak training adatokon):
  - MixUp (α=0.4)
  - Time-masking (6-12%)
  - Frequency-masking (6-10%)
- **TensorFlow Dataset pipeline**: hatékony, batch-alapú adatbetöltés

**Végeredmény:**
- `train_ds`: Tanító adathalmaz (19,567 minta, augmentált)
- `valid_ds`: Validációs adathalmaz (4,892 minta, nem augmentált)
- Formátum: (128, 384, 3) méretű spektrogramok + one-hot kódolt címkék (182 osztály)

## Futtatási Útmutató

### Előfeltételek

1. **Google Colab fiók**
2. **Google Drive**: Az adatbázis feltöltve a következő útvonalon:
   ```
   My Drive/Deep_Learning_NHF/birdclef-2024/
   ├── train_audio/
   │   ├── asbfly/
   │   ├── ashdro1/
   │   └── ...
   └── train_metadata.csv
   ```

### Futtatás

1. **Nyissa meg a notebookot Google Colab-ban:**
   
   [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/[YOUR-USERNAME]/[YOUR-REPO]/blob/main/Untitled4%20(4).ipynb)

2. **Futtassa a cellákat sorrendben:**
   - **Import Libraries**: Függőségek telepítése
   - **Kaggle Setup and Data Download**: Google Drive csatlakoztatás
   - **Configuration**: Hiperparaméterek beállítása
   - **Load Class Names**: Osztályok inicializálása
   - **Meta Data**: Adatok betöltése és szűrése
   - **Data Split**: Train/validation felosztás
   - **Data Loaders**: TensorFlow Dataset pipeline létrehozása
   - **Visualization**: Batch megjelenítés és ellenőrzés

3. **Drive engedélyezés**: Az első futtatáskor engedélyezze a Google Drive hozzáférést

4. **Ellenőrzés**: A notebook végén megjelennek a mintaképek és spektrogramok

## Technikai Részletek

### Konfiguráció (CFG osztály)
```python
img_size = [128, 384]      # Spectrogram méret
batch_size = 64            # Batch méret
duration = 15              # Audio hossz (másodperc)
sample_rate = 32000        # Mintavételezés
epochs = 10                # Tanítási epoch-ok
preset = 'efficientnetv2_b2_imagenet'  # Model
```

### Használt Könyvtárak
- TensorFlow 2.19.0
- Keras 3.10.0
- KerasCV 0.9.0
- Librosa (audio feldolgozás)
- Pandas, NumPy (adatkezelés)
- Matplotlib (vizualizáció)

## Eredmények

- **Betöltött minták száma**: 24,459
- **Madárfajok száma**: 182
- **Tanító minták**: 19,567
- **Validációs minták**: 4,892
- **Spektrogram dimenziók**: 128 (frekvencia) × 384 (idő) × 3 (csatorna)

## Megjegyzések

- Az eredeti Kaggle notebook módosítva lett Google Drive támogatással
- Kompatibilitási hibák javítva a legújabb library verziókhoz
- Reprodukálhatóság: `seed = 42`
