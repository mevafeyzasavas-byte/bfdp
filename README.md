# Beslenme ve Ders Programı - APK

Bu proje, `docs/index.html` sayfanızı bir Android uygulaması (APK) olarak
sunar. Uygulama sayfayı **internetten canlı olarak** yükler, yani
`docs/index.html`'i her güncellediğinizde APK'yı yeniden derlemenize
gerek KALMAZ — kullanıcılar uygulamayı her açtığında en güncel sürümü
otomatik görür.

## Nasıl çalışıyor?

1. `docs/index.html` → GitHub Pages ile yayınlanır (örn.
   `https://kullaniciadi.github.io/repo-adi/`).
2. APK içindeki tek ekran, bir WebView bileşenidir ve bu adresi açar.
3. Sayfayı güncellediğinizde (git push), GitHub Pages birkaç saniye
   içinde günceller; uygulamayı kapatıp yeniden açan (veya sayfayı
   aşağı çekip yenileyen) herkes yeni içeriği görür.
4. APK'yı yeniden derlemek SADECE `app/` klasöründeki Android kodunu
   (yani uygulamanın kendisini, tasarımını vs.) değiştirirseniz gerekir.

## Kurulum Adımları

### 1) Bu projeyi GitHub'a yükleyin
Bu klasörün tamamını (app/, docs/, .github/, build.gradle, vb.) yeni bir
GitHub reposuna push edin.

### 2) GitHub Pages'i açın
Repo → **Settings → Pages** → "Build and deployment" kısmında:
- Source: `Deploy from a branch`
- Branch: `main`, klasör: `/docs`
- Kaydedin. Birkaç dakika içinde adresiniz şu şekilde aktif olur:
  `https://KULLANICI_ADIN.github.io/REPO_ADIN/`

### 3) Uygulamanın adresini ayarlayın
`app/src/main/java/com/norhan/beslenme/MainActivity.java` içinde şu satırı
kendi adresinizle değiştirin:

```java
private static final String SITE_URL = "https://KULLANICI_ADIN.github.io/REPO_ADIN/";
```

Bu değişikliği yaptıktan sonra bir kez commit + push edin — bu, `app/`
klasörünü değiştirdiği için otomatik derleme (adım 4) tetiklenecektir.

### 4) İlk APK'yı otomatik aldırın
`.github/workflows/build-apk.yml` dosyası, `app/` klasöründe her
değişiklik push'landığında GitHub Actions üzerinde otomatik olarak:
- Debug APK derler,
- Onu bir **GitHub Release**'e ekler (Releases sekmesinde APK dosyasını
  indirebilirsiniz),
- Ayrıca Actions çalıştırmasının "Artifacts" kısmına da yükler.

Elle tetiklemek isterseniz: Repo → **Actions** → "Build APK" → **Run workflow**.

### 5) APK'yı telefona kurun
Releases sekmesinden `app-debug.apk`'yı indirip telefonunuzda açın
("Bilinmeyen kaynaklardan yükleme" iznini bir kez vermeniz gerekebilir).

## Önemli Not: İçerik güncellemesi ↔ APK güncellemesi farkı

| Ne değiştirdiniz? | APK'yı yeniden derlemek gerekir mi? |
|---|---|
| `docs/index.html` (beslenme/ders içeriği) | ❌ Hayır — sadece git push yeterli, uygulama otomatik günceli gösterir |
| `app/` klasöründeki Android kodu (tasarım, davranış, izinler) | ✅ Evet — push edince Actions otomatik yeni APK üretir |

## Not: İmzalama (signing)

Bu workflow **imzasız debug APK** üretir; bu APK doğrudan telefona
kurulabilir ama Play Store'a yüklenemez. Sadece kendi cihazınızda/aileniz
arasında kullanacaksanız bu yeterlidir. Play Store'a koymak isterseniz
ayrıca bir keystore oluşturup release imzalama adımı eklemek gerekir —
isterseniz bu kısmı da ayrıca hazırlayabilirim.

## Not: Çevrimdışı çalışma

Uygulama şu an internet bağlantısı gerektirir (sayfayı canlı çekiyor).
İnternetsizken de son görülen içeriği göstermesini isterseniz, WebView'e
basit bir "önbelleğe al ve bağlantı yoksa onu göster" mantığı
ekleyebilirim — isteğinize bağlı, ayrıca söyleyin.
