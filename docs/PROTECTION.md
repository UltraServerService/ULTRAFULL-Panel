# Protected runtime and updates — 0.2.2

ULTRAFULL uses AES-256-GCM authenticated envelopes around the registered Ultra
executables. Each new build uses independent random keys for each executable.
Decryption material is no longer passed through Go linker flags, which are
recorded in executable build information. Splitting generated key material is
obfuscation, not a cryptographic boundary against a server administrator.

The supervisor authenticates its assigned payload and selects an immutable,
SHA-256-addressed companion. An incomplete update cannot redirect an existing
supervisor to another generation's encrypted file. Old companions are retained
for rollback. Downloaded releases continue to require an Ed25519-signed release
record, a matching size and checksum, and a valid IP-bound lease.

Plaintext payloads execute from a sealed anonymous Linux memory file rather than
a named temporary executable. Core dumps are disabled for the supervisor and its
child only. No machine-wide debugger, SSH, firewall or core-dump setting is changed.
These controls reduce accidental exposure and casual extraction. They do not
prevent extraction by an administrator controlling the operating system. Memory
may also be swapped by the OS; no claim of hardware-backed secrecy is made.

The five existing core payloads are unchanged. Two tested TCP adapter images
are added as separately protected assets. TCP and ICMP can be selected for each
registered pair. Licensing does not reduce traffic encryption or configured
upload/download limits.
Existing active tunnels are not automatically restarted by a panel update.
The updated runtime package is used when installing the Ultra runtime.

Original component licenses, notices and corresponding source remain available.
The commercial license covers the ULTRAFULL service and integration; it does not
remove third-party rights or turn an open-source component into a closed product.

## Installation flow

The official bootstrap opens the ULTRAFULL terminal panel before activation.
Choose **1 — Iran server** or **2 — Foreign server**. On Iran, enter only the
license: its verified IPv4 must already be assigned to the server. On foreign
servers, one available public IPv4 is selected automatically; multiple addresses
are offered as numbered choices. Enter the licensed Iran IPv4 to authorize the
foreign node. Invalid licenses are reported inside the panel with
**@licenseultra_bot**; connectivity failures are reported separately.

## راهنمای کوتاه

تمام مراحل فعال‌سازی، دریافت نسخه و نصب در محیط پنل ULTRAFULL انجام می‌شود.
با انتخاب گزینهٔ ۱، فقط لایسنس ایران وارد می‌شود. پنل IP ثبت‌شده در لایسنس را
بررسی می‌کند و تنها در صورت وجود آن روی سرور، فعال‌سازی را ادامه می‌دهد.
برای سرور خارج، IP محلی به‌صورت خودکار یا از فهرست شماره‌دار انتخاب می‌شود؛
سپس IP ایرانِ دارای لایسنس وارد می‌شود. لایسنس نامعتبر و خطای ارتباط با سرویس
لایسنس، پیام‌های جداگانه دارند.

محافظت جدید استخراج ساده و دست‌کاری فایل‌ها را دشوارتر می‌کند، اما در برابر
مالک سروری که دسترسی کامل root دارد، تضمین غیرقابل‌استخراج بودن وجود ندارد.
آپدیت پنل باعث ری‌استارت خودکار تونل‌های فعال نمی‌شود.
