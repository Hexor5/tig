# Tiger Zone Tracker

Tiger ki location (latitude, longitude) aur kab dekha gaya, ye daalo.
Page batata hai ki tiger abhi kitne km ke andar ho sakta hai, aur map par circle dikhata hai.

## Kaise chalayein

`index.html` ko Chrome mein kholo. Bas itna hi. Saara code (design + logic) isi ek file mein hai.

GitHub Pages par: repo → Settings → Pages → Branch: `main`, folder: `/ (root)` → Save. Thodi der mein website ka link mil jayega.

## Hisaab kaise hota hai (`index.html` ke andar `analyze()`)

- Do location ke beech doori: haversine formula (lat-long se km).
- Speed = doori ÷ time (km/din). 4 ya zyada location hon to tiger ki apni speed, warna default (Speed setting).
- Peela circle (zyada sambhavna) = aam speed × last sighting ke baad ka time + 0.5 km.
- Laal circle (zyada se zyada) = tez speed (sabse tez 10% movements) × time + 0.5 km. Hadd 60 km.
- 60 km/din se tez movement ko galat entry maan kar speed ke hisaab se hata diya jata hai.

## Badalne layak cheezein

- Default speed: `index.html` mein `sampleState()` → `set:{avg:5,max:15}`
- Radius ki hadd: `CAP=60` (km), GPS error: `FLOOR=0.5` (km)
- Satellite map: `LAYERS.sat` (Esri World Imagery). Production mein apni map API key wali service lagayein.

## Dhyan rakhein

- Data abhi sirf usi browser ke localStorage mein save hota hai. Team ke liye server + database chahiye (agla step).
- Tiger location sensitive data hai, public website par login ke bina mat daalein.
- Ye sambhavna batata hai, pakki location nahi.
