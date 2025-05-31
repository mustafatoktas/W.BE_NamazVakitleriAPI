<h1 align="center">
Namaz Vakitleri API<a name="readme-top"></a>
</h1>

<div align="center">
  <img src="./Readme Resources/Namaz Vakitleri API Logo.png" alt="Logo" width="120"/>
</div>

## **İçindekiler**

- [API Hakkında](#api-hakkında)
- [Dokümantasyon](#dokümantasyon)
- [İstek Örnekleri](#i̇stek-örnekleri)
- [Bazı Şehir İsmi İstisnaları](#bazı-şehir-i̇smi-i̇stisnaları)
- [Lisans](#lisans)
- [İletişim](#i̇letişim)


![-----------------------------------------------------](./Readme%20Resources/Line.png)

## API Hakkında

Bu repo, namaz vakitlerini sunan API'nin dokümantasyonunu içerir.

API, namaz vakitlerini ve API anahtarı bilgilerini sunan iki farklı endpoint'ten oluşmaktadır.
Namaz vakitlerini sunan endpoint HTTP GET istekleriyle `il`, `ilce` ve `api_key` parametrelerini alır
ve belirtilen şehrin namaz vakitlerini 14 gün sonrasına kadar JSON formatında sağlar.

API anahtarı bilgilerini sunan endpoint ise kullanıcının API anahtarının seviyesini,
aylık istek sınırını ve kalan istek sayısını JSON formatında sağlar.


![-----------------------------------------------------](./Readme%20Resources/Line.png)

## Dokümantasyon

Base URL: `https://toktasoft.com/api/namaz-vakitleri/`


### Ana Endpoint

URL: `https://toktasoft.com/api/namaz-vakitleri/vakitler.php`

Bu endpoint 3 farklı parametre almaktadır.

| Parametre                       | Zorunlu Mu?                 | Açıklama                                                                |
| ------------------------------- | --------------------------- | ----------------------------------------------------------------------- |
| <p align="center">`api_key`</p> | <p align="center">evet</p>  | Kullanıcının API anahtarı                                               |
| <p align="center">`il`</p>      | <p align="center">evet</p>  | İl adı                                                                  |
| <p align="center">`ilce`</p>    | <p align="center">hayır</p> | İlçe adı <br> Parametre girilmezse merkez ilçenin vakitleri döndürülür. |


### API Anahtar Bilgilerini Sunan Endpoint

URL: `https://toktasoft.com/api/namaz-vakitleri/apikey.php`

Bu endpoint sadece 1 parametre almaktadır.

| Parametre                       | Zorunlu Mu?                | Açıklama                  |
| ------------------------------- | -------------------------- | ------------------------- |
| <p align="center">`api_key`</p> | <p align="center">evet</p> | Kullanıcının API anahtarı |


### API Anahtar Seviyeleri ve Aylık Sınırları

API anahtarı sahibi olabilmek için iletişime geçilmesi gerekilmektedir. 

| Seviye      | Aylık İstek Sınırı |
| ----------- | ------------------ |
| Bronz       | 100                |
| Gümüş       | 500                |
| Altın       | 1.000              |
| Platin      | 5.000              |
| Elmas       | 10.000             |
| VIP         | Sınırsız           |


![-----------------------------------------------------](./Readme%20Resources/Line.png)

## İstek Örnekleri

İstek örnekleri `curl` komut satırı aracı kullanılarak gösterilmiştir.

✅**Manisa merkez ilçenin vakitleri**

Merkez ilçeler için ilçe parametresi yazılmayabilir veya ilçe parametresine ilin adı yazılabilir.

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=manisa"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=manisa&ilce=manisa"
```

```json
{
  "status": 200,
  "monthly_request_count": 84,
  "result": {
    "today": "31 Mayıs Cumartesi",
    "city": "Manisa",
    "district": null,
    "vakitler": [
      {
        "tarih": "31-05-2025",
        "imsak": "03:54",
        "sabah": "05:41",
        "ogle": "13:13",
        "ikindi": "17:07",
        "aksam": "20:35",
        "yatsi": "22:14"
      },
      {
        "tarih": "01-06-2025",
        "imsak": "03:54",
        "sabah": "05:41",
        "ogle": "13:13",
        "ikindi": "17:07",
        "aksam": "20:36",
        "yatsi": "22:15"
      },
      ...
    ]
  },
  "error": null
}
```

❌**Yanlış İstek**

Kocaeli gibi merkez ilçenin adı il adından farklı olan bölgelerde merkez ilçe vakit bilgileri için ilçe parametresine ilçenin adı değil Manisa örneğindeki gibi ilin adı yazılmalıdır.
Kocaeli'de merkez ilçenin adı İzmit'tir.

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=kocaeli&ilce=izmit"
```

```json
{
  "status": 400,
  "monthly_request_count": 106,
  "result": null,
  "error": "Geçersiz ilçe parametresi"
}
```

✅**Elazığ merkez ilçenin vakitleri**

Adında ingilizce karakter dışında karakter bulunan il - ilçe farketmeksizin şehir isimleri olduğu
gibi yada ingilizce karakterlerle yazılabilir ve büyük küçük harf duyarlı değildir.

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=Elazığ"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=elazig"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=ElaZİg&ilce=ElaZİg"
```

```json
{
  "status": 200,
  "monthly_request_count": 122,
  "result": {
    "today": "31 Mayıs Cumartesi",
    "city": "Elazığ",
    "district": "Elazığ",
    "vakitler": [
      {
        "tarih": "31-05-2025",
        "imsak": "03:07",
        "sabah": "04:54",
        "ogle": "12:26",
        "ikindi": "16:20",
        "aksam": "19:48",
        "yatsi": "21:27"
      },
      {
        "tarih": "01-06-2025",
        "imsak": "03:06",
        "sabah": "04:53",
        "ogle": "12:26",
        "ikindi": "16:20",
        "aksam": "19:49",
        "yatsi": "21:28"
      },
      ...
    ]
  },
  "error": null
}
```

✅**Manisa'nın Akhisar ilçesinin vakitleri**

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=manisa&ilce=akhisar"
```

```json
{
  "status": 200,
  "monthly_request_count": 187,
  "result": {
    "today": "31 Mayıs Cumartesi",
    "city": "Manisa",
    "district": "Akhisar",
    "vakitler": [
      {
        "tarih": "31-05-2025",
        "imsak": "03:51",
        "sabah": "05:39",
        "ogle": "13:11",
        "ikindi": "17:06",
        "aksam": "20:34",
        "yatsi": "22:14"
      },
      {
        "tarih": "01-06-2025",
        "imsak": "03:50",
        "sabah": "05:38",
        "ogle": "13:12",
        "ikindi": "17:06",
        "aksam": "20:35",
        "yatsi": "22:15"
      },
      ...
    ]
  },
  "error": null
}
```

❌**Yanlış İstek**

İl - ilçe isimleri uyuşmazsa hata döndürülür.

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=kutahya&ilce=akhisar"
```

```json
{
  "status": 400,
  "monthly_request_count": 201,
  "result": null,
  "error": "Geçersiz ilçe parametresi"
}
```

✅**Apikey hakkında bilgiler**

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/apikey.php?api_key=myapikey"
```

```json
{
  "status": 200,
  "monthly_request_count": 78,
  "result": {
    "api_key": "mustafa",
    "api_level": "Bronz",
    "max_requests_per_month": 500,
    "remaining_requests": 422
  },
  "error": null
}
```


![-----------------------------------------------------](./Readme%20Resources/Line.png)

## Bazı Şehir İsmi İstisnaları

Farklı illerde bulunan ilçeler bazen aynı isimlere sahip olabiliyor. Bu yüzden bu ilçeler için istisnai istek isimleri belirlendi. 

- Aydın ve Bilecik'te bulunan Yenipazar

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=bilecik&ilce=yenipazar"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=aydin&ilce=yenipazar-a"
```

- Antalya ve Burdur'da bulunan Kemer

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=antalya&ilce=kemer"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=burdur&ilce=kemer-b"
```

- Antalya ve Isparta'da bulunan Aksu

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=antalya&ilce=aksu"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=isparta&ilce=aksu-i"
```

- Denizli ve Kastamonu'da bulunan Bozkurt

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=denizli&ilce=bozkurt"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=kastamonu&ilce=bozkurt-k"
```

- Kayseri ve Kastamonu'da bulunan Pınarbaşı

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=kayseri&ilce=pinarbasi"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=kastamonu&ilce=pinarbasi-k"
```

- Rize ve Tokat'ta bulunan Pazar

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=rize&ilce=pazar"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=tokat&ilce=pazar-t"
```

- Manisa ve Trabzon'da bulunan Köprübaşı

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=manisa&ilce=koprubasi"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=trabzon&ilce=koprubasi-t"
```

- Balıkesir ve Van'da bulunan Edremit

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=balikesir&ilce=edremit"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=van&ilce=edremit-v"
```

- Tekirdağ ve Van'da bulunan Saray

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=tekirdag&ilce=saray"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=van&ilce=saray-v"
```

- Mersin ve Yozgat'ta bulunan Aydıncık

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=mersin&ilce=aydincik"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=yozgat&ilce=aydincik-y"
```

- Tunceli ve Karabük'te bulunan Ovacık

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=tunceli&ilce=ovacik"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=karabuk&ilce=ovacik-k"
```

- Çanakkale ve Karabük'te bulunan Yenice

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=canakkale&ilce=yenice"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=karabuk&ilce=yenice-k"
```

- Burdur ve Sivas'ta bulunan Altınyayla

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=burdur&ilce=altinyayla"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=sivas&ilce=altinyaylas"
```

- Konya ve Zonguldak'ta bulunan Ereğli

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=konya&ilce=eregli"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=zonguldak&ilce=karadeniz-eregli"
```


### Diğer İstisnai Şehir İstek İsimleri

- Isparta'da bulunan Şarkikaraağaç

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=isparta&ilce=sarki-karaagac"
```

- Isparta'da bulunan Yenişarbademli

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=isparta&ilce=yenisar-bademli"
```

- Tekirdağ'da bulunan Marmaraereğlisi

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=tekirdag&ilce=mereglisi"
```


![-----------------------------------------------------](./Readme%20Resources/Line.png)

<div align="center">
  <a href="https://github.com/mustafatoktas/W.BE_RepoVisitorCounterAPI"><img src="https://toktasoft.com/api/repo-visitor-counter?repo=otn1xh4fnl4umsm&show_repo_name=1&show_date=1&show_brand=0&txt_color=209,215,224&bg_color=45,52,58" alt="Repo Visitor Counter"/></a>
</div>

<br>
  
<div align="center">
  <a href="https://buymeacoffee.com/mustafatoktas"><img src="./Readme Resources/Communication/Buy Me a Coffee.png" alt="Buy Me a Coffee" height="64"/></a>
</div>


![-----------------------------------------------------](./Readme%20Resources/Line.png)

## Lisans

```
Copyright 2024-2025 Mustafa TOKTAŞ

Licensed under the GNU General Public License v3.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    https://www.gnu.org/licenses/gpl-3.0.html

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```


![-----------------------------------------------------](./Readme%20Resources/Line.png)

## İletişim

<a href="mailto:info@mustafatoktas.com"             ><img src="./Readme Resources/Communication/Mail.png"     alt="Mail"     width="64"/></a>
<a href="https://t.me/mustafatoktas00"              ><img src="./Readme Resources/Communication/Telegram.png" alt="Telegram" width="64"/></a>
<a href="https://www.linkedin.com/in/mustafatoktas/"><img src="./Readme Resources/Communication/LinkedIn.png" alt="LinkedIn" width="64"/></a>

<p align="center">
  <a href="#readme-top"><img src="./Readme Resources/Back to Top.png" alt="Back to Top" height="64"/></a>
</p>
