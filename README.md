# Modbus Master for RTU Transmission Mode

### Parameters
* **BAUD_DIV_DEF** - the default divider of `clk` frequency.
	The equality must be satisfied (BAUD_DIV_DEF must be more or equal 2):
```
		clk frequency [Hz] / BAUD_DIV_DEF = 19200.
```		

* **BAUD_DIV_OPT1** and **BAUD_DIV_OPT2** - the optional dividers of `clk` frequency.
	Use for changing exchange speed via _Configuration register_.
	The equality must be satisfied  (BAUD_DIV_OPT1(2) must be more or equal 1):
```
		clk frequency [Hz] / BAUD_DIV_OPT1(2) = 57600 or 115200 or other standard values.
```			

* **CONFIG_DEFAULT** - the default state of _Configuration register_.

* **ADDR_DEFAULT** - the default state of _Slave address register_.

* **DE_TIME** - Driver Enable time: the RS-485 transmitter enable time in `clk` periods.

* **RESPONSE_FACTOR** - Response time-out expansion factor.
```
	Response time-out = 500 ms * (RESPONSE_FACTOR + 1).
```

* **TURNAROUND_FACTOR** - Turnaround delay expansion factor.
```
	Turnaround delay = 10 ms * (TURNAROUND_FACTOR + 1).
```			


### Signals description
| Signal | Description
| :------ | -----------
| `clk` 				| Clock.
| `rst` 				| Asynchronous reset.
| `cs_...` 			| Avalon MM Slave CS Interface.
| `pdu_...` 			| Avalon MM Slave PDU Interface.
| `parity_err` 		| Parity error one-clock pulse.
| `start_bit_err`	| Start bit error one-clock pulse.
| `stop_bit_err` 	| Stop bit error one-clock pulse.
| `addr_err` 		| Response frame slave address error one-clock pulse.
| `crc_err` 			| Frame CRC error one-clock pulse.
| `control_pdu` 		| Response PDU status. Sets when ready to process response PDU. Resets after _Control and status register_ is written.
| `ready` 			| Ready status signal.
| `rxd` 				| Serial line input.
| `txd` 				| Serial line output.
| `oe` 				| Serial line output enable.


###	CS Interface Address Space

| `cs_addr` | Description
| --------- | -----------
| 0 	    | **PDU size register** [R/W] <br/> Valid values: 1,..., 253
| 1 	    | **Configuration register** [R/W] <br/> [1 : 0] - Modbus RTU Transmission baud rate code: <ul><li> 0 - 9.6 kbps </li><li> 1 - 19.2 kbps </li><li> 2 - defined by BAUD_DIV_OPT1 </li><li> 3 - defined by BAUD_DIV_OPT2 </li></ul> <br/> [10 : 8] - Modbus RTU Transmission Mode <ul><li> [8] - Parity ena: 0 - disabled, 1 - enabled </li><li> [9] - Parity mode: 0 - Even, 1 - Odd </li><li> [10] - Stop bits: 0 - one, 1 - two </li></ul>
| 2 	    | **Slave address register** [R/W] <br/> Valid values: 1,..., 247.
| 3 	    | **Control and status register** [R/W] <br/> Write any value: request to send <br/> Read: <br/> <ul><li> [0] - Status of the response PDU: 1 - ready to process response PDU </li><li> [1] - Master status: 1 - ready to send new request </li></ul>



###	PDU Interface Address Space

	Each Avalon MM write operation store 4 consequent bytes:
| `pdu_addr` | PDU bytes
| ---------- | ---------
| 0 	     | 0...3,
| 1 	     | 4...7,
| ...	     |
| 63 	     | 252...253 plus two dummy bytes.



### Usage

To use the module you must have external MCU to handle with it. MCU software have to implement application layer of Modbus protocol.
For example transaction, see [Typical transaction timing diagram](https://github.com/vasilydenisenko/modbus_rtu_master/tree/main/docs/modbus.png).

If you are looking for a Modbus RTU Slave module, have a look at [this implementation](https://github.com/vasilydenisenko/modbus_rtu_slave).


### Contacts

Report an issue: <https://github.com/vasilydenisenko/modbus_rtu_master/issues>

Ask a question: dancingcroissant@inbox.ru
