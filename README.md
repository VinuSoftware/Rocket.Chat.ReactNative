# Vinu Chat — proje notları

Bu dosya, bu depodaki Vinu Chat değişikliklerini ve dağıtım öncesi saklanması gereken bilgileri özetler. Gizli anahtarların, parolaların veya sertifika içeriklerinin kendisini depoya yazmayın.

## Mevcut yapılandırma

| Alan | Değer / dosya |
|---|---|
| Uygulama adı | Vinu Chat |
| Rocket.Chat adresi | `https://chat.vinu.com.tr` |
| Android application ID | `com.vinu.chat` |
| Android Firebase projesi | `vinu-rocketchat` |
| Android Firebase dosyası | `android/app/google-services.json` |
| Android ilk versionCode | `1` |
| iOS ana uygulama bundle ID | `com.vinu.rocketchat` |
| iOS Firebase projesi | `vinu-rocketchat` |
| iOS Firebase dosyası | `ios/GoogleService-Info.plist` |
| iOS Share Extension | `com.vinu.rocketchat.ShareExtension` |
| iOS Notification Service | `com.vinu.rocketchat.NotificationService` |
| iOS App Group | `group.com.vinu.rocketchat` |

## Neler değiştirildi

- Android uygulama adı Vinu Chat yapıldı; Android kimliği `com.vinu.chat` olarak ayarlandı.
- Android Firebase yapılandırması Vinu'nun `vinu-rocketchat` projesine bağlandı.
- Android Fastlane paket kimliği Vinu uygulama kimliğine uyarlandı.
- Android `sharedUserId` kaldırıldı.
- İlk açılıştaki sunucu adresi `https://chat.vinu.com.tr` olarak dolduruluyor. Kullanıcı yine başka sunucu adresi girebilir.
- iOS ana uygulama, Share Extension ve Notification Service bundle ID'leri Vinu için ayarlandı.
- iOS App Group ve Keychain grup adları Vinu kimlikleriyle eşleştirildi.
- iOS Firebase yapılandırması Vinu'nun `vinu-rocketchat` projesine bağlandı.
- Apple hesabının provisioning profile'ında bulunmadığı için Communication Notifications entitlement'ı iki entitlement dosyasından çıkarıldı. Standart push bildirimleri için bu özel capability gerekli değildir.
- Xcode proje dosyasında Watch uygulaması hedefi kaldırılmış görünüyor; Vinu için Watch uygulaması planlanmadığından amaçla uyumlu.


## Firebase dosyaları

Uygulama derlemesi bu iki dosyayı bekler; Firebase'i kullandığımız sürece **yerlerini değiştirmeyin veya silmeyin**: (içeriği bitwarden'a kayıtlı)

- `android/app/google-services.json`
- `ios/GoogleService-Info.plist`


Bu Firebase istemci dosyaları uygulamada bulunur ve tek başlarına Firebase Admin/service-account anahtarı değildir. Firebase servis hesabı JSON'u, APNs özel anahtarı/sertifikası ve imzalama parolaları gizli tutulmalıdır. Bu depoya veya uygulama paketine koymayın.


## Bitwarden'da saklanacaklar

Bitwarden'da bir **Vinu Chat — mobil yayın** koleksiyonu veya güvenli kaydı oluşturun. Parolaları ve anahtar dosyalarını depo dosyalarına, README'ye, e-postaya ya da sohbet mesajına koymayın.

1. **Android Play upload anahtarı**: `vinu-upload.jks` dosyasının kendisi, alias (`vinu-upload` olarak belirlenmiş), keystore parolası, key parolası ve oluşturulma tarihi. Şifreli dosya eki olarak saklayın ve ayrı güvenli bir yedek tutun. Kaybolursa uygulama güncellemesi yayımlamak zorlaşabilir.
2. **Firebase sunucu servis hesabı**: `vinu-rocketchat` için FCM gönderme yetkili servis hesabı JSON dosyası (özellikle private key), hangi Rocket.Chat sunucusunda kullanıldığı ve oluşturulma/yenileme tarihi. Bu dosya mobil uygulama Firebase dosyalarından farklıdır; Rocket.Chat sunucusuna girilir.
3. **APNs üretim kimlik bilgisi**: Rocket.Chat'e yüklenen APNs üretim sertifikasının özel anahtarla birlikte dışa aktarılmış `.p12` dosyası ve export parolası; varsa Key ID, Team ID, konu olan bundle ID ve sertifika bitiş tarihi. Sertifika yerine APNs `.p8` anahtarı kullanıldıysa `.p8` içeriğini, Key ID ve Team ID'yi saklayın. Kullanılmayan alanları boş bırakın.
4. **Apple imzalama yedeği**: dağıtım sertifikası ve özel anahtarı dışa aktarılmış `.p12` dosyası ile parolası; Apple Developer Team ID ve App Store Connect erişim sahibinin bilgisi. Xcode otomatik imza kullanıyorsa ayrıca profile dosyası saklamak şart olmayabilir.
5. **Play Console yükleme otomasyonu** (Fastlane kullanılacaksa): Play Console service-account JSON dosyası ve bağlı Google hesabı/izin açıklaması. Bu, Firebase servis hesabından farklıdır.

Firebase `google-services.json` ve `GoogleService-Info.plist` içindeki uygulama yapılandırma değerlerini Bitwarden'da not olarak tutabilirsiniz; bunlar Admin SDK private key yerine geçmez. Firebase API anahtarlarına uygun API kısıtlamalarını da Google Cloud Console'da uygulayın.

## Bildirim sunucusu notu

- Rocket.Chat sunucu Push ayarlarına APNs üretim kimlik bilgileri ve `vinu-rocketchat` Firebase servis hesabı girilir. Firebase servis hesabı JSON'u uygulamaya eklenmez.
- Kendi APNs/FCM kimlik bilgilerinizle gönderim için Rocket.Chat Push ayarındaki **Ağ geçidi etkin** seçeneği kapalı olmalıdır. Ekrandaki sunucu kaydı/gizlilik uyarısı Rocket.Chat ağ geçidinin çalışmasıyla ilgilidir; kendi anahtarlarınızın girilmesine dair bir başarı göstergesi değildir.
- Anahtar kapatılamıyorsa sunucu yöneticisi Rocket.Chat dağıtımındaki ayar kilidini/üzerine yazmayı kontrol etmelidir. Sunucu kapatılıp açılınca ayarın kalıcı olarak kapalı olduğunu doğrulayın.
- APNs ve FCM kimlik bilgileri girildikten sonra sunucunun yeniden başlatılması gerekebilir. Mevcut Rocket.Chat mobil uygulamalarını kullanan kişiler de etkilenebileceğinden geçişi koordineli yapın.

