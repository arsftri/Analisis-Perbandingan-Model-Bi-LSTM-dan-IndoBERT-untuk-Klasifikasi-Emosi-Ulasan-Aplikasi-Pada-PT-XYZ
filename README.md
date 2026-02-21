# Analisis-Perbandingan-Model-Bi-LSTM-dan-IndoBERT-untuk-Klasifikasi-Emosi-Ulasan-Aplikasi-Pada-PT-XYZ

**## Dataset**: dikumpulkan melalui metode web scraping pada Google Play Store menggunakan ID aplikasi, bahasa Indonesia, SORTNEWST, dengan batasan waktu yaitu tahun 2017-2024.

**## Algoritma yang Digunakan**
**###🔷 Arsitektur Model Bi-LSTM**
**📌 1. Data Preprocessing**
- Text cleaning (lowercasing, removal of punctuation/symbols jika diperlukan)
- Tokenisasi teks
- Pembatasan kosakata maksimal 10.000 kata unik
- Padding sequence hingga maksimal 100 token
- Label encoding untuk 6 kelas emosi (0–5)

**📌 2. Arsitektur Model**
**-> Embedding Layer**
- Input dimension: 10.000 (vocabulary size)
- Output dimension: embedding vector (umumnya 100–300 dimensi)
- Input length: 100 token

**-> Bidirectional LSTM Layer**
- 32 units
- Bidirectional (forward & backward context learning)
- Activation: default tanh
- Return sequences: False

**-> Dropout Layer**
- Dropout rate: 0.5 Untuk mengurangi overfitting

**-> Dense Layer**
- Activation: ReLU Untuk feature transformation

**-> Output Layer**
- Dense (6 neurons)
- Activation: Softmax
- Multi-class classification (6 emotion labels)

**📌 3. Training Configuration** 
- Optimizer: Adam
- Loss Function: Categorical Crossentropy
- Batch size: 16
- Epoch: 5
- Data split: 70% training – 30% validation (stratified)

**###🔷 Arsitektur Model IndoBERT**
**📌 1. Data Preparation**
- Stratified train-validation split (70:30)
- Tokenisasi menggunakan Hugging Face IndoBERT Tokenizer
- Padding & truncation otomatis
- Maximum sequence length: mengikuti default tokenizer (umumnya 128)
- Attention mask digunakan untuk membedakan token asli & padding

**📌 2. Model Architecture**
- Pretrained Model: IndoBERT (Transformer-based)
- Base Architecture: BERT Encoder
  - Multi-head Self-Attention
  - Feed Forward Network
  - Layer Normalization
  - Positional Encoding
- Classification Head:
  - BertForSequenceClassification
  - Dropout layer (default BERT)
  - Fully connected layer (6 output neurons)
  - Activation: Softmax

**📌 3. Fine-Tuning Configuration**
- Optimizer: AdamW
- Learning rate: 3e-6
- Batch size: 16
- Epoch: 5
- Loss function: CrossEntropyLoss
- Evaluation per epoch (loss & prediction monitoring)

**###📊 Hasil & Evaluasi Model**
**🔷 1. Distribusi Emosi dalam Dataset**
- Total data: 1.230 ulasan
- Kategori emosi: Senang, Sedih, Marah, Takut, Netral, Muak
- Emosi paling dominan: Marah
- Emosi paling sedikit: Takut
- Dataset bersifat imbalanced, memengaruhi performa model pada kelas minoritas

**🔷 2. Hasil Model Bi-LSTM**
**📈 Performa Keseluruhan**
- Accuracy: 51%
- Macro F1-Score: 0.39
- Weighted F1-Score: 0.47

**🎯 Performa per Kelas**
- Senang → F1-Score: 0.77 (terbaik)
- Marah → F1-Score: 0.55
- Netral → F1-Score: 0.56
- Sedih → F1-Score: 0.27
- Muak → F1-Score: 0.16
- Takut → 0.00 (tidak terdeteksi)

**🔎 Insight**
- Model cukup baik mengenali emosi dengan ekspresi eksplisit (senang & marah).
- Kesulitan membedakan emosi negatif yang mirip secara semantik (sedih, muak, takut).
- Terindikasi overfitting ringan pada epoch akhir.
- Sensitif terhadap kelas mayoritas.

**🔷 3. Hasil Model IndoBERT**
**📈 Performa Keseluruhan**
- Accuracy: 60%
- Macro F1-Score: 0.48
- Weighted F1-Score: 0.58

**🎯 Performa per Kelas**
- Senang → F1-Score: 0.89 (tertinggi)
- Marah → F1-Score: 0.60
- Netral → F1-Score: 0.69
- Sedih → F1-Score: 0.41
- Muak → F1-Score: 0.31
- Takut → 0.00 (masih sulit terdeteksi)

**🔎 Insight**
- IndoBERT lebih stabil dan mampu menangkap konteks semantik Bahasa Indonesia.
- Lebih baik dalam membedakan ekspresi netral vs positif.
- Masih kesulitan pada kelas minoritas (takut).
- Transformer-based architecture terbukti lebih unggul dibanding RNN-based model.
