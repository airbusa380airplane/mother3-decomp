# Contributing

Keep changes focused on one function, data region, tool, or documented finding.

- Put reconstructed source in `src/` and shared headers in `include/`.
- Record ROM offsets or addresses with the target revision and supporting evidence.
- Mark uncertain names, types, and interpretations explicitly.
- Keep generated files in `build/`; describe how to reproduce them.
- Preserve attribution and existing license notices for imported code.
- Do not include ROMs or raw extracted assets in commits.

Once a build exists, include the build command, toolchain version, and comparison result with each code change. Distinguish byte-identical output from instruction matching and functional equivalence. Until then, state clearly when a change has not been compiled or verified.

Use pull requests for subsequent changes, with a short explanation of the purpose and validation performed.
