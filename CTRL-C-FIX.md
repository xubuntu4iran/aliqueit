# Ctrl+C handling (Linux / POSIX)

Build using `cd src && make -j$(nproc)` (GMP development headers and libraries required).

The modified `src/aliqueit.cc` installs a SIGINT handler and uses a POSIX fork/exec process-group wrapper instead of `system()` for external commands. Pressing Ctrl+C sends SIGINT to the launched command's process group, waits up to approximately 2 seconds for the shell to exit, then sends SIGTERM if necessary. Aliqueit exits with status 130 without advancing the sequence.

Notes: A process that deliberately detaches into a different process group/session cannot be controlled by this wrapper. If a child shell exits before its descendants, those descendants still receive SIGINT, but their checkpoint completion is not guaranteed. This patch has been compilation-tested, not end-to-end tested with YAFU/Msieve/ECM.
