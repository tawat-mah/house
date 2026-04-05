# house
5 Domains Hackathon 
# 🏠 House Image Classification — EfficientNet-B3
**Super AI Engineer Season 6 — House Image Classification Task**

**ผู้จัดทำ:** ธวัช มหิทธิ (Tawat Mah) — รหัส 607745  
**GitHub:** [tawat-mah](https://github.com/tawat-mah)

---

## 📌 รายละเอียดโปรเจกต์

จำแนกภาพบ้านออกเป็น 2 คลาส โดยใช้ **EfficientNet-B3** (Transfer Learning) ด้วย 5-Fold Cross Validation และ Test Time Augmentation (TTA)

| Class | ความหมาย |
|-------|----------|
| `0` | ไม่เห็นตัวบ้าน (เห็นแค่กำแพง / ต้นไม้ / รั้ว) |
| `1` | เห็นตัวบ้านชัดเจน |

---

## 🗂️ โครงสร้างการทำงาน

| Sector | หัวข้อ | รายละเอียด |
|--------|--------|------------|
| Sector 1 | Setup & Mount Drive | ติดตั้ง Library, Mount Drive, Set Config & Seed |
| Sector 2 | EDA & Data Verification | โหลด CSV, ดู Class Balance, Plot ตัวอย่างภาพ |
| Sector 3 | Dataset & Augmentation | สร้าง HouseDataset, Augmentation, DataLoader |
| Sector 4 | Model Building | โหลด EfficientNet-B3, เปลี่ยน Classifier Head |
| Sector 5 | Training (5-Fold CV) | AdamW + Cosine Scheduler, Save Checkpoint |
| Sector 6 | Inference & Submission | โหลด Checkpoint ทุก Fold, TTA, Export CSV |

---

## 🛠️ เครื่องมือที่ใช้

```
Python 3
PyTorch 2.10.0 (CUDA)
timm (EfficientNet-B3)
albumentations
pandas / numpy
matplotlib / seaborn
Google Colab (GPU: Tesla T4)
```

---

## 📊 ข้อมูล Dataset

| รายละเอียด | ค่า |
|-----------|-----|
| Train rows | 2,953 รูป |
| Test rows | 1,550 รูป |
| ขนาดรูป | 640 × 640 pixels |
| Class 0 | 1,520 รูป (51.5%) |
| Class 1 | 1,433 รูป (48.5%) |
| Missing files | 0 ✅ |

---

## 🤖 Model Architecture

- **Base Model:** EfficientNet-B3 (10.7M parameters)
- **Classifier Head:** Dropout → Linear(2)
- **Input Size:** 224 × 224
- **Output:** 2 classes

---

## ⚙️ Training Config

| Parameter | ค่า |
|-----------|-----|
| Optimizer | AdamW |
| Scheduler | CosineAnnealingLR |
| Batch Size | 32 |
| Folds | 5-Fold Stratified CV |
| Seed | 42 |
| GPU | Tesla T4 |

---

## 🔄 Data Augmentation

**Train (8 steps):**
- Horizontal/Vertical Flip
- Random Crop
- Rotate
- Color Jitter
- Normalize

**Validation (3 steps):**
- Resize → Normalize เท่านั้น

---

## ✅ ผลลัพธ์

| Fold | Accuracy |
|------|----------|
| Fold 1 | ~95% |
| Fold 2 | ~95% |
| Fold 3 | ~95% |
| Fold 4 | ~95% |
| **Fold 5 (Best)** | **95.93%** |
| **OOF Accuracy** | **95.02%** ✅ |

---

## 📂 ไฟล์ในโปรเจกต์

| ไฟล์ | คำอธิบาย |
|------|----------|
| `607745_ธวัช.ipynb` | Jupyter Notebook หลัก |
| `train.csv` | ข้อมูล Label สำหรับ Train |
| `submission.csv` | ผลลัพธ์ที่ใช้ส่ง (1,550 rows) |
| `fold_1.pth` ~ `fold_5.pth` | Checkpoint ของแต่ละ Fold (~41 MB/ไฟล์) |

---

## 🚀 วิธีรัน

1. เปิดไฟล์ใน [Google Colab](https://colab.research.google.com/) และเลือก **Runtime → T4 GPU**
2. Mount Google Drive และวางข้อมูลใน folder:
   ```
   MyDrive/Colab Notebooks/super-ai-engineer-ss-6-house-classification/
   ```
3. รัน Cell ตามลำดับ Sector 1 → Sector 6
4. ดาวน์โหลด `submission.csv` จาก Sector 6

