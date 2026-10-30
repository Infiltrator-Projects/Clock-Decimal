# French Revolutionary Decimal Clock — architecture and source boundaries

## Current source ownership

This repository now contains a local, independently buildable **copy** of provider `decimal`. `src/provider.c` contains the selected native ABI adapter. The other files under `src/` contain only reached supporting implementation definitions; `include/` contains the required Common ABI headers and this provider's build configuration.

The original implementation, combined build and installed package remain in Common. Nothing is deleted from Common, and no current package ownership is changed.

## Copied shared code

Required shared helpers are copied locally, including arithmetic, date coordinates, localization or ICU bridging where this provider uses them. Multiple provider repositories intentionally contain their own copies of these helpers. Combined implementation files were selected and reduced to this provider's reachable operations and data; unrelated executable provider implementations are excluded.

Public ABI declarations preserve compatibility with existing consumers. Private selector numeric values are retained where needed, while the other private selectors are removed. No other provider is registered or exported by this module.

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

The local `Makefile` builds one canonically named shared object directly from this repository, with hidden helper symbols and only the two versioned native entry points exported. It requires no Common checkout, Common archive, sibling resource repository or source download.

The existing Common resource package continues to own the installed path. An independent installer, package handover, release workflow and published release are separate work. This source copy does not claim those steps are complete.

See [provenance](../SOURCE.md), [file manifest](../SOURCE-MANIFEST.json), [model](MODEL.md) and [validation](VALIDATION.md).

## Implementation language

The clock provider and its arithmetic remain ISO C11: they require no C++ runtime, exception machinery or dynamic resource ownership. C exports preserve the exact Common temporal ABI. Builds link against the pinned Common gitlink (90c5b2c86afb7158e91794231ed970197aaf1e04) and must pass the native output, accuracy, package and upgrade checks before publication.
