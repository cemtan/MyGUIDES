#### WHEN TEARDOWN HANGS
---
---



**Actionplan:**

**1. Verify the replication you are trying to teardown has id {e06bc2c7-64e1-424d-bff8-2eceb0234997}**
Click the replication in the GUI. The end of the URL should match {e06bc2c7-64e1-424d-bff8-2eceb0234997}
See attached screenshot as an example.
If these do not match, stop the action plan and let me know.


**2. Stop Protector ISM node on hitachiprobe05 (make sure no jobs are in progress first):**
```bash
/opt/hitachi/protector/bin/diagdata --stop all
```

**3. Delete following file on hitachiprobe05.**
```bash
cd  /Protector-META/ISMMetadata/blobdata/HitachiVirtualStoragePlatform/540201/{e06bc2c7-64e1-424d-bff8-2eceb0234997}
rm state.xml
```

**4. Purge existing logs**
```bash
/opt/hitachi/protector/bin/diagdata -p
```

**5. Enable tracedebug**
```bash
/opt/hitachi/protector/bin/diagdata -t tracedebug
```

**6. Start Protector ISM**
```bash
/opt/hitachi/protector/bin/diagdata --start all
```

**7. If all snapshots have been removed outside protector, dissociate the pairs instead of teardown. If snapshots have not been removed, do teardown again.**

**8. If dissociate/teardown is successful, set trace level back again. This is important, if left at tracedebug it might fill the disk of your system.**
```bash
/opt/hitachi/protector/bin/diagdata -t tracealways
```
(respond yes to set for active services).
If it is not successful collect another set of logs from the ISM node.
