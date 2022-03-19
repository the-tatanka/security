# Fuzzing

Because the effort of fuzzing is sometimes enormous, it usually makes sense to perform fuzzing in a separate workflow.

Usually, the input or payload must comply with a certain format or protocol. Lists can help here:

- https://github.com/danielmiessler/SecLists

- https://wordlists.assetnote.io/

- https://github.com/fuzzdb-project/fuzzdb

- https://github.com/swisskyrepo/PayloadsAllTheThings

- https://www.kali.org/tools/seclists/

Whatever the target is, the attack vectors are within it’s I/O.

Tools:

- https://github.com/xmendez/wfuzz

- https://github.com/google/AFL

- https://pypi.org/project/APIFuzzer/
