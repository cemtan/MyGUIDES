#### REDHAT
---
---



##### Gx00 & G1x00 on RHEL6
---
```bash
defaults {
    polling_interval 60
    find_multipaths yes
    user_friendly_names yes
}
device {
        vendor            "HITACHI"
        product            "OPEN-.*"
        getuid_callout        "/sbin/scsi_id -g -u -s /block/%n"
        path_grouping_policy    multibus
        path_checker        tur
        path_selector        "round robin 0"
        hardware_handler    "0"
        failback            5
        features            "0"
        rr_min_io_rq          1000
        no_path_retry        5
}
blacklist {
# Blacklist devices not capable of multipathing. This includes most internal disks and disks served by single raidcontrollers
               protocol "scsi:unspec"
               protocol "scsi:ata"
               protocol "undef"
}
```
```bash
defaults {
               polling_interval 5
               max_polling_interval 15
               path_checker tur
               failback manual
               marginal_pathgroups yes
               fast_io_fail_tmo 3
               dev_loss_tmo 3600
               detect_checker no
               detect_prio yes
               prio path_latency
               prio_args "io_num=50 base_num=5"
               verbosity 2
}

devices {
               device {
                              vendor "(HITACHI|HP)"
                              product "^OPEN-"
                              path_grouping_policy "multibus"
                              path_selector "service-time 0"
               }
}      
```
```
device {
            vendor "HITACHI"
            product "OPEN-V"
            path_grouping_policy multibus
            path_selector "round-robin 0"
            failback immediate
            features "0"
            no_path_retry fail
            rr_weight uniform
        }

```