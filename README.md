#ilk projemiz..


# A-Star-Algorithm
Algorithm implementation and analysis for class project

## 🔍 Algoritmanın Ne Yaptığı
A* algoritması, en kısa yolu bulmak için kullanılan bir arama algoritmasıdır.

### 📂 Kategori
- Arama algoritmaları
- Grafik (graph) tabanlı algoritmalar

### ⚙️ Çözüm Yöntemi
- Heuristic (tahmin edici) + Gerçek mesafe = f(n) = g(n) + h(n)

---

## ⏱️ Ne Zaman ve Neden Kullanılır?
- Harita üzerinde yol bulma
- Oyunlarda yapay zekâ hareketi
- Robotik rota planlama

---

## 🧮 Karmaşıklık Analizi
- Zaman: O(b^d)
- Alan: O(b^d)
  - b: dallanma faktörü
  - d: çözümün derinliği

---

## 🪜 Adımlar
1. Başlangıç düğümünü open list’e ekle
2. En düşük `f(n)` değerine sahip düğümü seç
3. Komşularını değerlendir ve güncelle
4. Hedefe ulaşana kadar devam et

---

## 🎯 Kullanım Yerine Ait Örnek
- Başlangıç: (0, 0)
- Hedef: (4, 4)
- 5x5'lik grid üzerinde yol bulur.

---

## ✅ Avantajları
- En kısa yolu bulur
- Heuristik ile hızlandırılabilir

## ❌ Dezavantajları
- Bellek tüketimi yüksektir (çok düğüm saklar)

---

## 💻 Derleme ve Çalıştırma

```bash
g++ a-star.cpp Test.cpp -o program
./program

# Yapanlar:Furkan Danışık ve Yusuf Akın...
