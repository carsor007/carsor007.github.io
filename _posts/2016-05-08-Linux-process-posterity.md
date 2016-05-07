---
published: false
---

## Process Posterity

 - A child process, 
- spawned with fork,
- terminated with exit;
- status determined with wait.
- 
- Post-execution,
- child metadata lingers,
- remaining in the process table,
- until its status is known.
- 
- In this state of limbo,
- the child is a zombie,
- if its parent dies,
- the child is an orphan.
- 
- init, pid==1,
- is a foster parent;On the
- adopting orphaned processes,
- their ppid becomes 1.
- 
- init is a reaper,
- freeing process table slots;
- calling wait periodically,
- may its zombies R.I.P.
