# Facebook-toplu-arkadas-silme

NOT : İhlal yememek için kodun içerisine bekleme süresi ekledim. insana yakın hızda siliyor.

### ADIM 1

<p>- Facebook dilini Türkçe yap</p>
<p>- Tampermonkey eklentisi tarayıcınıza ekleyin</p>
<p>- Yeni Betik oluştura tıklayın</p>

![image](https://github.com/DenizKod/ARK-ISTEGI-IPTAL-ETME/assets/168285638/7e1b2696-803e-447a-ae3f-f7844a44d28f)

#### Aşağıdaki kodu betik oluşturma sayfasına yapıştırıp betiği kaydedin

```
// ==UserScript==
// @name         Arkadaş Listesinden Otomatik Silme
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  BGY dışı herkesi silme
// @author       Deniz KOD
// @match        https://www.facebook.com/*
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    document.addEventListener('keydown', function(e) {
        if (e.key === 'F2') {
            console.log("Silme işlemi başlatıldı.");
            startRemovingFriends();
        } else if (e.key === 'F4') {
            console.log("Silme işlemi durduruldu.");
            stopRemovingFriends();
        }
    });

    let isRunning = false;
    let triedToRemove = new Set();  // Zaten silinmeye çalışılmış arkadaşları takip etmek için

    function startRemovingFriends() {
        isRunning = true;
        removeNextFriend();
    }

    function stopRemovingFriends() {
        isRunning = false;
    }

    function removeNextFriend() {
        if (!isRunning) return;

        const allFriendButtons = Array.from(document.querySelectorAll('div[aria-label="Arkadaşlar"]'));
        const friendButton = allFriendButtons.find(button => !triedToRemove.has(button));

        if (friendButton) {
            triedToRemove.add(friendButton);  // Bu arkadaşı işaretleyerek gelecekte tekrar denemeyi önle
            friendButton.click();

            setTimeout(() => {
                const allMenuItems = Array.from(document.querySelectorAll('[role="menuitem"]'));
                const removeOption = allMenuItems.find(item => item.textContent.includes("Arkadaşlarımdan Çıkar"));

                if (removeOption) {
                    removeOption.click();
                    console.log("Arkadaşlarımdan Çıkar seçeneği tıklandı.");
                    setTimeout(confirmDeletion, 500);
                } else {
                    console.log("Arkadaşlarımdan Çıkar seçeneği bulunamadı.");
                    handlePopupError();
                }
            }, 1000);
        } else {
            console.log("Daha fazla arkadaş butonu bulunamadı.");
        }
    }

    function confirmDeletion() {
        const confirmButton = document.querySelector('div[aria-label="Onayla"]');
        if (confirmButton) {
            confirmButton.click();
            console.log("Onayla butonuna basıldı.");
            setTimeout(removeNextFriend, 1500);
        } else {
            console.log("Onayla butonu bulunamadı.");
            handlePopupError();
        }
    }

    function handlePopupError() {
        const errorButton = document.querySelector('span[class*="Tamam"]');
        if (errorButton) {
            errorButton.click();
            console.log("Hata mesajı 'Tamam' butonu tıklandı, devam ediliyor.");
        }
        setTimeout(removeNextFriend, 10);
    }
})();
```
<p>- Profilde arkadaşlarınız'a girin</p>

### F2 tuşu ile silme işlemini başlatın

<p>- sildikçe aşağı doğru kaydırın yada betiği başlatmadna önce en aşağı kadar inin</p>

### DİKKAT : Bazı kişilerin profili "facebook tarafından banlandığı için o kişileri silemiyorsunuz"

![image](https://github.com/DenizKod/FacebookBGY/assets/168285638/baa299f3-c8f7-4f7e-bed4-58d829308df4)

Bot bu durumda takılma yaşıyor ve sürekli aynı kişiyi silmeye çalışıyor. düzeltmek için "TAMAM" butonuna bastıktan sonra hemen yanındaki arkadaşınıza tıklayın. aşağıdaki görselde işaretli yere tıklamanız gerekiyor.

![image](https://github.com/DenizKod/FacebookBGY/assets/168285638/9cb56c73-28b0-4eb6-922a-9a11a9614f95)

### NOT : İhlal yememek için kodun içerisine bekleme süresi ekledim. insana yakın hızda siliyor.

