# ansiktsgjenkjenning-raspberrypi
 Enklast mogleg kom i gang med ansiktsgjenkjenning på Raspberry PI.

## Stegvis gjennomgang:
1. Oppdater Raspberry PI: 
```
sudo apt -y update 
&& sudo apt -y full-upgrade
```
2. Aktiver kamera: Sjå "settings" frå hovudmenyen.
3. Kontroller at kamera fungerer.
```
raspistill -o testbilete.jpg
```
4. Installer avhengingheter ('dependencies'):
```
sudo apt install build-essential \
    cmake \
    gfortran \
    git \
    wget \
    curl \
    graphicsmagick \
    libgraphicsmagick1-dev \
    libatlas-base-dev \
    libavcodec-dev \
    libavformat-dev \
    libboost-all-dev \
    libgtk2.0-dev \
    libjpeg-dev \
    liblapack-dev \
    libswscale-dev \
    pkg-config \
    python3-dev \
    python3-numpy \
    python3-pip \
    zip
    python3-picamera
```
5. Oppdater:
```
sudo pip3 install --upgrade picamera[array]
```
6. Auk størrelsen på swap-fila slik at det går an å byggje dlib:
```
sudo nano /etc/dphys-swapfile
```
Finn `CONF_SWAPSIZE` og endre verdien frå 100 til 1024. Lagre, gå ut og køyr:
```
sudo /etc/init.d/dphys-swapfile restart
```
7. Bygg og installer dlib (NB: Ca. 30 minutt på Raspberry PI 4):
```
cd
git clone -b 'v19.6' --single-branch https://github.com/davisking/dlib.git
cd ./dlib
sudo python3 setup.py install --compiler-flags "-mfpu=neon"
```
8. Juster swap-fila tilbake:
```
sudo nano /etc/dphys-swapfile
```
Finn `CONF_SWAPSIZE` og endre verdien frå 1024 til 100. Lagre, gå ut og køyr:
```
sudo /etc/init.d/dphys-swapfile restart
```
9. Installer `face_recognition` og eksempler:
```
sudo pip3 install face_recognition
```
10. Køyr eit enkelt eksempel:
```
cd ./face_recognition/examples
python3 facerec_on_raspberry_pi.py
```
Sjå om 'kamera ser deg'. Det skal stå "Unknown", om du ikkje er Obama. Finn eit bilete av Obama på mobilen og vis dette til kameraet. Det skal no stå 'I see someone named Barack Obama!'
11. Treningstid! 
>   The final step is to start recognising your own faces. Create a directory and, in it, place some good-quality passport-style photos of yourself or those you want to recognise. You can then edit the `facerec_on_raspberry_pi.py` script to use those files instead. You’ve now got a robust prototype of face recognition. This is just the beginning. These libraries can also identify ‘generic’ faces, meaning it can detect whether a person is there or not, and identify features such as the eyes, nose, and mouth. There’s a world of possibilities available, starting with these simple scripts. Have fun.

## Problemsøking:
- Dersom du ikkje har eksempelfilene kan desse lastast ned direkte frå:
```
cd
git clone --single-branch https://github.com/
ageitgey/face_recognition.git
```
- Sjølve biblioteket kan du laste ned og lese meir om her hjå @ageitgey sitt repository [face_regocnition](https://github.com/ageitgey/face_recognition)