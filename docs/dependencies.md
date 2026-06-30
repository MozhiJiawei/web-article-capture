# Dependencies

The runtime workflow depends on Codex Browser access for rendered webpage inspection.

The bundled validator uses only the Python standard library:

```powershell
python verify_dependencies.py
```

`verify_dependencies.py` compiles the bundled validator and runs its self-test. It does not perform webpage capture.

Network access is task-dependent: real capture work needs access to the target webpages and image assets, while dependency verification does not.
