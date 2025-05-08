# Использование пьезодиска в качестве Z-пробника

![Схема подключения](http://3dtoday.ru/upload/main/0aa/0aa029e2c5641ebff3a33ed71cb2e249.png)

Ниже приведён скетч для Arduino. Настраивайте следующие параметры по месту:

1. Размеры массивов (количество усредняемых измерений)
2. Пороговое значение (`threshold`)
3. Интервал между измерениями (`interval`)

```cpp
#include <Average.h>   // https://github.com/MajenkoLibraries/Average

// Усреднение N последних измерений
Average<int> ave1(2);
Average<int> ave2(2);
Average<int> ave3(2);
Average<int> ave(2);   // для XOR-проверки
Average<int> ave4(12); // дополнительный массив для соплового пьезо

#define DEBUG 0 // 1 — включить вывод в Serial для отладки

#ifdef DEBUG
  const int interval = 100; // мкс между замерами пьезо в режиме отладки
#else
  const int interval = 1;
#endif

const int ledPin     = 13;   // светодиод на D13
const int relayPin   = 19;   // выход оптопары на D3
const int piezoPin1  = A1;
const int piezoPin2  = A2;
const int piezoPin3  = A3;
const int piezoPin4  = A4;

const bool useNozzlePiezo = 0; // игнорировать/участвовать 4-й пьезо
const int  threshold      = 8; // порог срабатывания
const int  displayMax     = 20; // максимальное значение для Serial Plotter

int sensorReading1 = 0;
int sensorReading2 = 0;
int sensorReading3 = 0;
int sensorReading4 = 0;

void setup() {
  pinMode(ledPin, OUTPUT);
  pinMode(relayPin, OUTPUT);
  digitalWrite(ledPin, LOW);
  digitalWrite(relayPin, LOW);
  if (DEBUG == 1) {
    Serial.begin(250000); // для Serial Plotter
  }
}

void loop() {
  // Чтение аналоговых входов
  sensorReading1 = analogRead(piezoPin1);
  sensorReading2 = analogRead(piezoPin2);
  sensorReading3 = analogRead(piezoPin3);
  sensorReading4 = useNozzlePiezo ? analogRead(piezoPin4) : 0;

  // Усреднение
  ave1.push(sensorReading1);
  ave2.push(sensorReading2);
  ave3.push(sensorReading3);
  ave4.push(sensorReading4);

  // Простая проверка превышения порога
  if (sensorReading1 >= threshold || sensorReading2 >= threshold || sensorReading3 >= threshold) {
    ave.push(threshold);
  } else {
    ave.push(0);
  }

  #if DEBUG == 1
  // Вывод в Serial для Plotter: фиктивные и реальные данные
  Serial.print(displayMax); Serial.print(",");
  Serial.print(threshold);  Serial.print(",");
  Serial.print(min(sensorReading1, displayMax)); Serial.print(",");
  Serial.print(min(sensorReading2, displayMax)); Serial.print(",");
  Serial.print(min(sensorReading3, displayMax)); Serial.print(",");
  Serial.println(min(sensorReading4, displayMax));
  #endif

  // Логика с учётом усреднения и дополнительной чувствительности сопла
  if (ave1.mean() >= threshold
   || ave2.mean() >= threshold
   || ave3.mean() >= threshold
   || (useNozzlePiezo && ave4.mean() >= threshold * 20)
   || ave.mean()  >= threshold) {
    digitalWrite(ledPin, HIGH);
    digitalWrite(relayPin, HIGH);
    delayMicroseconds(50000); // длительность сигнала 50 мс
  } else {
    digitalWrite(ledPin, LOW);
    digitalWrite(relayPin, LOW);
  }

  delayMicroseconds(interval);
}
```

**Перед тестированием** убедитесь, что контакты пьезодиска надёжно заизолированы от металлических частей принтера и соблюдена полярность подключения.
