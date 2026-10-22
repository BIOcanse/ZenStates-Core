# Resource Manager local changes

This vendored GPL-3.0 ZenStates-Core copy is modified for Resource Manager HardwareBridge. Upstream provenance remains in `.gitmodules` and the original LICENSE/README. The original source license is unchanged.

The bridge uses the existing PawnIO AMD/SMU/SMBus/LPC path exclusively. Cpu no longer constructs IODriver; unused raw PCI-port, AOD/APOB, physical-memory AGESA and MMIO diagnostics are omitted. These diagnostics are not bridge protocol operations. PowerTable initialization remains independent and uses RyzenSmu. No replacement raw-memory driver or fallback is added.

The dependency PostBuild and application publish recipe no longer copy InpOut or WinIo binaries. Other applications' installed services, drivers and ACLs are never changed. The unmodified historical driver source remains upstream material, not a product initialization path.

Validation must compile/publish the bridge without launching hardware actions and inspect the actual publication file list. Source edits alone do not establish hardware qualification.
