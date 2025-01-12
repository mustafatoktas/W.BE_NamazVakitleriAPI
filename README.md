<h1 align="center">Namaz Vakitleri API</h1>

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


![-----------------------------------------------------](./Readme%20Resources/Çizgi.png)

## API Hakkında

Bu repo, Php diliyle geliştirilmiş olan namaz vakitlerini sunan
API'nin dokümantasyonunu içerir.

API, namaz vakitlerini ve API anahtarı bilgilerini sunan iki farklı endpoint'ten oluşmaktadır.
Namaz vakitlerini sunan endpoint HTTP GET istekleriyle `il`, `ilce` ve `api_key` parametrelerini alır
ve belirtilen şehrin namaz vakitlerini 14 gün sonrasına kadar JSON formatında sağlar.

API anahtarı bilgilerini sunan endpoint ise kullanıcının API anahtarının seviyesini,
aylık istek sınırını ve kalan istek sayısını JSON formatında sağlar.


![-----------------------------------------------------](./Readme%20Resources/Çizgi.png)

## Dokümantasyon

Base URL: `https://toktasoft.com/api/namaz-vakitleri/`


### Ana Endpoint

URL: `https://toktasoft.com/api/namaz-vakitleri/vakitler.php`

Bu endpoint 3 farklı parametre almaktadır.

| Parametre                       | Zorunlu Mu?                 | Açıklama                                                                |
| ------------------------------- | --------------------------- | ----------------------------------------------------------------------- |
| <p align="center">`il`</p>      | <p align="center">evet</p>  | İl adı                                                                  |
| <p align="center">`ilce`</p>    | <p align="center">hayır</p> | İlçe adı <br> Parametre girilmezse merkez ilçenin vakitleri döndürülür. |
| <p align="center">`api_key`</p> | <p align="center">evet</p>  | Kullanıcının API anahtarı                                               |


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


![-----------------------------------------------------](./Readme%20Resources/Çizgi.png)

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
  "success": true,
  "result": [
    {
      "tarih": "29-07-2024",
      "imsak": "04:23",
      "sabah": "06:02",
      "ogle": "13:22",
      "ikindi": "17:13",
      "aksam": "20:31",
      "yatsi": "22:04"
    },
    {
      "tarih": "30-07-2024",
      "imsak": "04:24",
      "sabah": "06:03",
      "ogle": "13:22",
      "ikindi": "17:13",
      "aksam": "20:30",
      "yatsi": "22:02"
    },
    ...
  ],
  "today": "29 Temmuz Pazartesi",
  "city": "Manisa",
  "district": null,
  "monthly_request_count": 84
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
  "success": false,
  "error": "Geçersiz ilçe parametresi.",
  "monthly_request_count": 106
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
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=ElaZIg&ilce=ElaZIg"
```

```json
{
  "success": true,
  "result": [
    {
      "tarih": "30-07-2024",
      "imsak": "03:37",
      "sabah": "05:16",
      "ogle": "12:35",
      "ikindi": "16:26",
      "aksam": "19:43",
      "yatsi": "21:15"
    },
    {
      "tarih": "31-07-2024",
      "imsak": "03:38",
      "sabah": "05:17",
      "ogle": "12:35",
      "ikindi": "16:25",
      "aksam": "19:42",
      "yatsi": "21:14"
    },
    ...
  ],
  "today": "30 Temmuz Salı",
  "city": "Elazığ",
  "district": null,
  "monthly_request_count": 122
}
```

✅**Manisa'nın Akhisar ilçesinin vakitleri**

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=manisa&ilce=akhisar"
```

```json
{
  "success": true,
  "result": [
    {
      "tarih": "30-07-2024",
      "imsak": "04:21",
      "sabah": "06:01",
      "ogle": "13:20",
      "ikindi": "17:12",
      "aksam": "20:29",
      "yatsi": "22:02"
    },
    {
      "tarih": "31-07-2024",
      "imsak": "04:23",
      "sabah": "06:02",
      "ogle": "13:20",
      "ikindi": "17:11",
      "aksam": "20:28",
      "yatsi": "22:01"
    },
    ...
  ],
  "today": "30 Temmuz Salı",
  "city": "Manisa",
  "district": "Akhisar",
  "monthly_request_count": 187
}
```

❌**Yanlış İstek**

İl - ilçe isimleri uyuşmazsa hata döndürülür.

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=kutahya&ilce=akhisar"
```

```json
{
  "success": false,
  "error": "Geçersiz ilçe parametresi.",
  "monthly_request_count": 201
}
```

✅**Apikey hakkında bilgiler**

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/apikey.php?api_key=myapikey"
```

```json
{
  "success": true,
  "result": {
    "api_key": "myapikey",
    "api_level": "Gümüş",
    "max_requests_per_month": 500,
    "remaining_requests": 422
  },
  "monthly_request_count": 78
}
```


![-----------------------------------------------------](./Readme%20Resources/Çizgi.png)

## Bazı Şehir İsmi İstisnaları

Farklı illerde bulunan bazı ilçeler aynı isimlere sahip olabiliyor. Bu yüzden bu ilçeler için istisnai istek isimleri belirlendi. 

- Aydın - Bilecik | Yenipazar
```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=bilecik&ilce=yenipazar"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=aydin&ilce=yenipazar-a"
```

- Antalya - Burdur | Kemer

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=antalya&ilce=kemer"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=burdur&ilce=kemer-b"
```

- Antalya - Isparta | Aksu

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=antalya&ilce=aksu"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=isparta&ilce=aksu-i"
```

- Denizli - Kastamonu | Bozkurt

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=denizli&ilce=bozkurt"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=kastamonu&ilce=bozkurt-k"
```

- Kayseri - Kastamonu | Pınarbaşı

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=kayseri&ilce=pinarbasi"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=kastamonu&ilce=pinarbasi-k"
```

- Rize - Tokat | Pazar

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=rize&ilce=pazar"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=tokat&ilce=pazar-t"
```

- Manisa - Trabzon | Köprübaşı

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=manisa&ilce=koprubasi"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=trabzon&ilce=koprubasi-t"
```

- Balıkesir - Van | Edremit

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=balikesir&ilce=edremit"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=van&ilce=edremit-v"
```

- Tekirdağ - Van | Saray

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=tekirdag&ilce=saray"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=van&ilce=saray-v"
```

- Mersin - Yozgat | Aydıncık

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=mersin&ilce=aydincik"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=yozgat&ilce=aydincik-y"
```

- Tunceli - Karabük | Ovacık

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=tunceli&ilce=ovacik"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=karabuk&ilce=ovacik-k"
```

- Çanakkale - Karabük | Yenice

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=canakkale&ilce=yenice"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=karabuk&ilce=yenice-k"
```

- Burdur - Sivas | Altınyayla

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=burdur&ilce=altinyayla"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=sivas&ilce=altinyaylas"
```

- Konya - Zonguldak | Ereğli

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=konya&ilce=eregli"
```

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=zonguldak&ilce=karadeniz-eregli"
```

### Diğer İstisnai Şehir İstek İsimleri

- Isparta | Şarkikaraağaç

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=isparta&ilce=sarki-karaagac"
```

- Isparta | Yenişarbademli

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=isparta&ilce=yenisar-bademli"
```

- Tekirdağ | Marmaraereğlisi

```sh
curl -X GET "https://toktasoft.com/api/namaz-vakitleri/vakitler.php?api_key=myapikey&il=tekirdag&ilce=mereglisi"
```


![-----------------------------------------------------](./Readme%20Resources/Çizgi.png)

<a href="https://github.com/mustafatoktas/W.BE_RepoVisitorCounterAPI" target="_blank"> <img src="https://toktasoft.com/api/github2/repo-visitor-counter.php?repo=otn1xh4fnl4umsm&show_repo_name=1&show_date=1&show_brand=0" alt="Repo Visitor Counter"/> </a>

<a href="https://buymeacoffee.com/mustafatoktas" target="_blank"> <img src="./Readme Resources/İletişim/Buy Me a Coffee.png" alt="Buy Me a Coffee" height="64"/> </a>


![-----------------------------------------------------](./Readme%20Resources/Çizgi.png)

## Lisans

```
Copyright 2024 Mustafa TOKTAŞ

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


![-----------------------------------------------------](./Readme%20Resources/Çizgi.png)

## İletişim

<a href="mailto:info@mustafatoktas.com"              target="_blank"> <img src="./Readme Resources/İletişim/Mail.png"     alt="Mail"     width="64"/> </a>
<a href="https://t.me/mustafatoktas00"               target="_blank"> <img src="./Readme Resources/İletişim/Telegram.png" alt="Telegram" width="64"/> </a>
<a href="https://www.linkedin.com/in/mustafatoktas/" target="_blank"> <img src="./Readme Resources/İletişim/LinkedIn.png" alt="LinkedIn" width="64"/> </a>