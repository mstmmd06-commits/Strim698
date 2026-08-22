# استفاده از ایمیج پایه‌ی مرورگر به عنوان پایه (چون همه چیز رو آماده داره)
FROM ghcr.io/m1k1o/neko/firefox:latest

USER root

# نصب وابستگی‌های مورد نیاز برای اوپن‌کد دسکتاپ
RUN apt-get update && apt-get install -y --no-install-recommends \
    wget \
    curl \
    libgtk-3-0 \
    libnotify4 \
    libnss3 \
    libxss1 \
    libxtst6 \
    xdg-utils \
    libatspi2.0-0 \
    libdrm2 \
    libgbm1 \
    libxcb-dri3-0 \
    libsecret-1-0 \
    fonts-liberation \
    && rm -rf /var/lib/apt/lists/*

# دانلود اوپن‌کد دسکتاپ (نسخه لینوکس)
RUN wget -O /tmp/opencode-desktop.AppImage \
    "https://github.com/sst/opencode/releases/download/v1.0.223/opencode-desktop-linux-x86_64.AppImage" && \
    chmod +x /tmp/opencode-desktop.AppImage && \
    mv /tmp/opencode-desktop.AppImage /usr/local/bin/opencode-desktop

# اسکریپت استارت سفارشی
RUN echo '#!/bin/bash\n\
set -e\n\
\n\
# تنظیم متغیرهای محیطی برای اوپن‌کد دسکتاپ (اگه لازم باشه)\n\
export DISPLAY=:99.0\n\
\n\
# استارت اوپن‌کد دسکتاپ در پس‌زمینه (بدون سندباکس چون تو کانتینر هستیم)\n\
opencode-desktop --no-sandbox --disable-gpu --disable-software-rasterizer &\n\
\n\
# استارت مرورگر پیش‌فرض (برای رابط کاربری وب)\n\
# اگه فقط اوپن‌کد رو می‌خوای، این خط رو کامنت کن یا تغییرش بده\n\
exec /usr/bin/supervisord -c /etc/neko/supervisord.conf\n\
' > /usr/local/bin/start-custom.sh && \
    chmod +x /usr/local/bin/start-custom.sh

CMD ["/usr/local/bin/start-custom.sh"]
