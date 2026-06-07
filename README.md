# relay-with-minecraft
percobaan sederhana menghidupakn led relay dengan basis phyton dan arduino
# Minecraft Relay Bridge

Proyek iseng yang menghubungkan lever di Minecraft ke modul relay fisik lewat Arduino. Waktu lever di-klik, relay di meja ikut bunyi.

---

## Cara kerjanya
Command block nulis ke chat log setiap kali lever ditekan. Python baca log itu terus-terusan, dan kalau ada teks yang cocok, kirim sinyal ke Arduino via USB.

---

## Yang dibutuhkan

- Arduino (Uno/Nano bebas)
- Modul relay 1-channel 5V
- Kabel jumper
- Minecraft Java Edition
- Python 3

---

## Wiring

| Relay | Arduino |
|-------|---------|
| VCC   | 5V      |
| GND   | GND     |
| IN    | D3      |

---

## Setup

### Arduino

Upload kode ini, terus **tutup Arduino IDE** setelah selesai biar port-nya nggak dikunci.

```cpp
const int pinRelay = 3;

void setup() {
  Serial.begin(9600);
  pinMode(pinRelay, OUTPUT);
  digitalWrite(pinRelay, LOW);
}

void loop() {
  if (Serial.available() > 0) {
    char data = Serial.read();
    if (data == '1') digitalWrite(pinRelay, HIGH);
    else if (data == '0') digitalWrite(pinRelay, LOW);
  }
}
```

### Minecraft

1. Ambil command block: `/give @p command_block`
2. Bikin dua command block:
   - Lever ON → `/say LED ON`
   - Lever OFF (pakai redstone torch inverter) → `/say LED OFF`

### Python

```bash
pip install pyserial
```

Edit bagian ini di script sesuai setup kamu:

```python
PORT = 'COM8'
LOG  = r"C:\path\ke\logs\latest"
```

Jalankan:

```bash
python jembatan_mc.py
```

---

## Catatan

- Kalau relay nggak merespons, cek nomor COM di Device Manager
- Pastikan Minecraft pakai mode Creative + cheats aktif
- File log yang dibaca adalah `logs/latest`, bukan `latest.log`
