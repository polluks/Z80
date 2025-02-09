===============
Version history
===============

v0.2
====

*To be released on 2023-05-xx*

This is an important update that addresses a number of issues and also includes new features. Please note that the changes introduced in this release break the binary compatibility with the previous version.

**Changes:**

1. Changed the :doc:`license <License>` from GPL to LGPL (by popular request).
2. Moved the public header from ``<emulation/CPU/Z80.h>`` to ``<Z80.h>``.
3. Removed the Xcode project.
4. Switched the build system from Premake to `CMake <https://cmake.org>`_.
5. Switched to `Zeta <https://zeta.st>`_ v0.1.
6. Added `pkg-config <https://www.freedesktop.org/wiki/Software/pkg-config>`_ support.
7. Added the following files to the project: :file:`.vimrc`, :file:`CITATION.cff`, :file:`file_id.diz` and :file:`THANKS`.
8. Added detailed documentation.
9. Added Pascal binding, courtesy of Zoran Vučenović.
10. Added tests.
11. Added :ref:`public macros <APIReference:Macros>` for checking the library version, working with flags, accessing the 16-bit registers and other purposes.
12. Added the :c:func:`z80_execute` function for running a simplified emulation without RESET and interrupts.
13. Added the :c:func:`z80_refresh_address` function for getting the refresh address of the current M1 cycle.
14. Added the :c:func:`z80_in_cycle` and :c:func:`z80_out_cycle` functions for obtaining the clock cycle at which the I/O M-cycle begins, relative to the start of the instruction.
15. Renamed the ``z80_reset`` function to :c:func:`z80_instant_reset`.
16. Added optional emulation of the special RESET, along with the new :c:func:`z80_special_reset` function.
17. Added the :c:data:`Z80::fetch_opcode<Z80.fetch_opcode>` and :c:data:`Z80::fetch<Z80.fetch>` callbacks for performing opcode fetch operations and memory read operations on instruction data respectively.
18. Added the :c:data:`Z80::nop<Z80.nop>` callback for performing disregarded opcode fetch operations during internal NOP M-cycles.
19. Added emulation of the NMI acknowledge M-cycle through the new :c:data:`Z80::nmia<Z80.nmia>` callback.
20. Added emulation of the INT acknowledge M-cycle through the new :c:data:`Z80::inta<Z80.inta>` callback, which replaces ``Z80::int_data``.
21. Added optional full emulation of the interrupt mode 0, along with the new :c:data:`Z80::int_fetch<Z80.int_fetch>` callback for performing bus read operations on instruction data. If not enabled at compile-time, the old simplified emulation is built, which supports only the most typical instructions.
22. Added four callbacks for notifying the execution of important instructions: :c:data:`Z80::ld_i_a<Z80.ld_i_a>`, :c:data:`Z80::ld_r_a<Z80.ld_r_a>`, :c:data:`Z80::reti<Z80.reti>` and :c:data:`Z80::retn<Z80.retn>`.
23. Added hooking functionality through the ``ld h,h`` instruction and the new :c:data:`Z80::hook<Z80.hook>` callback.
24. Added the :c:data:`Z80::illegal<Z80.illegal>` callback for delegating the emulation of illegal instructions.
25. Added :ref:`emulation options <APIReference:Configuration>` that can be configured at runtime.
26. Removed ``Z80::state``. Replaced with individual members for the registers, the interrupt enable flip-flops and the interrupt mode.
27. Removed the superfluous EI flag. The previous opcode is checked instead, which is faster and makes the :c:type:`Z80` object smaller.
28. Removed all module-related stuff.
29. Added emulation of the additional flag changes performed during the extra 5 clock cycles of the following instructions: ``ldir``, ``lddr``, ``cpir``, ``cpdr``, ``inir``, ``indr``, ``otir`` and ``otdr``.
30. Added emulation of the interrupt acceptance deferral that occurs during the ``reti`` and ``retn`` instructions.
31. Added MEMPTR emulation. The ``bit N,(hl)`` instruction now produces a correct value of F.
32. Added optional emulation of Q. If enabled at compile-time, the ``ccf`` and ``scf`` instructions will produce a correct value of F.
33. Added emulation of the ``out (c),255`` instruction (Zilog Z80 CMOS).
34. Added optional emulation of the bug affecting the ``ld a,{i|r}`` instructions (Zilog Z80 NMOS). If enabled at compile-time and configured at runtime, the P/V flag will be reset if an INT is accepted during the execution of these instructions.
35. Increased granularity. The emulator can now stop directly after fetching a ``DDh`` or ``FDh`` prefix if it runs out of clock cycles. This also works during the INT response in mode 0.
36. Reimplemented the HALT state. The emulation should now be fully accurate. HALTskip optimization is also supported.
37. Fixed a bug in the ``sll`` instruction.
38. Fixed a bug in the ``INX`` and ``OUTX`` macros affecting the S and N flags.
39. Fixed a bug in the ``OUTX`` macro affecting the MSByte of the port number.
40. Fixed the clock cycles of the ``dec XY`` and ``in (c)`` instructions.
41. Fixed the ``read_16`` function so that the order of the memory read operations is not determined by the order in which the compiler evaluates expressions.
42. Fixed the order in which the memory write operations are performed when the SP register is involved. This affects the NMI response, the INT response in modes 1 and 2, and the following instructions: ``ex (sp),{hl|XY}``, ``push TT``, ``push XY``, ``call WORD``, ``call Z,WORD`` and ``rst N``.
43. Fixed the handling of illegal instructions to avoid stack overflows in long sequences of ``DDh/FDh`` prefixes.
44. Fixed several implicit conversions to avoid warnings about loss of sign and precision.
45. Fixed some bitwise operations to avoid undefined behavior and arithmetic right shifts on signed integers.
46. Fixed violations of the C standard in several identifiers.
47. Renamed the 8-bit register lists: ``X/Y`` to ``J/K``; ``J/K`` and ``P/Q`` to ``O/P``.
48. Replaced all P/V overflow computation functions with a single, faster macro.
49. Replaced all register resolution functions with macros.
50. Replaced all ``ld {J,K|O,P}`` instructions that have the same destination and source register with NOPs. In addition, the "illegal" forms of the following instructions are now executed without using the illegal instruction handler: ``ld O,P``, ``ld O,BYTE``, ``U [a,]P`` and ``V O``.
51. Optimizations in flag computation and condition evaluation.
52. New source code comments and improvements to existing ones.
53. Improved code aesthetics.
54. Other improvements, optimizations and minor changes.

v0.1
====

*Released on 2018-11-10*

Initial public release.
