# Installation

In slm.toml add:
```
FTDI = { git = "JITx-Inc/FTDI", version = "0.3.3" }
```

# FT2232HL
## Component
This is an FTDI [FT2232HL](https://ftdichip.com/wp-content/uploads/2024/05/DS_FT2232H.pdf) component.
```
inst ftdi : FTDI/components/FT2232HL/component()
```

## Application circuit
This is a basic application circuit for the FT2232HL, following the [hardware design guidelines](https://ftdichip.com/wp-content/uploads/2020/08/AN_146_USB_Hardware_Design_Guidelines_for_FTDI_ICs.pdf).
Circuit Includes:
1.  Bypass capacitors, and power filters
2.  Configuration passives
3.  12 MHz Crystal Resonator 
```
  inst debug-if : FTDI/DebugIF/circuit(
    FTDI/components/FT2232HL/FT2232H-MPSSE,
    FTDI/components/FT2232HL/FT2232H-RS232,
    R-query = R-query,
    C-query = C-query
    )
```
### Ports
```
    port VDD-3v3 : power
    port VDD-USB : power
    port USB : usb-data
    port CFG : microwire-4()
```

Supports:
Require `uart-fc()`, `jtag`, and/or `gpio` from the child `debug-if.ftdi`.

### Parameters
- mode-a:FT2232H-Mode  Set the communication mode for Channel A
- mode-b:FT2232H-Mode  Set the communication mode for Channel B

-- 

- use-internal-1v8:True|False = true  Use the internal regulator to provide 1.8V to VCORE from VREGOUT
- self-powered-reset:True|False = true  Reset for FTDI on Cable Connect
- R-query:ResistorQuery = ?  
- C-query:CapacitorQuery = ?
