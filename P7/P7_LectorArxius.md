# **Pràctica 7: Lector d'arxius**
## **7.1 Codi**
````c++
#include "Arduino.h"
#include "FS.h"
#include "SPIFFS.h"
#include "SD.h"
#include "AudioGeneratorAAC.h"
#include "AudioOutputI2S.h"
#include "AudioFileSourcePROGMEM.h"
#include "sampleaac.h"

AudioFileSourcePROGMEM *in;
AudioGeneratorAAC *aac;
AudioOutputI2S *out;


void setup(){
  Serial.begin(115200);

  in = new AudioFileSourcePROGMEM(sampleaac, sizeof(sampleaac));
  aac = new AudioGeneratorAAC();
  out = new AudioOutputI2S();
  out -> SetGain(0.125);
  out -> SetPinout(26,25,22);
  aac->begin(in, out);
}

void loop(){
  if (aac->isRunning()) {
    aac->loop();
  } else {
    aac -> stop();
    Serial.printf("Sound Generator\n");
    delay(1000);
  }
}

````

## **7.2 Funcionament**
S'hi afegeixen les capçaleres corresponents que són bàsicament llibreries d'àudio. També es defineixen 3 punters, un que servirà per llegir el fitxer d'àudio, el segon servirà per descodificar el fitxer i el darrer servirà per enviar de manera analògica l'àudio

A l'apartat de setup s'inicialitza un punter in amb els paràmetres de la mostra del fitxer d'àudio aac i la mida. Un altre punter aac que serà l'àudio codificat a aac i l'out que serà l'àudio de sortida. Se li inicialitza amb un guany de 0.125 i s'inicialitzen els pins

Al loop es comprovarà que si l'aac s'executi fins a estar descodificada. Quan acabi el procés es pararà i es generarà el so amb el missatge que "Sound Generator"