FROM makenotes/apline_php74:1.7
#FROM makenotes/apline_php72:1.5

# Install composer and change it's cache home
RUN curl -o /usr/bin/composer https://mirrors.aliyun.com/composer/composer.phar \
    && chmod +x /usr/bin/composer
ENV COMPOSER_HOME=/tmp/composer



# 字体 - 宋体
ADD fonts/simsun.ttc  /usr/share/fonts/simsun.ttc
ADD fonts/simsun.ttf /usr/share/fonts/simsun.ttf
# 安装 wkhtmltopdf
ADD wkhtmltopdf/wkhtmltopdf /usr/bin/wkhtmltopdf
RUN chmod 755 /usr/bin/wkhtmltopdf
# 添加 wkhtmltopdf 启动脚本
ADD wkhtmltopdf/startup /usr/bin/startup
RUN chmod 755 /usr/bin/startup

# php image's www-data user uid & gid are 82, change them to 1000 (primary user)
RUN apk --no-cache add shadow && usermod -u 1000 www-data && groupmod -g 1000 www-data

# GIT 配置
ARG GIT_LOGIN_ACCOUNT=test
ARG GIT_LOGIN_TOKEN=test
ARG GIT_USER_NAME=test
ARG GIT_USER_EMAIL=test@majorbio.com
RUN git config --global user.name "${GIT_USER_NAME}" && \
    git config --global user.email "${GIT_USER_EMAIL}" && \
    git config --global core.fileMode false

RUN mkdir -p /home/www-data && \
    usermod -d /home/www-data www-data && \
    echo "https://${GIT_LOGIN_ACCOUNT}:${GIT_LOGIN_TOKEN}@git.i-sanger.com" > /home/www-data/.git-credentials && \
    echo "[user]\n\tname = ${GIT_USER_NAME}\n\temail = ${GIT_USER_EMAIL}\n[credential]\n\thelper = store" > /home/www-data/.gitconfig && \
    chown -R www-data:www-data /home/www-data && \
    git config --global credential.helper store

WORKDIR /var/www
