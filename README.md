# configdrive_mount_helper

Ansible role that installs a custom mount helper that mounts a CephFS using credentials given via a config drive.
(In the future, types other than CephFS might be supported.)

Using this mount helper, one can simply run

```bash
mount -t configdrive <ID> /mnt -o _netdev
```

to mount a CephFS under `/mnt`.
To make this work, the credentials must be passed as a block device under `/dev/disk/by-id/<ID>`.
That block device must contain a filesystem with the following files on the root level:

| Filename          | Content                                                                   |
| ----------------- | ------------------------------------------------------------------------- |
| `type`            | Must contain the string `ceph`.                                           |
| `spec`            | Mount source, e.g., `mons.ceph.example.com:/volumes/csi/csi-vol-foo/bar`. |
| `fs`              | Name of the CephFS. This is passed to `-o fs=`.                           |
| `name`            | Name of the Ceph client. This is passed to `-o name=`.                    |
| `secretfile.file` | Secret of the Ceph client. This file is passed to `-o secretfile=`.       |

## Requirements

This role should work in pretty much any distribution that has an `/sbin` directory.

## License

This work is licensed under the [MIT License](./LICENSE).
