FROM tiredofit/alpine-arm:edge

RUN apk update && \
    apk add \
        build-base \
        git \
        linux-headers

RUN mkdir -p /usr/src/wiringpi && \
git clone https://github.com/WiringPi/WiringPi /usr/src/wiringpi

RUN cd /usr/src/wiringpi && \

sed -i -e "s|/usr/local/bin/gpio|/usr/bin/gpio|" wiringPi/wiringPi.c && \
sed -i -e "/sys\/ioctl/i#include <asm/ioctl.h>" wiringPi/wiringPiSPI.c && \

make -C wiringPi all static  && \
make -C devLib all static INCLUDE+="-I`pwd`/wiringPi" LDFLAGS+="-L`pwd`/wiringPi" && \
make -C gpio INCLUDE+="-I`pwd`/wiringPi -I`pwd`/devLib" LDFLAGS+="-L`pwd`/wiringPi -L`pwd`/devLib" && \
cp wiringPi/*.h /usr/include/ && \
cp wiringPi/libwiringPi.a /usr/lib/ && \
cp wiringPi/libwiringPi.so* /usr/lib/ && \
ln -s /usr/lib/libwiringPi.so* /usr/lib/libwiringPi.so && \
cp devLib/*.h /usr/include/ && \
cp devLib/libwiringPiDev.a /usr/lib/ && \
cp devLib/libwiringPiDev.so* /usr/lib/ && \
ln -s /usr/lib/libwiringPiDev.so* /usr/lib/libwiringPiDev.so && \
cp gpio/gpio /usr/bin/ && \
chmod 4755 /usr/bin/gpio && \
mkdir -p /usr/share/man/man1 && \
cp gpio/gpio.1 /usr/share/man/man1/
