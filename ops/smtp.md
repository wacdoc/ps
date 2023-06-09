# خپل د SMTP میل لیږلو سرور جوړ کړئ

## تمثیل

SMTP کولی شي مستقیم د بادل پلورونکو څخه خدمات واخلي، لکه:

* [ایمیزون SES SMTP](https://docs.aws.amazon.com/ses/latest/dg/send-email-smtp.html)
* [علي بادل بریښنالیک فشار](https://www.alibabacloud.com/help/directmail/latest/three-mail-sending-methods)

تاسو کولی شئ خپل میل سرور هم جوړ کړئ - لامحدود لیږل، ټیټ ټول لګښت.

لاندې، موږ ګام په ګام وښایه چې څنګه خپل میل سرور جوړ کړو.

## د سرور انتخاب

د ځان کوربه SMTP سرور عامه IP ته اړتیا لري د 25، 456، او 587 پرانیستې بندرونو سره.

په عام ډول کارول شوي عامه بادلونه دا بندرونه په ډیفالټ ډول بند کړي ، او ممکن د کاري حکم په صادرولو سره یې خلاص شي ، مګر دا په هرصورت خورا ستونزمن دی.

زه د کوربه څخه پیرود وړاندیز کوم چې دا بندرونه خلاص دي او د ریورس ډومین نومونو تنظیم کولو ملاتړ کوي.

دلته، زه [Contabo](https://contabo.com) وړاندیز کوم.

کونټابو د کوربه توب چمتو کونکی دی چې د آلمان په میونخ کې میشته دی چې په 2003 کې د خورا رقابتي نرخونو سره تاسیس شوی.

که تاسو یورو د پیرود اسعارو په توګه غوره کړئ، قیمت به ارزانه وي (د 8GB حافظې او 4 CPUs سره یو سرور په کال کې شاوخوا 529 یوآن لګښت لري، او د نصب کولو ابتدايي فیس د یو کال لپاره وړیا دی).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/UoAQkwY.webp)

کله چې امر ورکړئ، تبصره `prefer AMD` ، او د AMD CPU سره سرور به ښه فعالیت ولري.

په لاندې کې، زه به د Contabo VPS د مثال په توګه واخلم ترڅو وښیې چې څنګه خپل میل سرور جوړ کړئ.

## د اوبنټو سیسټم تنظیم کول

دلته عملیاتي سیسټم اوبنټو 22.04 دی

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/smpIu1F.webp)

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/m7Mwjwr.webp)

که په ssh کې سرور ښکاره شي `Welcome to TinyCore 13!` (لکه څنګه چې په لاندې شکل کې ښودل شوي)، دا پدې مانا ده چې سیسټم لا تر اوسه ندي نصب شوی. مهرباني وکړئ ssh منحل کړئ او د بیا ننوتلو لپاره یو څو دقیقو انتظار وکړئ.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/-qKACz9.webp)

کله چې `Welcome to Ubuntu 22.04.1 LTS` څرګند شي ، نو پیل بشپړ شوی ، او تاسو کولی شئ لاندې مرحلو ته دوام ورکړئ.

### [اختیاري] د پراختیا چاپیریال پیل کړئ

دا ګام اختیاري دی.

د اسانتیا لپاره، ما د اوبنټو سافټویر نصب او سیسټم ترتیب په [github.com/wactax/ops.os/tree/main/ubuntu](https://github.com/wactax/ops.os/tree/main/ubuntu) کې کیښود.

په یو کلیک سره د نصبولو لپاره لاندې کمانډ چل کړئ.

```
bash <(curl -s https://raw.githubusercontent.com/wactax/ops.os/main/ubuntu/boot.sh)
```

چینایي کاروونکي، مهرباني وکړئ د دې پرځای لاندې کمانډ وکاروئ، او ژبه، د وخت زون، او نور به په اوتومات ډول تنظیم شي.

```
CN=1 bash <(curl -s https://ghproxy.com/https://raw.githubusercontent.com/wactax/ops.os/main/ubuntu/boot.sh)
```

### کانټابو IPV6 فعالوي

IPV6 فعال کړئ ترڅو SMTP هم د IPV6 پتې سره بریښنالیکونه واستوي.

سمون `/etc/sysctl.conf`

لاندې کرښې بدل کړئ یا اضافه کړئ

```
net.ipv6.conf.all.disable_ipv6 = 0
net.ipv6.conf.default.disable_ipv6 = 0
net.ipv6.conf.lo.disable_ipv6 = 0
```

[د کانټابو ټیوټوریل سره تعقیب کړئ: ستاسو سرور ته د IPv6 ارتباط اضافه کول](https://contabo.com/blog/adding-ipv6-connectivity-to-your-server/)

ایډیټ `/etc/netplan/01-netcfg.yaml` ، یو څو لینونه اضافه کړئ لکه څنګه چې په لاندې شکل کې ښودل شوي (د Contabo VPS ډیفالټ ترتیب فایل لا دمخه دا لینونه لري، یوازې دوی یې بې برخې کړئ).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/5MEi41I.webp)

بیا `netplan apply` د دې لپاره چې تعدیل شوي ترتیب اغیزمن شي.

وروسته له دې چې ترتیب بریالی شو، تاسو کولی شئ د خپلې بهرنۍ شبکې ipv6 پته لیدلو لپاره `curl 6.ipw.cn` وکاروئ.

## د ترتیب کولو ذخیره کولو عملیات کلون کړئ

```
git clone https://github.com/wactax/ops.soft.git
```

## ستاسو د ډومین نوم لپاره وړیا SSL سند تولید کړئ

د بریښنالیک لیږل د کوډ کولو او لاسلیک کولو لپاره د SSL سند ته اړتیا لري.

موږ د سندونو جوړولو لپاره [acme.sh](https://github.com/acmesh-official/acme.sh) کاروو.

acme.sh د خلاصې سرچینې اتوماتیک سند لاسلیک کولو وسیله ده،

د تنظیم کولو ګودام ops.soft دننه کړئ، `./ssl.sh` چل کړئ، او په **پورتنۍ ډایرکټر** کې به یو `conf` فولډر رامینځته شي.

خپل د DNS برابرونکي له [acme.sh dnsapi](https://github.com/acmesh-official/acme.sh/wiki/dnsapi) څخه ومومئ، `conf/conf.sh` ایډیټ کړئ.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/Qjq1C1i.webp)

بیا د خپل ډومین نوم لپاره `123.com` او `*.123.com` سندونو رامینځته کولو لپاره `./ssl.sh 123.com` چل کړئ.

لومړی چل به په اوتومات ډول [acme.sh](https://github.com/acmesh-official/acme.sh) نصب کړي او د اتوماتیک نوي کولو لپاره ټاکل شوې دنده اضافه کړي. تاسو کولی شئ وګورئ `crontab -l` ، دلته داسې کرښه شتون لري لکه په لاندې ډول.

```
52 0 * * * "/mnt/www/.acme.sh"/acme.sh --cron --home "/mnt/www/.acme.sh" > /dev/null
```

د تولید شوي سند لپاره لاره یو څه ده لکه `/mnt/www/.acme.sh/123.com_ecc。`

د سند نوي کول به `conf/reload/123.com.sh` سکریپټ ته زنګ ووهي، دا سکریپټ ایډیټ کړئ، تاسو کولی شئ د اړوندو غوښتنلیکونو د سند کیچ تازه کولو لپاره کمانډونه لکه `nginx -s reload` اضافه کړئ.

## د chasquid سره SMTP سرور جوړ کړئ

[chasquid](https://github.com/albertito/chasquid) د خلاصې سرچینې SMTP سرور دی چې په Go ژبه لیکل شوی.

د پخوانی میل سرور برنامو لپاره د بدیل په توګه لکه Postfix او Sendmail، chasquid د کارولو لپاره ساده او اسانه دی، او دا د ثانوي پراختیا لپاره هم اسانه دی.

چلول `./chasquid/init.sh 123.com` به په اوتومات ډول په یو کلیک سره نصب شي (د خپل لیږل شوي ډومین نوم سره 123.com بدل کړئ).

## د بریښنالیک لاسلیک DKIM ترتیب کړئ

DKIM د بریښنالیک لاسلیکونو لیږلو لپاره کارول کیږي ترڅو لیکونه د سپیم په توګه چلند کیدو مخه ونیسي.

وروسته له دې چې کمانډ په بریالیتوب سره پرمخ ځي، تاسو به د DKIM ریکارډ ترتیبولو ته وهڅول شي (لکه څنګه چې لاندې ښودل شوي).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/LJWGsmI.webp)

یوازې خپل DNS ته د TXT ریکارډ اضافه کړئ (لکه څنګه چې لاندې ښودل شوي).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/0szKWqV.webp)

## د خدماتو حالت او لاګونه وګورئ

 `systemctl status chasquid` د خدمت حالت وګورئ.

د عادي عملیاتو حالت لکه څنګه چې په لاندې انځور کې ښودل شوی

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/CAwyY4E.webp)

 `grep chasquid /var/log/syslog` یا `journalctl -xeu chasquid` کولی شي د خطا لاګ وګوري.

## د ډومین نوم ترتیب بدل کړئ

د ریورس ډومین نوم دا دی چې IP پتې ته د اړونده ډومین نوم ته د حل کولو اجازه ورکړي.

د ریورس ډومین نوم تنظیم کول کولی شي بریښنالیکونه د سپیم په توګه پیژندلو مخه ونیسي.

کله چې بریښنالیک ترلاسه شي ، ترلاسه کونکی سرور به د لیږلو سرور IP پتې کې د ریورس ډومین نوم تحلیل ترسره کړي ترڅو دا تایید کړي چې ایا د لیږلو سرور معتبر ریورس ډومین نوم لري.

که د لیږلو سرور د ریورس ډومین نوم ونه لري یا که د ریورس ډومین نوم د لیږلو سرور IP پتې سره سمون ونلري، ترلاسه کوونکی سرور ممکن بریښنالیک د سپیم په توګه پیژني یا یې رد کړي.

[https://my.contabo.com/rdns](https://my.contabo.com/rdns) ته لاړ شئ او تنظیم کړئ لکه څنګه چې لاندې ښودل شوي

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/IIPdBk_.webp)

د ریورس ډومین نوم تنظیم کولو وروسته، په یاد ولرئ چې سرور ته د ډومین نوم ipv4 او ipv6 فارورډ ریزولوشن ترتیب کړئ.

## د chasquid.conf کوربه نوم ایډیټ کړئ

`conf/chasquid/chasquid.conf` د ریورس ډومین نوم ارزښت ته بدل کړئ.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/6Fw4SQi.webp)

بیا د خدمت بیا پیل کولو لپاره `systemctl restart chasquid` چل کړئ.

## د git ذخیره کې conf بیک اپ کړئ

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/Fier9uv.webp)

د مثال په توګه ، زه د conf فولډر زما خپل ګیتوب پروسې ته په لاندې ډول بیک اپ کړم

لومړی یو شخصي ګودام جوړ کړئ

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/ZSCT1t1.webp)

د conf لارښود دننه کړئ او ګودام ته یې وسپارئ

```
git init
git add .
git commit -m "init"
git branch -M main
git remote add origin git@github.com:wac-tax-key/conf.git
git push -u origin main
```

## لیږونکی اضافه کړئ

منډې

```
chasquid-util user-add i@wac.tax
```

لیږونکی اضافه کولی شي

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/khHjLof.webp)

### ډاډ ترلاسه کړئ چې پټنوم په سمه توګه تنظیم شوی

```
chasquid-util authenticate i@wac.tax --password=xxxxxxx
```

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/e92JHXq.webp)

د کارونکي اضافه کولو وروسته، `chasquid/domains/wac.tax/users` به ​​تازه شي، په یاد ولرئ چې ګودام ته یې وسپارئ.

## DNS د SPF ریکارډ اضافه کوي

SPF (د لیږونکي پالیسۍ چوکاټ) د بریښنالیک تصدیق ټیکنالوژي ده چې د بریښنالیک درغلۍ مخنیوي لپاره کارول کیږي.

دا د بریښنالیک لیږونکي پیژندنه د دې په چک کولو سره تاییدوي چې د لیږونکي IP پته د هغه ډومین نوم د DNS ریکارډونو سره سمون لري چې دا یې ادعا کوي، د جعلي بریښنالیکونو لیږلو څخه د جعلکارانو مخه نیسي.

د SPF ریکارډونو اضافه کول کولی شي د امکان تر حده د سپیم په توګه پیژندل شوي بریښنالیکونو مخه ونیسي.

که ستاسو د ډومین نوم سرور د SPF ډول ملاتړ نه کوي، یوازې د TXT ډول ریکارډ اضافه کړئ.

د مثال په توګه، د `wac.tax` SPF په لاندې ډول دی

`v=spf1 a mx include:_spf.wac.tax include:_spf.google.com ~all`

د `_spf.wac.tax` لپاره SPF

`v=spf1 a:smtp.wac.tax ~all`

په یاد ولرئ چې ما دلته `include:_spf.google.com` ، دا ځکه چې زه به وروسته د ګوګل میل باکس کې د لیږلو پتې په توګه `i@wac.tax` ترتیب کړم.

## د DNS ترتیب DMARC

DMARC د (ډومین پر بنسټ د پیغام تصدیق، راپور ورکول او موافقت) لنډیز دی.

دا د SPF باونسونو نیولو لپاره کارول کیږي (شاید د تشکیلاتو غلطیو له امله رامینځته شي ، یا بل څوک د سپیم لیږلو لپاره د تاسو په څیر وي).

د TXT ریکارډ اضافه کړئ `_dmarc` ،

منځپانګه په لاندې ډول ده

```
v=DMARC1; p=quarantine; fo=1; ruf=mailto:ruf@wac.tax; rua=mailto:rua@wac.tax
```

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/k44P7O3.webp)

د هر پیرامیټر معنی په لاندې ډول ده

### p (پاليسي)

د بریښنالیکونو اداره کولو څرنګوالی په ګوته کوي چې د SPF (لیږونکي پالیسۍ چوکاټ) یا DKIM (DomainKeys Identified Mail) تصدیق ناکاموي. د p پیرامیټر د دریو ارزښتونو څخه یو ته ټاکل کیدی شي:

* هیڅ: هیڅ اقدام نه دی شوی، یوازې د تایید پایله د بریښنالیک راپور ورکولو میکانیزم له لارې لیږونکي ته ورکول کیږي.
* قرنطین: هغه بریښنالیک چې تایید یې نه وي په سپیم فولډر کې واچوئ ، مګر بریښنالیک به مستقیم رد نه کړي.
* رد کړئ: په مستقیم ډول هغه بریښنالیکونه رد کړئ چې تایید کې پاتې راغلي.

### fo (د ناکامۍ اختیارونه)

د راپور ورکولو میکانیزم لخوا بیرته راستانه شوي معلوماتو مقدار مشخص کوي. دا د لاندې ارزښتونو څخه یو ته ټاکل کیدی شي:

* 0: د ټولو پیغامونو لپاره د اعتبار پایلې راپور کړئ
* 1: یوازې هغه پیغامونه راپور کړئ چې تایید یې ناکام وي
* d: یوازې د ډومین نوم تایید ناکامۍ راپور کړئ
* s: یوازې د SPF تایید ناکامۍ راپور کړئ
* l: یوازې د DKIM تایید ناکامۍ راپور کړئ

### rua & ruf

* rua (د مجموعي راپورونو لپاره د URI راپور ورکول): د راټولو راپورونو ترلاسه کولو لپاره بریښنالیک پته
* ruf (د عدلي راپورونو لپاره د URI راپور ورکول): د تفصيلي راپورونو ترلاسه کولو لپاره بریښنالیک آدرس

## ګوګل میل ته د بریښنالیکونو لیږلو لپاره د MX ریکارډونه اضافه کړئ

ځکه چې ما یو وړیا کارپوریټ میل باکس ونه موند چې د نړیوال ادرسونو ملاتړ کوي (کیچ-ټول، کولی شي د دې ډومین نوم ته لیږل شوي بریښنالیکونه ترلاسه کړي، د مخکینیو محدودیتونو پرته)، ما زما د Gmail میل باکس ته د ټولو بریښنالیکونو لیږلو لپاره chasquid کارولی.

**که تاسو خپل تادیه شوی سوداګریز میل باکس لرئ، مهرباني وکړئ MX مه بدلوئ او دا مرحله پریږدئ.**

`conf/chasquid/domains/wac.tax/aliases` ایډیټ کړئ، د لیږلو میل باکس ترتیب کړئ

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/OBDl2gw.webp)

`*` ټول بریښنالیکونه په ګوته کوي ، `i` د بریښنالیک آدرس مخکینۍ ده چې د لیږلو کارونکي پورته رامینځته شوي. د بریښنالیک لیږلو لپاره، هر کارن اړتیا لري چې یو لیک اضافه کړي.

بیا د MX ریکارډ اضافه کړئ (زه دلته مستقیم د ریورس ډومین نوم پته ته اشاره کوم، لکه څنګه چې په لاندې شکل کې په لومړۍ کرښه کې ښودل شوي).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/7__KrU8.webp)

وروسته له دې چې ترتیب بشپړ شو، تاسو کولی شئ د نورو بریښنالیکونو څخه کار واخلئ ترڅو بریښنالیکونه `i@wac.tax` او `any123@wac.tax` ته واستوئ ترڅو وګورئ چې تاسو په Gmail کې بریښنالیکونه ترلاسه کولی شئ.

که نه، د chasquid log وګورئ ( `grep chasquid /var/log/syslog` ).

## د ګوګل میل سره i@wac.tax ته بریښنالیک واستوئ

وروسته له دې چې ګوګل میل بریښنالیک ترلاسه کړ، ما په طبیعي توګه هیله درلوده چې د i.wac.tax@gmail.com پرځای `i@wac.tax` سره ځواب ووایم.

[https://mail.google.com/mail/u/1/#settings/accounts](https://mail.google.com/mail/u/1/#settings/accounts) ته لاړ شئ او "بل بریښنالیک پته اضافه کړئ" کلیک وکړئ.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/PAvyE3C.webp)

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/_OgLsPT.webp)

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/XIUf6Dc.webp)

بیا، د بریښنالیک لخوا ترلاسه شوي تایید کوډ دننه کړئ چې لیږل شوی و.

په نهایت کې ، دا د ډیفالټ لیږونکي پته په توګه تنظیم کیدی شي (د ورته پتې سره ځواب ورکولو اختیار سره).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/a95dO60.webp)

پدې توګه، موږ د SMTP میل سرور تاسیس بشپړ کړ او په ورته وخت کې د بریښنالیکونو لیږلو او ترلاسه کولو لپاره د ګوګل میل څخه کار اخلو.

## د ازموینې بریښنالیک واستوئ ترڅو وګورئ چې ایا ترتیب بریالی دی

`ops/chasquid` داخل کړئ

چلول `direnv allow` (direnv په تیرو یو کلیدي ابتکار پروسې کې نصب شوی او په شیل کې هک اضافه شوی)

بیا منډه کړه

```
user=i@wac.tax pass=xxxx to=iuser.link@gmail.com ./sendmail.coffee
```

د پیرامیټونو معنی په لاندې ډول ده

* کارن: SMTP کارن نوم
* پاس: SMTP پټنوم
* ته: ترلاسه کوونکی

تاسو کولی شئ د ازموینې بریښنالیک واستوئ.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/ae1iWyM.webp)

دا سپارښتنه کیږي چې د ازموینې بریښنالیکونو ترلاسه کولو لپاره Gmail وکاروئ ترڅو وګورئ چې تنظیمات بریالي دي.

### د TLS معیاري کوډ کول

لکه څنګه چې لاندې انځور کې ښودل شوي، دا کوچنی قفل شتون لري، پدې معنی چې د SSL سند په بریالیتوب سره فعال شوی.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/SrdbAwh.webp)

بیا "اصلي بریښنالیک وښایاست" کلیک وکړئ

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/qQQsdxg.webp)

### DKIM

لکه څنګه چې لاندې انځور کې ښودل شوي، د Gmail اصلي بریښنالیک پاڼه DKIM ښکاره کوي، پدې معنی چې د DKIM ترتیب بریالی دی.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/an6aXK6.webp)

د اصلي بریښنالیک سرلیک کې ترلاسه شوي چیک کړئ، او تاسو لیدلی شئ چې د لیږونکي پته IPV6 دی، پدې معنی چې IPV6 هم په بریالیتوب سره تنظیم شوی.
