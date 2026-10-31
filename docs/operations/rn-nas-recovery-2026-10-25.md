# RN NAS Recovery and Validation

## October 25-31, 2026

### System

- Host: redundant-net
- NAS mount: /mnt/rn-nas
- Storage partition: /dev/sdd1
- Filesystem: NTFS3
- Storage capacity: approximately 3 TB

## Incident

RN's external NAS reported an NTFS dirty-flag warning.

The issue was investigated and corrected on October 25, 2026.

## Repair

The NTFS filesystem was checked using ntfsfix.

The repair process included:

1. Filesystem inspection using ntfsfix.
2. Successful clearing of the NTFS dirty flag.
3. Removal of the previously configured forced NTFS mount option from /etc/fstab.
4. Remounting through the Linux NTFS3 driver.
5. Validation of filesystem and Samba accessibility.

The dirty-flag issue was resolved on October 25. Subsequent troubleshooting did not establish a recurrence.

## Validation

After the repair:

- /dev/sdd1 was mounted at /mnt/rn-nas.
- NTFS3 was operating in read-write mode.
- Samba access was verified.
- Storage remained accessible for RN's backup NAS role.

On October 31, the RN health check confirmed:

- smbd: active
- /mnt/rn-nas: mounted

## Separate Storage Observations

USB storage reset events were observed on October 27-29.

The exact cause was not established.

SMART information for the external Toshiba NAS disk reported a passing overall health assessment and no current reallocated or pending sectors.

Separate performance measurements showed substantial I/O wait associated with RN's internal system HDD, rather than the external NAS disk, during the observed samples.

These findings do not establish that the USB reset issue has been resolved.

## Result

The October 25 NTFS dirty-flag problem was repaired, normal NTFS3 mounting was restored, and RN's Samba NAS service was validated.

The unrelated USB reset events remain an unresolved observation and are not represented here as repaired.
