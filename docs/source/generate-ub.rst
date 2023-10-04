Generate UB programs
=====================

This doc is a guide to generate UB programs with UBFuzz, MUSIC, and Csmith-NoSafe.
Seeds used by these approaches are located in ``/artifact/eval-generator/seeds/``.

UBFuzz
--------

To generate UB programs, execute

.. code-block:: console

  $ cd /artifact/eval-generator/UBFuzz/
  $ ./generate_ub.py --cpu 32

.. note::

   With 32 cores (``--cpu 32``), the script takes roughly 2 to 3 hours to finish.

The generated UB programs will be in ``./mutants/``. To analyze the result, execute

.. code-block:: console

  $ ./analyze_ub.py

MUSIC
--------

To generate UB programs, execute

.. code-block:: console

  $ cd /artifact/eval-generator/MUSIC/
  $ ./generate_ub.py --cpu 32

.. note::

   With 32 cores (``--cpu 32``), the script takes roughly 1 to 2 hours to finish.

The generated UB programs will be in ``./mutants/``. To analyze the result, execute

.. code-block:: console

  $ ./analyze_ub.py

Csmith-NoSafe
--------

To generate UB programs, execute

.. code-block:: console

  $ cd /artifact/eval-generator/Csmith-NoSafe/
  $ ./generate_ub.py --cpu 32

.. note::

   With 32 cores (``--cpu 32``), the script takes roughly 1 to 2 hours to finish.

The generated UB programs will be in ``./mutants/``. To analyze the result, execute

.. code-block:: console

  $ ./analyze_ub.py

