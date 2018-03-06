# Modbus Library for ESP8266/ESP32

## API

### Add [multiple] regs
```
bool addReg(uint16_t address, uint16_t value = 0, uint16_t numregs = 1)
bool addHreg(uint16_t offset, uint16_t value = 0, uint16_t numregs = 1)
bool addCoil(uint16_t offset, bool value = false, uint16_t numregs = 1)
bool addIsts(uint16_t offset, bool value = false, uint16_t numregs = 1)
bool addIreg(uint16_t offset, uint16_t value = 0, uint16_t nemregs = 1)
```
### Write regs
```
bool Reg(uint16_t address, uint16_t value)
bool Hreg(uint16_t offset, uint16_t value)
bool Coil(uint16_t offset, bool value)
bool Ists(uint16_t offset, bool value)
bool Ireg(uint16_t offset, uint16_t value)
```
### Read regs
```
uint16_t Reg(uint16_t address)
uint16_t Hreg(uint16_t offset)
bool Coil(uint16_t offset)
bool Ists(uint16_t offset)
uint16_t Ireg(uint16_t offset)
```
### Callbacks
`void cbEnable(bool state = TRUE)
Callback generation control. Callback generation is enabled by default.

`void cbDisable()
Disable callback generation.

`void onConnect(cbModbusConnect cb)
Assign callback function on new incoming connection event.

`typedef bool (*cbModbusConnect)(IPAddress ip)
Connect event callback function definition. Client IP address is passed as argument.

`typedef uint16_t (*cbModbus)(TRegister* reg, uint16_t val)

bool onGet(uint16_t address, cbModbus cb = cbDefault, uint16_t numregs = 1)
bool onSet(uint16_t address, cbModbus cb = cbDefault, uint16_t numregs = 1)
bool onGetCoil(uint16_t address, cbModbus cb = cbDefault, uint16_t numregs = 1)
bool onSetCoil(uint16_t address, cbModbus cb = cbDefault, uint16_t numregs = 1)
bool onGetHreg(uint16_t address, cbModbus cb = cbDefault, uint16_t numregs = 1)
bool onSetHreg(uint16_t address, cbModbus cb = cbDefault, uint16_t numregs = 1)
bool onGetIsts(uint16_t address, cbModbus cb = cbDefault, uint16_t numregs = 1)
bool onSetIsts(uint16_t address, cbModbus cb = cbDefault, uint16_t numregs = 1)
bool onGetIreg(uint16_t address, cbModbus cb = cbDefault, uint16_t numregs = 1)
bool onSetIreg(uint16_t address, cbModbus cb = cbDefault, uint16_t numregs = 1)
```
### Macros
```
#define COIL(n)
#define ISTS(n)
#define IREG(n)
#define HREG(n)
#define COIL_VAL(v)
#define COIL_BOOL(v)
#define ISTS_VAL(v)
#define ISTS_BOOL(v)
```
### ModBus IP specific
```
void begin()
void task()
```

### Callback example

```
bool coil = false; // Define external variable to get/set value
uint16_t cbCoilSet(TRegister* reg, uint16_t val) {	// 'reg' is pointer to reg to modify, 'val' is new register value
  coil = COIL_BOOL(val);
  return val;	// Returns value to be saved to TRegister structure
}
uint16_t cbCoilGet(TRegister* reg, uint16_t val) {
  return COIL_VAL(coil);	// Returns value to be returned to ModBus master as reply for current request
}
bool cbConn(IPAddress ip) {
	Serial.println(ip);
	return true;		// Return 'true' to allow connection or 'false' to drop connection
}
ModbusIP mb;	// ModbusIP object
void setup() {
...
  mb.onConnect(cbConn);   // Add callback on connection event
  mb.begin();
  mb.addCoil(COIL_NR);     // Add Coil
  mb.onSet(COIL(COIL_NR), cbCoilSet); // Add callback on Coil COIL_NR value set
  mb.onGet(COIL(COIL_NR), cbCoilGet); // Add callback on Coil COIL_NR value get
...
}
void loop() {
...
	mb.task();
...
}
```


## Contributions

https://github.com/emelianov/modbus-esp8266<br>
a.m.emelianov@gmail.com

Original version:<br>
http://github.com/andresarmento/modbus-esp8266<br>
prof (at) andresarmento (dot) com

## License

The code in this repo is licensed under the BSD New License. See LICENSE.txt for more info.
