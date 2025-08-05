# 🎓 Eğitim Asistanı - AI Destekli Öğrenme Platformu

Modern web teknolojileri ve Google Gemini AI ile güçlendirilmiş, kapsamlı eğitim asistanı platformu. Model Context Protocol (MCP) kullanarak gelişmiş AI araçları sunar.

## 🌟 Özellikler

### 🤖 AI Destekli Özellikler
- **PDF Özetleme**: Akademik belgeler, ders notları ve kitapları detaylı analiz
- **Video Özetleme**: YouTube videoları ve yerel video dosyalarını zaman damgalı özet
- **Ses Transkripsiyon**: Ses kayıtlarını yazıya çevirme ve analiz
- **Quiz Oluşturma**: Konuya özel test soruları ve cevap anahtarları
- **Web Araması**: Güncel bilgilerle desteklenmiş içerik

### 💎 Plan Sistemi
- **Free Plan**: Gemini 1.5 Pro, sınırsız mesaj, dosya yükleme, temel özellikler
- **Pro Plan**: Gemini 2.5 Pro, sınırsız mesaj, dosya yükleme, öncelikli destek

### 🎨 Modern Arayüz
- **Tailwind CSS** ile responsive tasarım
- **Glass morphism** efektleri
- **Real-time chat** WebSocket desteği
- **Drag & drop** dosya yükleme
- **Dark theme** gradient arka plan

### 🔐 Güvenlik & Kullanıcı Yönetimi
- Bcrypt şifre hashleme
- Session tabanlı kimlik doğrulama
- MySQL veritabanı entegrasyonu
- Dosya güvenliği ve temizleme

## 🏗️ Mimari

### Backend Teknolojileri
- **Flask**: Web framework
- **Flask-SocketIO**: Real-time iletişim
- **Google Gemini AI**: Yapay zeka modeli
- **FastMCP**: Model Context Protocol sunucusu
- **MySQL**: Veritabanı
- **PyMySQL**: Veritabanı bağlantısı

### Frontend Teknolojileri
- **Tailwind CSS**: Utility-first CSS framework
- **Font Awesome**: Icon kütüphanesi
- **Socket.IO**: Real-time client
- **Marked.js**: Markdown parser
- **Vanilla JavaScript**: Modern ES6+

### MCP (Model Context Protocol)
- **Standardize edilmiş AI araç çağrıları**
- **JSON tabanlı iletişim**
- **Asenkron işlem desteği**
- **Modüler araç sistemi**

## 📁 Proje Yapısı

```
eğitim-asistanı/
├── btkeduweb/                 # Web uygulaması
│   ├── templates/            # HTML şablonları
│   │   ├── base.html        # Ana şablon
│   │   ├── login.html       # Giriş sayfası
│   │   ├── register.html    # Kayıt sayfası
│   │   ├── chat.html        # Chat arayüzü
│   │   ├── chats.html       # Chat geçmişi
│   │   ├── pricing.html     # Plan sayfası
│   │   └── payment.html     # Ödeme sayfası
│   └── web_app.py           # Ana Flask uygulaması
├── edumcp/                   # MCP sunucusu
│   ├── app/
│   │   ├── tools/           # AI araçları
│   │   │   ├── pdf_summarizer.py    # PDF özetleme
│   │   │   ├── video_summarizer.py  # Video özetleme
│   │   │   ├── audio_transcriber.py # Ses transkripsiyon
│   │   │   └── quiz_generator.py    # Quiz oluşturma
│   │   ├── config.py        # Yapılandırma
│   │   ├── server.py        # MCP sunucusu
│   │   └── functions.py     # Yardımcı fonksiyonlar
│   └── main.py              # MCP başlatıcı
└── README.md                # Bu dosya
```

## 🚀 Kurulum

### 1. Gereksinimler
```bash
# Python 3.8+ gerekli
pip install flask flask-socketio
pip install google-generativeai
pip install fastmcp
pip install pymysql bcrypt
pip install python-dotenv
pip install yt-dlp requests
pip install anyio asyncio
```

### 2. Ortam Değişkenleri
`.env` dosyası oluşturun:
```env
# Gemini AI
GEMINI_API_KEY=your_gemini_api_key_here

# Veritabanı
DB_HOST=localhost
DB_USER=your_db_user
DB_PASSWORD=your_db_password
DB_NAME=education_assistant

# Flask
SECRET_KEY=your_secret_key_here

# MCP
MCP_URL=http://localhost:8000

# Google Search (Opsiyonel)
GOOGLE_API_KEY=your_google_api_key
GOOGLE_CSE_ID=your_custom_search_engine_id
```

### 3. Veritabanı Kurulumu
```sql
-- Kullanıcılar tablosu
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    email VARCHAR(100) NOT NULL,
    full_name VARCHAR(100) NOT NULL,
    role ENUM('user', 'admin') DEFAULT 'user',
    plan_type ENUM('free', 'pro') DEFAULT 'free',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    last_login TIMESTAMP NULL,
    is_active BOOLEAN DEFAULT TRUE
);

-- Chat oturumları
CREATE TABLE chat_sessions (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    session_id VARCHAR(255) UNIQUE NOT NULL,
    session_name VARCHAR(255) DEFAULT 'Yeni Chat',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    last_activity TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    is_active BOOLEAN DEFAULT TRUE,
    FOREIGN KEY (user_id) REFERENCES users(id)
);

-- Chat mesajları
CREATE TABLE chat_messages (
    id INT AUTO_INCREMENT PRIMARY KEY,
    session_id VARCHAR(255) NOT NULL,
    user_id INT NOT NULL,
    message_type ENUM('user', 'assistant') NOT NULL,
    content TEXT NOT NULL,
    file_path VARCHAR(500) NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id)
);

-- Yüklenen dosyalar
CREATE TABLE uploaded_files (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    filename VARCHAR(255) NOT NULL,
    original_filename VARCHAR(255) NOT NULL,
    file_path VARCHAR(500) NOT NULL,
    file_type ENUM('PDF', 'AUDIO', 'VIDEO', 'TEXT', 'DOCUMENT') NOT NULL,
    file_size BIGINT NOT NULL,
    uploaded_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    is_active BOOLEAN DEFAULT TRUE,
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```

### 4. Klasör Yapısı
```bash
# Dosya yükleme klasörü oluşturun
mkdir -p C:\mcpler\education_mcp\shared_uploads
```

## 🎯 Çalıştırma

### 1. MCP Sunucusunu Başlatın
```bash
cd edumcp
python main.py
```

### 2. Web Uygulamasını Başlatın
```bash
cd btkeduweb
python web_app.py
```

### 3. Tarayıcıda Açın
```
http://localhost:5000
```

## 🛠️ AI Araçları

### 📄 PDF Özetleme
```python
# Kullanım örneği
pdf_ozetle(
    pdf_dosyasi_yolu="document.pdf",
    ozet_tipi="kapsamli",  # kisa, genis, kapsamli
    hedef_dil="Türkçe"
)
```

**Özellikler:**
- Sayfa özetleri ve zaman damgaları
- Tablolar ve grafikler analizi
- Bahsedilen kaynaklar
- Anahtar kelimeler ve etiketler
- Öğrenme çıktıları

### 🎥 Video Özetleme
```python
# YouTube video
kullanıcı: https://youtube.com/watch?v=... videosunu kapsamlı özetle. 
model: tool için gerekli parametreleri json ile yollar ve "bu videoyu kapsamlı özetledim ......"
videoyu_ozetle(
    video_url="https://youtube.com/watch?v=...",
    ozet_tipi="kapsamli",
    hedef_dil="otomatik"
)
kullanıcı: dosya yükler ve "bu videoyu kısa özetle"
# Yerel dosya
tool dosya yoluyla beraber gerekli parametreleri json ile yollar ve "bu videonun kısaca özeti......"
videoyu_ozetle(
    video_dosyasi_yolu="video.mp4",
    ozet_tipi="genis"
)
```

**Özellikler:**
- YouTube video indirme
- Zaman damgalı özet
- Görsel materyal analizi
- Bahsedilen kaynaklar

### 🎵 Ses Transkripsiyon
```python
ses_dosyasini_transkript_et(
    ses_dosyasi_yolu="audio.mp3",
    cikti_tipi="ozet",  # transkript, ozet
    hedef_dil="Türkçe"
)
```

**Özellikler:**
- Çoklu format desteği (MP3, WAV, M4A, etc.)
- Konuşmacı ayrımı
- Ses kalitesi değerlendirmesi
- Zaman damgalı transkript

### 📝 Quiz Oluşturma
```python
soru_olustur(
    konu="Osmanlı Tarihi",
    soru_sayisi=10,
    zorluk="orta",  # kolay, orta, zor, karisik
    soru_tipi="karisik",  # test, acik_uclu, dogru_yanlis
    web_arama=True
)
```

**Özellikler:**
- Çoktan seçmeli, açık uçlu, doğru/yanlış sorular
- Web araması ile güncel bilgiler
- Detaylı cevap anahtarları
- Zorluk seviyesi analizi

## 🎨 Arayüz Özellikleri

### Modern Tasarım
- **Glass morphism** efektleri
- **Gradient** arka planlar
- **Hover animasyonları**
- **Loading** durumları
- **Responsive** tasarım

### Chat Arayüzü
- **Real-time** mesajlaşma
- **Typing** göstergeleri
- **File upload** drag & drop
- **Markdown** desteği
- **Chat geçmişi**

### Kullanıcı Deneyimi
- **Auto-resize** textarea
- **Quick actions** butonları
- **Tool status** göstergeleri
- **Error handling**
- **Success notifications**

## 🔧 Yapılandırma

### Plan Yönetimi
```python
# Free plan özellikleri
- Gemini 1.5 Pro model
- Sınırsız mesaj
- Dosya yükleme
- Temel AI asistanı
- Chat geçmişi

# Pro plan özellikleri  
- Gemini 2.5 Pro model
- Sınırsız mesaj
- Dosya yükleme
- Öncelikli destek
- Gelişmiş analitikler
```

### Dosya Desteği
```python
# Desteklenen formatlar
PDF: .pdf
Audio: .mp3, .wav, .m4a, .aac, .ogg, .webm
Video: .mp4, .avi, .mov, .mkv, .webm
Text: .txt, .doc, .docx
```

## 🚀 Deployment

### Production Ayarları
```python
# web_app.py
if __name__ == '__main__':
    socketio.run(app, 
                debug=False,  # Production'da False
                host='0.0.0.0', 
                port=5000)
```

### Nginx Yapılandırması
```nginx
server {
    listen 80;
    server_name your-domain.com;
    
    location / {
        proxy_pass http://127.0.0.1:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
    
    location /socket.io/ {
        proxy_pass http://127.0.0.1:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}
```

## 🤝 Katkıda Bulunma

1. Fork yapın
2. Feature branch oluşturun (`git checkout -b feature/amazing-feature`)
3. Commit yapın (`git commit -m 'Add amazing feature'`)
4. Push yapın (`git push origin feature/amazing-feature`)
5. Pull Request açın

## 📝 Lisans

Bu proje MIT lisansı altında lisanslanmıştır.

## 🆘 Destek

Sorularınız için:
- GitHub Issues
- Email: support@egitim-asistani.com

## 🙏 Teşekkürler

- **Google Gemini AI** - Yapay zeka modeli
- **FastMCP** - Model Context Protocol
- **Tailwind CSS** - UI framework
- **Flask** - Web framework

---

**Eğitim Asistanı** - AI ile öğrenmenin geleceği 🚀