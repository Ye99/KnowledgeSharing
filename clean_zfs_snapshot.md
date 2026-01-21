# ZFS Snapshot Space Cleanup

## What you learned
- `zfs list` shows pool usage that includes snapshots, while tools like `ncdu /` only see live data.
- Large snapshot usage can make `rpool` look much larger than the filesystem scan.
- Deleting the right snapshots can reclaim large amounts of space quickly.

## Find where snapshots consume space

Check snapshot usage totals by dataset:

```bash
zfs list -o name,used,avail,refer,usedbysnapshots -r rpool
```

Look for datasets with large `usedbysnapshots`.

## Identify which snapshots are large

List snapshots for the dataset that has high snapshot usage:

```bash
zfs list -t snapshot -o name,used,refer -r rpool/ROOT/ubuntu_lr1yvv/var/lib
```

Sort by size if desired (example):

```bash
zfs list -t snapshot -o name,used,refer -r rpool | sort -h -k2
```

## Delete large snapshots to reclaim space

Destroy the specific snapshots you want to remove:

```bash
sudo zfs destroy "pool/dataset@snapshot-name"
```

Example from this cleanup:

```bash
sudo zfs destroy "rpool/ROOT/ubuntu_lr1yvv/var/lib@process-photo-vm_works"
```

## Verify reclaimed space

```bash
zfs list -o name,used,avail,refer,usedbysnapshots -r rpool
```
