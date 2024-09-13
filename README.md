# VSS (Volume Shadow Copy Service) API Wrapper

This project provides a Go wrapper for the Windows Volume Shadow Copy Service (VSS) API. It allows you to create, manage, and delete VSS snapshots programmatically.

## Features

- Initialize and manage VSS snapshots.
- Handle VSS errors with custom error types.
- Support for various VSS contexts and backup types.
- Query and manipulate VSS snapshot properties.
- Asynchronous operations with VSS API.

## Requirements

- Windows operating system.
- Go 1.16 or later.

## Installation

To use this package, you need to install the required dependencies:

```sh
go get github.com/8ugMak1r/go-vss
```

## Usage

### Create a VSS Snapshot

To create a VSS snapshot, use the `NewVssSnapshot` function:

```go
snapshotSet, err := NewVssSnapshot("C:\\", 60, vss.DefaultOption)
if err != nil {
    log.Fatalf("Failed to create VSS snapshot: %v", err)
}

snapshotSet, err := NewVssSnapshots([]string{"C:\\", "D:\\"}, 60, vss.DefaultOption)
if err != nil {
    log.Fatalf("Failed to create VSS snapshot: %v", err)
}

```

###  Expose the snapshot to a specified volume

```go
exposeVolume := "Z:"
err = snapshotSet.Snapshots[0].Expose(exposeVolume)
if err != nil {
    log.Fatalf("Failed to expose VSS snapshot: %v", err)
}
log.Printf("Snapshot exposed at: %s", exposedPath)
```

### Delete VSS SnapshotSet

To delete a VSS snapshot, call the `Delete` method on the `VssSnapshot` or `VssSnapshotSet` object:

```go
err := snapshotSet.Delete()
if err != nil {
    log.Fatalf("Failed to delete VSS snapshot: %v", err)
}
```


## Error Handling

The package provides custom error types for handling VSS errors:

- `vssError`: Represents an error returned from the VSS API.
- `vssTextError`: Represents a textual error message.

Example:

```go
if err != nil {
    if vssErr, ok := err.(*vssError); ok {
        log.Printf("VSS error: %s", vssErr.Error())
    } else {
        log.Printf("Error: %v", err)
    }
}
```

## References

1. [Restic PR #2274 - GitHub](https://github.com/restic/restic/pull/2274)
2. [VSS Volume Snapshot Attributes - Microsoft Docs](https://docs.microsoft.com/en-us/windows/win32/api/vss/ne-vss-vss_volume_snapshot_attributes)
3. [Volume Shadow Snapshot (VSS) Format - libyal](https://github.com/libyal/libvshadow/blob/main/documentation/Volume%20Shadow%20Snapshot%20(VSS)%20format.asciidoc#422-store-attribute-flags)
4. [Volume Shadow Copy BackupComplete and VSS\_E\_BAD\_STATE - Narkive](https://microsoft.public.win32.programmer.kernel.narkive.com/aObDj2dD/volume-shadow-copy-backupcomplete-and-vss-e-bad-state)
5. [Difference between VSS Full Backup and VSS Copy - Microsoft Tech Community](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/What-is-the-difference-between-VSS-Full-Backup-and-VSS-Copy/ba-p/423575)


## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request on GitHub.

