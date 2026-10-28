# Resource Manager local changes

This vendored GPL-3.0 ZenStates-Core copy is modified for Resource Manager HardwareBridge. Upstream provenance remains in `.gitmodules` and the original LICENSE/README. The original source license is unchanged.

The bridge uses the existing PawnIO AMD/SMU/SMBus/LPC path exclusively. Cpu no longer constructs IODriver; unused raw PCI-port, AOD/APOB, physical-memory AGESA and MMIO diagnostics are omitted. These diagnostics are not bridge protocol operations. PowerTable initialization remains independent and uses RyzenSmu. No replacement raw-memory driver or fallback is added.

The dependency PostBuild and application publish recipe no longer copy InpOut or WinIo binaries. Other applications' installed services, drivers and ACLs are never changed. The unmodified historical driver source remains upstream material, not a product initialization path.

Validation must compile/publish the bridge without launching hardware actions and inspect the actual publication file list. Source edits alone do not establish hardware qualification.

## HardwareBridge 0.2.1: PM-table exception storm

- `Hardware/PowerTable.cs`: an absent MMIO singleton leaves BCLK unknown and applies no correction. Missing SMU, missing DRAM base address and an unsupported table definition are retained as `UnavailableKind` facts instead of exceptions. Only a table that has not updated is retried; a thrown refresh failure returns once.
- `Hardware/Mmio.cs`: `GetBclk` returns null when no physical-memory backend exists. It does not construct a driver or invent a clock value.
- `Cpu.cs`: omit the duplicate initial refresh; the PowerTable constructor already initializes it.

The bridge retains PM-table capability facts per session. Repeated describe operations do not refresh the table; `refresh-power-table` explicitly re-establishes availability. Offline tests in `tests/PowerTableHarness` open no hardware.


## HardwareBridge: diagnostic resource initialization

Cpu now honors Timings.Enabled for SMBus/MemoryConfig and Wmi.Enabled or Sensors.Enabled for SystemInfo. Null flags preserve enabled behavior. The bridge explicitly disables generic AMD MSR/SMN diagnostics with IoModule.Enabled=false and disables unused Timings/Aod/Wmi/Sensors diagnostics while retaining RyzenSMU/PowerTable and the PCI mutex. Disabled generic I/O omits AMD-family module loading, topology/patch/SVI/HSMP initialization, and ISA/SMBus mutex creation. The startup CPUID buffer is released after initialization in this profile; Opcode.Close is idempotent for later disposal. The wholly unused direct LPC module is removed. Default enabled diagnostic initialization order and error handling are retained. No driver installation, fallback, setting restoration, or protocol change is added.
