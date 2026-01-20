# RSS to Telegram Bot

Bu bot, Yeni Bakış Haber RSS beslemesinden otomatik olarak Telegram kanalınıza haber gönderir. Thumbnail görsellerle birlikte güzel formatlı mesajlar paylaşır.

## 🎯 Özellikler

- ✅ RSS'den otomatik haber çekme
- ✅ Her haberle birlikte thumbnail görseli
- ✅ Güzel formatlanmış mesajlar (HTML)
- ✅ Kategori bazlı emojiler
- ✅ Yinelenen paylaşımları önleme
- ✅ 10 dakikada bir otomatik kontrol
- ✅ Rate limit koruması
- ✅ Görsel optimizasyonu

## 📋 Kurulum Adımları

### 1. Telegram Bot Oluşturma

1. Telegram'da [@BotFather](https://t.me/BotFather) ile konuşma başlatın
2. `/newbot` komutunu gönderin
3. Bot için bir isim seçin (örn: "Yeni Bakış Haber Bot")
4. Bot için bir kullanıcı adı seçin (örn: "yenibakishaber_bot")
5. BotFather size bir **bot token** verecek. Bunu kaydedin!

### 2. Chat ID Bulma

**Kanal için:**
1. Botunuzu kanalınıza admin olarak ekleyin
2. Kanala bir mesaj gönderin
3. Şu URL'yi ziyaret edin: `https://api.telegram.org/bot<BOT_TOKEN>/getUpdates`
   (BOT_TOKEN yerine kendi tokenınızı yazın)
4. Yanıtta `"chat":{"id":-100XXXXXXXXX}` şeklinde chat ID'nizi bulacaksınız
5. Chat ID'yi kaydedin (örn: `-1001234567890`)

**Özel grup için:**
1. Botunuzu gruba admin olarak ekleyin
2. Gruba bir mesaj gönderin ve yukarıdaki adımları tekrarlayın

**Kendinize göndermek için:**
1. [@userinfobot](https://t.me/userinfobot) ile konuşma başlatın
2. Bot size chat ID'nizi verecek (örn: `123456789`)

### 3. GitHub Repository Kurulumu

1. Bu repository'yi fork edin veya yeni bir repository oluşturun
2. Dosyaları repository'nize ekleyin:
   - `post_to_telegram.py`
   - `.github/workflows/rss-to-telegram.yml`

### 4. GitHub Secrets Ayarlama

Repository'nizde **Settings** → **Secrets and variables** → **Actions** → **New repository secret**:

1. **TELEGRAM_BOT_TOKEN**
   - Value: BotFather'dan aldığınız token (örn: `123456789:ABCdefGHIjklMNOpqrsTUVwxyz`)

2. **TELEGRAM_CHAT_ID**
   - Value: Kanal/Grup/Kişi chat ID'niz (örn: `-1001234567890` veya `123456789`)

### 5. Workflow'u Etkinleştirme

1. Repository'de **Actions** sekmesine gidin
2. Workflow'u enable edin
3. İlk çalışma için **Run workflow** butonuna tıklayın

## 🚀 Kullanım

Bot otomatik olarak:
- Her 10 dakikada bir RSS beslemesini kontrol eder
- Yeni haberleri bulur
- Thumbnail ile birlikte Telegram'a gönderir
- Gönderilen haberleri `posted_links.json` dosyasında saklar

## 🎨 Mesaj Formatı

```
📰 Yeni Haber

[Haber Başlığı]

📂 Kategori

🔗 Devamı için tıklayın
```

## 📸 Thumbnail Kaynakları

Bot sırasıyla şu kaynaklardan thumbnail arar:
1. RSS enclosure (feed'deki görsel)
2. Open Graph image (og:image meta tag)
3. Twitter Card image
4. WordPress featured image
5. İçerikteki ilk büyük görsel

## ⚙️ Özelleştirme

### RSS URL Değiştirme
`rss-to-telegram.yml` dosyasında:
```yaml
env:
  RSS_URL: https://www.yenibakishaber.com/rss  # Buraya istediğiniz RSS URL'i yazın
```

### Kontrol Sıklığını Değiştirme
`rss-to-telegram.yml` dosyasında:
```yaml
schedule:
  - cron: "*/10 * * * *"  # Her 10 dakika (değiştirebilirsiniz)
```

Örnekler:
- `*/5 * * * *` - Her 5 dakika
- `*/15 * * * *` - Her 15 dakika
- `*/30 * * * *` - Her 30 dakika
- `0 * * * *` - Her saat başı

### Kategori Emojileri Özelleştirme
`post_to_telegram.py` dosyasında `category_emojis` sözlüğünü düzenleyin.

### Mesaj Sınırını Değiştirme
`post_to_telegram.py` dosyasında:
```python
MAX_ENTRIES_TO_PROCESS = 10  # Bir seferde kaç haber işlensin
```

## 🔍 Sorun Giderme

### "Unauthorized" Hatası
- Bot token'ınızı kontrol edin
- BotFather'dan yeni bir token alın

### "Chat not found" Hatası
- Chat ID'nin doğru olduğundan emin olun
- Botu kanala/gruba admin olarak ekleyin

### "Message is too long" Hatası
- Haber başlıkları çok uzunsa bot otomatik kısaltır
- Gerekirse `create_beautiful_post` fonksiyonunu düzenleyin

### Görsel Gönderilmiyor
- RSS'de görsel var mı kontrol edin
- İnternet bağlantısını kontrol edin
- Telegram'ın görsel boyut sınırına (10MB) dikkat edin

### Bot Tekrar Ediyor
- `posted_links.json` dosyası commit edilmeli
- GitHub Actions'ın dosya yazma izni olmalı

## 📝 Log Kontrol

1. GitHub repository'de **Actions** sekmesine gidin
2. Son workflow run'ına tıklayın
3. "post-to-telegram" job'ına tıklayın
4. Detaylı logları görün

## 🤝 Katkıda Bulunma

Pull request'ler memnuniyetle karşılanır!

## 📄 Lisans

MIT License - Özgürce kullanabilirsiniz!

---

**Not:** Bot ilk çalıştığında RSS'deki son 10 haberi kontrol eder ama sadece yeni olanları gönderir. Daha önce gönderilmiş haberler tekrar gönderilmez.
