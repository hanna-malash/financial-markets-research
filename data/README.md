# Data directory

Git stores contracts, manifests, small reviewed samples, and deterministic fixtures. Complete raw and analytical datasets remain outside version control.

```text
data/
  raw/          source downloads and responses, ignored
  interim/      temporary normalized data, ignored
  processed/    complete analytical datasets, ignored
  sample/       small reviewed examples eligible for commit
```

Every committed sample must document:

- source and retrieval method;
- license and redistribution permission;
- UTC time range;
- schema version;
- row count and checksum;
- sanitization performed;
- known gaps and limitations;
- command that regenerates or validates the sample.

Sanitized Schurfer exports require explicit review before publication. Samples must not contain credentials, private configuration, unrestricted context objects, private communication content, direct production access details, or data with unknown redistribution rights.
