# French Revolutionary Decimal Clock — architecture and source boundaries

## Current source ownership

The initial extraction retained provider `decimal` and its reachable implementation from Common 1.19.78. That source-copy arrangement is historical. The current independent module owns its clock-specific arithmetic, formatting and callback implementation in `src/provider.c`; Common owns public ABI declarations, generic time arithmetic, checked loading and shared support. Builds compile the pinned Common static archive rather than maintain copied Common headers or helper implementations. The original Common temporal source is retained.

## Native resource contract

- **Stable identity:** `decimal`, kind `clock`, canonical name `French Revolutionary Decimal Clock`.
- **Metadata entry:** `infiltratr_temporal_resource_metadata_v1`.
- **Executable entry:** `infiltratr_temporal_clock_provider_v1`.
- **Canonical file:** `French Revolutionary Decimal Clock.so`.
- **Native directory:** `/System/Resources/Clocks`.
- **Consumers:** Common's validated loader, used by Calendar, Files, System Settings and other compatible clients.
- **Failure boundaries:** a missing or invalid provider is unavailable; it must not be replaced by a different provider or alter canonical instants.
- **Trust boundary:** native module code executes in the loading process and is intended for trusted installed resources.

## Build and packaging

The local `Makefile` builds one canonically named shared object with hidden helper symbols and two versioned native entry points. It requires the exact Common submodule specified by `COMMON_REVISION` and `COMMON_VERSION`. Independent packaging owns only this resource and its PDF, with the upgrade and release contract defined in [PACKAGING.md](PACKAGING.md).

See [provenance](../SOURCE.md), [file manifest](../SOURCE-MANIFEST.json), [model](MODEL.md) and [validation](VALIDATION.md).

## Implementation language

The clock provider and its arithmetic remain ISO C11: they require no C++ runtime, exception machinery or dynamic resource ownership. C exports preserve the exact Common temporal ABI. Builds link against the authoritative Common gitlink recorded in the current Makefile and must pass the native output, accuracy, package and upgrade checks before publication.
