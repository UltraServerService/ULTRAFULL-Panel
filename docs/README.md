# ULTRAFULL Commercial

Version 0.2.8 raises the bounded Spoof TUN queue from 64 to 1024 packets. Only the exact old default of an owned TCP Spoof installation is migrated, with an original-config backup. Custom settings and source identities are preserved. Updating stages this setting for the next Spoof restart; it does not restart an active tunnel. Existing upstream packet loss can still affect reliability.

نسخهٔ ۰٫۲٫۸ صف محدود اسپوف را از ۶۴ به ۱۰۲۴ بسته افزایش می‌دهد. فقط پیش‌فرض قدیمیِ تونل ثبت‌شده با پشتیبان ارتقا می‌یابد؛ تنظیمات سفارشی و آدرس‌های اسپوف حفظ می‌شوند. برای تونل فعال، صف جدید از ری‌استارت بعدی اسپوف اثر می‌کند و آپدیت خودکار آن را قطع نمی‌کند. این اصلاح تضمین حذف افت بستهٔ بالادست نیست.

Version 0.2.7 corrects invalid raw TCP carrier checksums using an owned, exact-flow egress action. Spoof identities, real endpoints, encryption and TUN/IPX/TCP remain unchanged. The correction is restored when the Spoof service starts and is removed with that tunnel, preserving unrelated traffic and queue settings. Updating applies it to an already running Spoof service without restarting the tunnel. New Spoof configurations use a 5-second heartbeat and a 60-second timeout.

نسخهٔ ۰٫۲٫۷ چک‌سام حامل TCP اسپوف را فقط برای جریان همان تونل اصلاح می‌کند؛ مبدأهای اسپوف، مقصدها، رمزنگاری و ساختار TUN/IPX/TCP تغییر نمی‌کنند. اصلاح در استارت بعدی هم برقرار می‌شود و همراه حذف تونل پاک می‌شود. آپدیت، اصلاح چک‌سام را بدون ری‌استارت تونل فعال اعمال می‌کند.

Spoof is a fourth independent family (TUN / IPX / TCP), with scoped guest firewall and optional per-VM hypervisor setup. Initial installation offers the existing repository token or a license. The owner token is saved root-only after a verified download; license activation remains cached until it expires or is rejected. The installed ultrafull command opens local files without fetching GitHub. Updates and first-time missing package downloads need network access.

گزینهٔ چهارم اسپوف از TUN/IPX/TCP استفاده می‌کند. ابتدا ورود با توکن اصلی یا لایسنس انتخاب می‌شود. توکن پس از دریافت تأییدشده، خصوصی برای روت ذخیره می‌شود. بازکردن پنل نصب‌شده به گیت‌هاب وابسته نیست؛ تمدید لایسنس و دریافت آپدیت مسیر جدا دارند.

## English

ULTRAFULL manages Ultra, Paqet, Backhaul and Spoof from one terminal panel. Ultra offers
ICMP, UDP-hop, HTTPS, XHTTP and UltraMax transport choices. This guide covers
installation and administration; it intentionally omits transport internals.

Use a fresh Ubuntu 22.04+ or Debian 12+ amd64 server with systemd. Install your
user panel first if you need one. Obtain an Iran-IP license from the seller.
Download and run the official installer:

```bash
curl -fL --proto '=https' --tlsv1.2 https://ultraradar.ultranec.pro:9448/install-commercial.sh -o install-commercial.sh
sudo bash install-commercial.sh
```

Choose **GitHub token / owner** or **License** on first installation. Token access uses the existing private repository; it does not require a license. In license mode the installer asks for the server role. Outbound HTTPS to the authority on port
9448 is required. The same command installs Iran and Foreign.

The installer opens the ULTRAFULL panel. Choose **1 — Iran server** or
**2 — Foreign server**. On **Iran**, enter only the license; its verified IPv4
must already be assigned to the server. On **Foreign**, enter only the licensed Iran IPv4. The local source IPv4 is detected automatically for the connection to the license service. Foreign
installation can happen first. The authority verifies that the request actually
comes from the server IPv4 and that the Iran entitlement is active; foreign
servers have no per-license count limit. Knowing an active Iran IP permits
foreign enrollment by design; a foreign grant cannot activate the Iran server.

Run `sudo ultrafull`. Choose **1 Install bots** on Iran. Either create a SOCKS or
mixed inbound in the Iran panel with listen `127.0.0.1`, port `10805` and no
credentials, or choose automatic creation. Never expose this unauthenticated
inbound publicly. Automatic creation uses a separate owned local Xray service.
Existing panel attachment requires its local HandlerService and RoutingService;
only a routing rule and owned outbound are added, without restarting user Xray.

Supply a reliable client link for Telegram. You may save the setup and supply it
later; the bots remain pending until access is configured. All three bots and
their child control bots use this same local Telegram inbound. Enter separate
tokens and numeric owner IDs for **ultrasource**, **tunnelcheck**, **configuptime**.
The installer verifies tokens through the configured route; an active webhook
or duplicate token is rejected.

Choose **2 Register server (SSH)** on Iran, then copy its displayed public key
to the same option on each foreign server. Each customer has their own generated
SSH identity. No seller SSH key is installed. In ultrasource, add a named pair,
its management IPs, SSH users and dedicated child-control bot token. Supply a
separate, real, non-expiring user-panel client link when asked for uptime. This
credential determines whether customers can pass traffic; it is not the
Telegram connectivity credential. Ensure its port is present on the pair.

tunnelcheck reads the ultrasource registry: select a pair name, then Ultra,
UltraMax, Paqet or Backhaul. IP changes are checked on both hosts and applied
with the existing rollback/restart controls. Removing a pair stops its uptime
notifications; historical incidents remain available. Uptime tests use real
HTTPS requests, rather than declaring a tunnel healthy from service state alone.

UltraMax supports explicit **TCP** and **ICMP** engine selection in the panel and control bot. The choice is coordinated across both servers, preserves keys and configured limits, and survives an IP change. A mode or source-IP change restarts the affected tunnel services, checks complete download and upload transfers, and restores the previous configuration if verification fails. Update both hosts before using these controls.

Panel labels are English. The first authenticated connection of each installed
tunnel shows the full-screen ULTRAFULL animation once. Reopening a panel or
changing ports does not replay the animation. Tunnel traffic encryption and
configured bandwidth limits are not reduced by licensing.

Use **Update panel** for a signed commercial update. It preserves private
customer state and installed tunnel configurations; a classic installation is
not silently converted. Use `sudo ultrafull-activate` after renewal. Correct the
server clock if activation reports time errors. Online licenses refresh every
15 minutes; an already issued lease can keep working for at most 24 hours during
an authority outage, and never beyond the license expiry. Explicit revocation
removes the lease on refresh. The license guard stops only owned tunnels when
access expires, preserving SSH and the user's panel.

Back up `/etc/ultrafull-commercial`, `/var/lib/ultrafull-commercial`, and
`/opt/ultrafull-commercial/manager/data` securely. These contain private data.
The terminal's **Delete all tunnels** reviews and removes owned tunnels on that
server; run it on both sides, then delete the pair in ultrasource. It preserves
the user panel, unrelated services and SSH access. This release does not promise
seamless migration of an established customer TCP session between transports.

See [component notices](THIRD_PARTY.md). Open-source rights remain intact.

## فارسی

<div dir="rtl">

### معرفی

ULTRAFULL برای نصب و مدیریت Ultra، Paqet و Backhaul طراحی شده است. در بخش Ultra، گزینه‌های ICMP، UDP-hop، HTTPS، XHTTP و UltraMax در دسترس هستند.

### پیش‌نیازها و نصب

از سرور خام **Ubuntu 22.04 یا جدیدتر** یا **Debian 12 یا جدیدتر**، با معماری amd64 و systemd استفاده کنید. در صورت نیاز به پنل مدیریت کاربران، ابتدا آن را نصب کنید.

دستورهای نصب بخش انگلیسی را روی هر دو سرور اجرا کنید. دسترسی HTTPS خروجی به سرویس لایسنس روی پورت `9448` باید برقرار باشد.

لایسنس به **یک آی‌پی عمومی ایران** اختصاص دارد و تعداد سرورهای خارج محدود نیست:

- در پنل، گزینهٔ **۱ — Iran server** را انتخاب کرده و فقط لایسنس را وارد کنید. IP از لایسنس خوانده می‌شود و باید روی همان سرور موجود باشد.
- برای خارج، گزینهٔ **۲ — Foreign server** را انتخاب کنید. فقط IP ایرانِ دارای لایسنس را وارد کنید. آی‌پی مبدأ این سرور برای ارتباط با سرویس لایسنس به‌صورت خودکار تشخیص داده می‌شود.

اگر لایسنس معتبر نباشد، پیام راهنما داخل پنل نمایش داده می‌شود. برای دریافت لایسنس معتبر به **@licenseultra_bot** مراجعه کنید.

نصب سرور خارج پیش از ایران نیز امکان‌پذیر است. سرویس مرکزی، اعتبار لایسنس ایران و آی‌پی مبدأ درخواست را بررسی می‌کند. در این مدل، اطلاع از آی‌پی ایرانِ دارای لایسنس برای ثبت سرور خارج کافی است؛ مجوز خارج برای فعال‌سازی سرور ایران قابل استفاده نیست.

پس از نصب، پنل با دستور `sudo ultrafull` اجرا می‌شود.

### نصب ربات‌ها

نصب و اجرای ربات‌ها روی **سرور ایران خودتان** انجام می‌شود. گزینهٔ اول پنل، **Install bots**، مراحل راه‌اندازی را آغاز می‌کند.

برای اتصال به تلگرام، یک ورودی SOCKS یا mixed با تنظیمات زیر در پنل ایران ایجاد کنید:

| تنظیم | مقدار |
| --- | --- |
| Listen IP | `127.0.0.1` |
| Port | `10805` |
| Username / Password | بدون نام کاربری و رمز عبور |

این ورودی باید فقط به‌صورت محلی در دسترس باشد. در صورت انتخاب ساخت خودکار، اسکریپت سرویس محلی جداگانه‌ای ایجاد می‌کند. اتصال به ورودی موجود پنل از طریق API محلی انجام می‌شود و نیازی به ری‌استارت Xray کاربران ندارد.

یک کانفیگ معتبر و پایدار برای اتصال به تلگرام وارد کنید. هر سه ربات و ربات‌های کنترل جفت‌سرورها از همین ورودی استفاده می‌کنند؛ بنابراین کانفیگ انتخابی نباید مرتباً قطع یا منقضی شود. افزودن یا تغییر آن در مراحل بعد نیز امکان‌پذیر است. تا پیش از ثبت کانفیگ معتبر، وضعیت اتصال ربات‌ها در انتظار راه‌اندازی باقی می‌ماند.

توکن و آیدی عددی مدیر را برای هر یک از ربات‌های **ultrasource**، **tunnelcheck** و **configuptime** وارد کنید. برای هر ربات از توکن جداگانه استفاده کنید. توکن‌ها از مسیر پروکسی بررسی می‌شوند و ربات دارای webhook فعال پذیرفته نمی‌شود.

### ثبت کلید SSH و جفت‌سرورها

گزینهٔ **Register server (SSH)** کلید اختصاصی مدیریت سرورهای شما را آماده می‌کند. کلید عمومی نمایش‌داده‌شده در ایران را در همین گزینه روی سرور خارج وارد کنید. کلید خصوصی روی سرور ایران خودتان نگهداری می‌شود؛ کلید SSH فروشنده نصب نخواهد شد.

سپس در ربات ultrasource، یک نام برای جفت‌سرور انتخاب کنید و آی‌پی‌های مدیریت، نام کاربران SSH و توکن ربات کنترل آن جفت را وارد کنید.

برای پایش قطعی‌ها، یک کانفیگ سالم از پنل اصلی کاربران ارائه کنید. این کانفیگ باید روی پورت مشتری همان جفت فعال باشد و ترجیحاً محدودیت زمان یا حجم نداشته باشد. تشخیص قطعی کاربران بر اساس همین کانفیگ انجام می‌شود؛ کانفیگ اتصال ربات‌ها به تلگرام جداست.

### مدیریت و پایش

در ربات tunnelcheck، نام جفت‌سرور و سپس نوع تونل را انتخاب کنید. تغییر آی‌پی روی هر دو سمت بررسی و همراه با ری‌استارت سرویس مربوط اعمال می‌شود.

با حذف جفت از ultrasource، هشدارهای جدید اپتایم آن متوقف می‌شوند و تاریخچهٔ قطعی‌های قبلی حفظ می‌شود. پایش مشتری با درخواست واقعی HTTPS انجام می‌شود؛ فعال بودن سرویس به‌تنهایی نشان‌دهندهٔ اتصال سالم نیست.

پس از اولین اتصال تأییدشدهٔ هر تونل، نشان ULTRAFULL یک بار نمایش داده می‌شود. باز کردن مجدد پنل یا تغییر پورت، این نمایش را تکرار نمی‌کند. زبان پنل انگلیسی است.

### انتخاب حالت UltraMax

در پنل و ربات کنترل، برای UltraMax یکی از دو حالت **TCP** یا **ICMP** را انتخاب کنید. تغییر روی هر دو سرور هماهنگ اعمال می‌شود. انتخاب حالت با تغییر آی‌پی عوض نمی‌شود و کلیدها و سقف‌های تنظیم‌شده حفظ می‌شوند.

پس از تغییر حالت یا آی‌پی، سرویس‌های مربوط دوباره راه‌اندازی می‌شوند و دانلود و آپلود واقعی بررسی می‌شود. اگر بررسی موفق نباشد، تنظیم قبلی برمی‌گردد. پیش از استفاده، هر دو سرور را به‌روز کنید.

### به‌روزرسانی و تمدید

برای دریافت نسخهٔ امضاشده از گزینهٔ **Update panel** استفاده کنید. تنظیمات خصوصی و کانفیگ تونل‌های نصب‌شده حفظ می‌شوند. نصب نسخهٔ تجاری، نسخهٔ شخصی قدیمی را به‌صورت خودکار تبدیل نمی‌کند.

پس از تمدید لایسنس، دستور `sudo ultrafull-activate` را اجرا کنید. اگر تونل به دلیل انقضای مجوز متوقف شده باشد، پس از فعال‌سازی از بخش Manage tunnels آن را راه‌اندازی کنید. ساعت سرور باید صحیح باشد.

اعتبار مجوز هر ۱۵ دقیقه بررسی می‌شود. در صورت اختلال موقت سرویس مرکزی، مجوز قبلی حداکثر ۲۴ ساعت و فقط تا پایان مدت لایسنس قابل استفاده است. لغو صریح مجوز در اولین بررسی بعدی اعمال می‌شود. انقضای مجوز، تونل‌های متعلق به این نصب را متوقف می‌کند؛ دسترسی SSH و پنل کاربران حفظ می‌شوند.

### پشتیبان‌گیری و حذف

از مسیرهای زیر به‌صورت امن پشتیبان تهیه کنید. این مسیرها حاوی اطلاعات خصوصی هستند:

- `/etc/ultrafull-commercial`
- `/var/lib/ultrafull-commercial`
- `/opt/ultrafull-commercial/manager/data`

توکن‌ها، کلیدهای خصوصی و کانفیگ کاربران را در گیت‌هاب منتشر نکنید.

گزینهٔ **Delete all tunnels** موارد قابل حذف را نمایش می‌دهد و تونل‌های متعلق به ULTRAFULL را روی همان سرور حذف می‌کند. برای حذف هر دو سمت، این گزینه را روی هر دو سرور اجرا و سپس جفت را از ultrasource حذف کنید. پنل کاربران و سرویس‌های نامرتبط حذف نمی‌شوند.

تغییر مسیر ممکن است به اتصال مجدد نشست TCP مشتری نیاز داشته باشد؛ انتقال بدون وقفهٔ نشست موجود تضمین نمی‌شود. رمزنگاری ترافیک و سقف‌های سرعت به دلیل لایسنس کاهش داده نمی‌شوند.

مجوز اجزای متن‌باز و سورس لازم آن‌ها در بسته حفظ شده است. جزئیات در [فهرست مجوزها](THIRD_PARTY.md) قرار دارد. لایسنس ULTRAFULL جایگزین مجوز مستقل Backhaul Premium نیست.

</div>


## Spoof setup

Install Spoof on Iran first. Choose whether the Iran VM is on your own hypervisor. If yes, enter its VM ID and complete pinned administrative SSH setup once; later requests use a forced-command key restricted to that VM and MAC. If rented, no hypervisor is contacted. Enter the real peer IP, two tested public spoof identities and customer ports; the private pairing bundle supplies the matching foreign configuration. Select Spoof / Foreign and paste that bundle on the peer. Register both nodes with the existing Telegram source bot for route, health and restart controls.

Only scoped rules owned by this feature are managed. Service-active alone is not a successful spoof test: health requires a source-bound TUN exchange and both expected outer source identities. Provider filtering cannot be removed from inside a rented VPS, and current success does not guarantee connectivity during a future shutdown. Keep the pairing bundle private: it contains the shared encryption key.

## Owned VM registration through a main server (0.2.6)

Select **Main server / automatic VM registration** for an owned Iran VM. Leave the VM ID as `auto` and paste the reusable private `UFSMMAIN1` connection profile from your main administrator. The VM creates its own private key and requests enrollment over fingerprint-pinned TLS. The main server accepts only requests whose TCP source IPv4 matches the requested local IP, resolves that IP in the mother Virtualizor inventory and checks the VM MAC. It authorizes a source-IP/VM/MAC-restricted SSH key, then relays only narrow firewall RPC operations. The private registration profile authorizes enrollment and must not be published or shared with customers. No administrative mother key is installed on the VM. Rented VPSs skip this workflow.

The main administrator configures the relay once with `sudo python3 /opt/ultrafull/app/spoof_main_setup.py`; verified mother SSH access must already exist on that main server. `sudo ultrafull-spoof-enroll` prints its reusable private connection profile. The default enrollment endpoint uses TCP55125; runtime relay uses the main server SSH port. These management paths must be reachable inside Iran. Registration fails closed if the inventory, identity, key or TLS fingerprint cannot be verified. Existing direct VM-scoped mother registrations remain supported.
