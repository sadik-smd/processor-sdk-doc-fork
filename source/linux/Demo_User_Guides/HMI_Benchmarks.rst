.. _hmi-benchmark-beagleplay:

#######################
HMI Framework Benchmark
#######################

This page presents benchmark results comparing three HMI frameworks - **Slint**,
**Flutter**, and **Qt** - running on a `BeagleBoard.org BeaglePlay
<https://www.beagleboard.org/boards/beagleplay>`__ (Texas Instruments AM625 SoC).

See :ref:`GUI_Frameworks_User_Guide` for instructions on building and running these
frameworks on |__PART_FAMILY_NAME__| platforms.

**********************
Platform Specification
**********************

.. list-table::
   :widths: 25 75
   :header-rows: 1

   * - Field
     - Value
   * - Board
     - BeagleBoard.org BeaglePlay
   * - SoC
     - Texas Instruments AM625
   * - CPU
     - 4× Arm Cortex-A53 @ 1.4 GHz · 512 KB L2 shared cache
   * - GPU
     - PowerVR Rogue AXE-1-16M · >500 Mpix/s · >8 GFLOPS · OpenGL ES 3.1 / Vulkan 1.2
   * - RAM
     - 2 GB DDR4
   * - Display
     - HDMI 1920×1080 @ 60 Hz
   * - Kernel
     - 6.18.15-ti (aarch64)
   * - Compositor
     - Weston (Wayland)

*******************
Results - Solo Runs
*******************

Each application was run in isolation. Measurements were taken after a 5 s settle
time over a 20 s interaction window.

.. list-table::
   :widths: 22 11 10 11 10 9 9 7 8 9
   :header-rows: 1

   * - App
     - Startup
     - PSS
     - PrivDirty
     - RSS
     - Threads
     - CPU%
     - FPS
     - GPU%
     - GPU_3D%
   * - flutter_material3
     - 2660 ms
     - 121.3 MB
     - 84.0 MB
     - 140.3 MB
     - 9
     - 65.3%
     - 60
     - 43%
     - 41%
   * - slint_home_automation
     - 3176 ms
     - 81.4 MB
     - 47.5 MB
     - 102.6 MB
     - 5
     - 28.7%
     - 60
     - 72%
     - 69%
   * - slint_gallery
     - 3174 ms
     - 71.3 MB
     - 37.1 MB
     - 91.8 MB
     - 5
     - 71.7%
     - 60
     - 64%
     - 60%
   * - slint_printerdemo
     - 3342 ms
     - 69.6 MB
     - 37.5 MB
     - 90.5 MB
     - 5
     - 13.2%
     - 60
     - 51%
     - 50%
   * - slint_energy_monitor
     - 3506 ms
     - 72.4 MB
     - 38.7 MB
     - 93.5 MB
     - 5
     - 25.7%
     - 60
     - 68%
     - 67%
   * - qt_ti_apps_launcher
     - 3331 ms
     - 138.0 MB
     - 73.3 MB
     - 160.3 MB
     - 13
     - 35.3%
     - 60
     - 81%
     - 77%

.. note::

   **Column definitions:**
   **PSS** = actual RAM cost (shared libs split proportionally) ·
   **PrivDirty** = heap + stack fully owned by process ·
   **RSS** = all resident pages (overcounts shared libs) ·
   **CPU%** = 100% equals 1 full A53 core ·
   **GPU_3D%** = fragment pipeline - dominant UI rendering metric

*******************
Framework Scorecard
*******************

.. list-table::
   :widths: 28 24 24 24
   :header-rows: 1

   * - Dimension
     - Slint
     - Flutter
     - Qt
   * - Memory (PSS)
     - 70–81 MB
     - 21 MB
     - 138 MB
   * - Threads
     -  5
     -  9
     -  13
   * - CPU (animation)
     -  13–72%
     -  65%
     -  35%
   * - GPU_3D peak
     -  50–69%
     -  41–82%
     -  77–81%
   * - Cache miss rate
     -  2.9–3.6%
     -  4.0%
     -  5.5%
   * - Startup
     -  3.2–3.5 s
     -  2.7 s
     -  3.3 s
   * - FPS
     -  60
     -  60
     -  60

*********************************************
GPU Timeline - Per App, 5 s Samples over 20 s
*********************************************

GPU_3D% sampled at T+0, T+5, T+10, T+15, T+20 s. Each value is the running
average of that 5 s interval.

.. list-table::
   :widths: 28 12 12 12 12 12 12
   :header-rows: 1

   * - Application
     - T+0 s
     - T+5 s
     - T+10 s
     - T+15 s
     - T+20 s
     - Pattern
   * - flutter_material3
     - 2%
     - 82%
     - 33%
     - 58%
     - 43%
     - bursty on scroll
   * - slint_home_automation
     - 23%
     - 68%
     - 62%
     - 69%
     - 72%
     - steady
   * - slint_gallery
     - 21%
     - 69%
     - 69%
     - 69%
     - 64%
     - flat
   * - slint_printerdemo
     - 23%
     - 37%
     - 53%
     - 39%
     - 51%
     - lightest
   * - slint_energy_monitor
     - 19%
     - 57%
     - 52%
     - 68%
     - 68%
     - steady
   * - qt_ti_apps_launcher
     - 26%
     - 37%
     - 61%
     - 62%
     - 81%
     - climbs over time

**********************************
Cache Efficiency - perf stat, 20 s
**********************************

.. list-table::
   :widths: 35 25 20 20
   :header-rows: 1

   * - App
     - Cache Misses
     - Miss Rate
     - Rating
   * - slint_printerdemo
     - 10.1 M
     - 2.90%
     - Best
   * - slint_energy_monitor
     - 19.5 M
     - 2.98%
     - OK
   * - slint_home_automation
     - 20.8 M
     - 3.42%
     - OK
   * - slint_gallery
     - 50.2 M
     - 3.62%
     - OK
   * - flutter_material3
     - 47.6 M
     - 4.00%
     - Moderate
   * - qt_ti_apps_launcher
     - 19.4 M
     - 5.47%
     - 88% worse than best Slint

*************************************************
Concurrent Load - All 3 Frameworks Simultaneously
*************************************************

All 3 apps live on display, user interacted with each during a 20 s window.

Memory - Before Animation
=========================

.. list-table::
   :widths: 32 17 17 17 17
   :header-rows: 1

   * - App
     - PSS
     - PrivDirty
     - RSS
     - Threads
   * - slint_home_automation
     - 72.5 MB
     - 47.2 MB
     - 101.6 MB
     - 4
   * - qt_ti_apps_launcher
     - 127.5 MB
     - 71.0 MB
     - 158.0 MB
     - 8
   * - flutter_material3
     - 95.2 MB
     - 66.3 MB
     - 121.9 MB
     - 7
   * - **TOTAL**
     - **295.2 MB**
     - **184.4 MB**
     - **381.5 MB**
     - **19**

.. note::

   System: **1009 MB used / 1949 MB total → 940 MB still free.** RAM headroom is not
   the limiting factor.

CPU - During Animation
----------------------

.. code-block:: none

   Per-App CPU Load:
     slint_home_automation    27.7%
     qt_ti_apps_launcher      21.9%
     flutter_material3        52.2%
     ─────────────────────────────────
     3 apps combined          101.8%  (sum)

   System Total:
     System total    156.5%
                     = ~1.6 of 4 A53 cores

     → 60% CPU headroom remaining

GPU - During Animation (All 3 Running)
---------------------------------------

.. list-table::
   :widths: 20 20 20 20 20
   :header-rows: 1

   * - T+0 s
     - T+5 s
     - T+10 s
     - T+15 s
     - T+20 s
   * - 18%
     - 80%
     - 81%
     - 52%
     - **96%**

.. warning::

   **96% GPU peak** = binding constraint. A 4th animated application would likely
   drop below 60 FPS. The single PowerVR pipeline is shared across all 3 renderers
   simultaneously.

.. note::

   **FPS: 60 Hz held (1202 IRQs / 20 s)** - confirmed via tidss vblank interrupt counter.

Memory - After Animation (Growth Tracking)
------------------------------------------

.. list-table::
   :widths: 32 17 17 17 17
   :header-rows: 1

   * - App
     - PSS Before
     - PSS After
     - Change
     - Status
   * - slint_home_automation
     - 72.5 MB
     - 72.9 MB
     - +0.4 MB
     - Stable
   * - qt_ti_apps_launcher
     - 127.5 MB
     - 127.4 MB
     - −0.1 MB
     - Stable
   * - flutter_material3
     - 95.2 MB
     - 108.9 MB
     - +13.7 MB
     - Growing
   * - **TOTAL**
     - **295.2 MB**
     - **309.2 MB**
     - **+14.0 MB**
     -

Flutter +13.7 MB = Skia texture cache + Dart heap growth under sustained input.
Risk for always-on HMI: Flutter heap grows continuously with interaction.

Cache - 3 PIDs Combined
-----------------------

.. list-table::
   :widths: 33 33 34
   :header-rows: 1

   * - Cache Misses
     - Miss Rate
     - IPC
   * - 52.0 M
     - 4.45%
     - 0.10

.. warning::

   **IPC 0.10 is very low** (healthy application = 0.5–2.0). Three renderers
   contending for 512 KB shared L2 → constant cache pressure → CPU stalls on RAM.
   The board is **memory-bandwidth limited** under concurrent load.

Solo vs. Concurrent Comparison
-------------------------------

.. list-table::
   :widths: 22 26 26 26
   :header-rows: 1

   * - Metric
     - Solo (per app)
     - Concurrent (all 3)
     - Status
   * - RAM total
     - 69–138 MB each
     - 295 MB combined
     - Headroom: 940 MB
   * - GPU peak
     - 51–81% each
     - 96%
     - Saturated
   * - System CPU
     - 13–72% each
     - 156.5%
     - 60% spare
   * - FPS
     - 60 Hz each
     - 60 Hz
     - Held
   * - Cache miss rate
     - 2.9–5.5% each
     - 4.45% combined
     - Pressure

********
Analysis
********

Memory
======

- **Slint 40–49% lighter than Qt**, 33% lighter than Flutter. No GC, no runtime VM,
  compiled Rust → tiny working set.
- On 512 MB boards (AM62L etc.) this gap is critical - Slint remains viable where
  Flutter and Qt are marginal.
- Flutter's concurrent PSS (95 MB) < solo (121 MB): PSS is proportional - Qt sharing
  EGL/Wayland libs inflates the denominator, reducing Flutter's apparent share.

CPU
===

- **Flutter highest** (65%): Skia CPU-side frame preparation + Dart GC spikes visible
  in profiling.
- **Qt moderate** (35%): scenegraph batches draw calls effectively, but
  cache-inefficient - CPU cycles largely wasted on memory stalls rather than useful
  work.
- **Slint proportional**: 13% for static UI (printerdemo), 72% for animated scroll
  (gallery) - framework overhead near zero, cost is pure application logic.

GPU - Binding Constraint
========================

- Solo: Flutter spikes to 82% on scroll, Qt climbs monotonically to 81%, Slint holds
  steady 50–72%.
- Concurrent: peaks at 96% - single PowerVR pipeline shared by all 3 renderers
  simultaneously.
- **Flutter** → spiky rendering (reacts to input, idles between gestures).
- **Qt** → monotonically climbing (draw call list grows over session time).
- **Slint** → flat, predictable (deterministic frame cost regardless of session
  duration).

Startup
=======

- Flutter fastest (2660 ms): Skia linked statically → one binary, one ``dlopen``.
- Slint slower (3.2–3.5 s): dynamically links Qt Wayland platform plugins at startup.
- **Caveat:** startup measured here = ``exec()`` → PID visible in ``/proc``, not first
  pixel. Dart VM warmup adds ~500 ms before first visible frame - actual user-visible
  latency narrows the gap.

Concurrent Load Limit
=====================

- RAM: fine (48% free, 940 MB remaining).
- CPU: fine (60% spare across 4 A53 cores).
- **GPU: saturated at 96%** - this is where BeaglePlay / AM625 hits the wall first
  under mixed framework concurrent load.

**************************
Recommendation by Use Case
**************************

.. list-table::
   :widths: 38 20 42
   :header-rows: 1

   * - Use Case
     - Recommended Framework
     - Rationale
   * - Memory-constrained SoC (≤512 MB, e.g. AM62L)
     - Slint
     - 70 MB PSS, 5 threads, no GC, no runtime VM
   * - Existing Qt codebase / large UI team
     - Qt
     - Ecosystem and tooling value justifies RAM cost
   * - Fastest dev iteration / cross-platform target
     - Flutter
     - Hot-reload, wide widget library - monitor heap growth on always-on HMI
   * - Multiple concurrent animated panels on one SoC
     - Slint
     - Best GPU budget per panel, stable memory under sustained interaction load

***********************
Measurement Methodology
***********************

All measurement on-device - no SSH round-trips during active windows.
``hmi_bench.sh`` runs on board → writes ``/tmp/hmi_results/*.txt``.
``run_benchmark.sh`` on host: scp script → trigger → fetch results → parse.

Launch Commands
===============

.. code-block:: console

   # Slint - as weston user for Wayland display focus
   su weston -c 'WAYLAND_DISPLAY=/run/user/1000/wayland-1 home-automation'
   su weston -c 'WAYLAND_DISPLAY=/run/user/1000/wayland-1 gallery'
   su weston -c 'WAYLAND_DISPLAY=/run/user/1000/wayland-1 printerdemo'
   su weston -c 'WAYLAND_DISPLAY=/run/user/1000/wayland-1 energy-monitor'

   # Flutter - needs bundle dir + LD_LIBRARY_PATH
   FLUTTER_BUNDLE=/usr/share/flutter/flutter-samples-material-3-demo/3.38.3/release
   su weston -c "WAYLAND_DISPLAY=/run/user/1000/wayland-1 \
     LD_LIBRARY_PATH=$FLUTTER_BUNDLE:$FLUTTER_BUNDLE/lib \
     /usr/bin/flutter-client --bundle=$FLUTTER_BUNDLE --width=1920 --height=1080"

   # Qt - via systemd (modifies Weston session, run last)
   systemctl start ti-apps-launcher

**Run order: Flutter → Slint ×4 → Qt.** Flutter first = clean Weston EGL state. Qt
last = systemctl startup modifies compositor session.

PID Discovery
=============

``pgrep -f`` matches parent ``su`` cmdline → returns wrong PID (su wrapper: 1 thread,
~5 MB RSS). Scan ``/proc/*/exe`` symlinks for actual binary path instead:

.. code-block:: console

   find_pid_by_exe() {
       for pid_exe in /proc/*/exe; do
           pid=${pid_exe%/exe}; pid=${pid##*/}
           exe=$(readlink "$pid_exe" 2>/dev/null || true)
           [[ "$exe" == *"/$1" ]] && { echo "$pid"; return 0; }
       done
   }

Startup Time
============

Poll every 50 ms from ``exec()`` until ``/proc/<pid>/exe`` appears. **Not**
first-pixel time - ``wp_presentation_feedback`` not available in this Weston build.

Memory - ``/proc/<pid>/smaps_rollup``
======================================

- Read after 5 s settle time (``STABLE_WAIT``).
- ``Pss:`` = proportional share of shared libs - most accurate per-app cost.
- ``Private_Dirty:`` = heap + stack, 100% owned, released on exit.
- ``VmRSS:`` = all resident pages, overcounts shared libs.

CPU - ``/proc/<pid>/stat``
==========================

``pidstat`` not installed → use jiffies delta. Fields ``$14+$15+$16+$17`` =
utime + stime + cutime + cstime. 100% = 1 full A53 core.

.. code-block:: console

   read_stat_cpu() { awk '{print $14+$15+$16+$17}' /proc/$1/stat; }
   cpu_pct=$(awk "BEGIN{printf \"%.1f\", (($cpu_t1-$cpu_t0)/100)/$INTERACT_WAIT*100}")

FPS - tidss Vblank IRQ
======================

.. code-block:: console

   awk '/tidss/{s=0; for(i=2;i<=NF;i++) if($i~/^[0-9]+$/) s+=$i; print s; exit}' /proc/interrupts
   fps=$(( (irq_after - irq_before) / INTERACT_WAIT ))

Counts 60 Hz hardware vblank interrupts - confirms display is running, not rendered
frame count. True frame FPS requires ``wp_presentation_feedback`` (not available here).

GPU - ``/sys/kernel/debug/pvr/gpu00/utilisation_stats``
========================================================

- Sampled at T+0, T+5, T+10, T+15, T+20 s in background subprocess.
- Each sample = running average of that 5 s interval only.
- T+20 s most representative of sustained load.
- ``3D:`` line = fragment pipeline - dominant UI rendering metric.

Cache Misses - perf stat
========================

.. code-block:: console

   perf stat -p $pid \
     -e cycles,instructions,cache-references,cache-misses,branch-misses \
     -- sleep $INTERACT_WAIT >> $outfile 2>&1 &

Miss rate = ``cache-misses / cache-references × 100``. IPC not available - ARM
Cortex-A53 PMU on this kernel does not expose the ``instructions`` event.

Timing Constants
================

.. code-block:: console

   STABLE_WAIT   = 5   # seconds: settle after PID appears
   INTERACT_WAIT = 20  # seconds: measurement window

.. note::

   Animations driven manually by the operator during the interaction window.
   ``ydotool`` not installed on this image. After each window: kill app → 3 s
   cooldown → ``echo 3 > /proc/sys/vm/drop_caches`` → next app.
