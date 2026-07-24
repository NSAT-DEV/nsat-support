# NSAT — Neal's Sorta Accurate Timer
## User Guide (App v2.5)

NSAT is an iOS timing app for open-road time-speed-distance racing events such as the Silver State Classic Challenge, the Nevada Open Road Challenge, and the Big Bend Open Road Race. It pairs with a RaceBox GPS receiver over Bluetooth, starts the clock automatically at a scheduled GPS-synchronized start time, measures your progress against a target average speed, and stops the clock the instant the front of your car crosses a geographically defined finish line. Every run is scored, logged, and exportable.

---

## What You Need

- An iPhone or iPad running iOS 16 or later.
- A RaceBox Mini, RaceBox Mini S, or RaceBox Micro GPS receiver, powered on and mounted in the car.
- Bluetooth enabled on the phone. The app will ask for Bluetooth permission on first launch.

The app forces the screen to maximum brightness and keeps it awake while running, so a phone charger is strongly recommended for long runs. All notification banners and sounds are silenced while NSAT is in the foreground so nothing interrupts the display mid-race.

---

## The Big Idea

NSAT does not have a start button. Instead, you set a **start time** (in UTC, synchronized to GPS atomic time) and a **course** with a known finish-line coordinate. When the GPS clock reaches the start time, the timer starts on its own — whether you've launched yet or not. When your position crosses the finish-line latitude (or longitude, depending on direction of travel), the timer stops on its own. Your score is the **variance**: how many seconds your actual time differs from the "perfect" time implied by the course distance and your target average speed.

Timing is interpolated between GPS packets at both the start and the finish, so resolution is finer than the GPS update rate.

---

## First Launch and Connecting

1. Power on the RaceBox and launch NSAT. You'll see the title screen.
2. Tap **Scan for Devices**. A device picker appears listing every RaceBox in range.
3. Tap your device to connect.

The app remembers your RaceBox. On every later launch it reconnects automatically — you'll see a brief "Reconnecting…" spinner instead of the scan button. If the Bluetooth link drops mid-session, the app silently reconnects in the background without losing race state.

The title screen also offers **Race Courses** and **Run Log** buttons, so you can review courses and past results without a RaceBox connected.

## Acquiring a GPS Fix

After connecting, the app waits for the GPS solution to reach race quality. The status card shows four items and the app will not proceed until all are satisfied:

1. **Satellites** — needs enough satellites for a solid solution (green at 4+).
2. **Fix Status** — must reach a full **3D Fix**.
3. **Accuracy** — horizontal accuracy must be better than **1.0 m**.
4. **Nano Sync** — the receiver's nanosecond time resolution must be fully resolved.

This can take a minute or two with a clear sky view, longer under cover. When everything turns green the main dashboard appears, and the auto-start time is set to at least four minutes in the future (rounded up to the next whole minute) so you always have setup time regardless of how long the sync took.

---

## Setting Up a Run (Idle Screen)

Before the start time arrives, the dashboard shows everything you need to configure, top to bottom:

**GPS status bar.** Fix quality, satellite count, validity, and your current coordinates.

**Active course banner (yellow).** Shows the selected course. Tap it (or the crossed-flags icon in the top-right toolbar) to open the course library and pick a different one. Selecting a course loads its finish coordinate, distance, and direction of travel, and resets the timer state.

**Variance Offset toggle (indigo).** Appears when a previous run's result is on record. See "Chasing Zero" below.

**GPS TIME.** The live atomic clock from the receiver, in UTC.

**START TIME.** The scheduled start, shown as minutes:seconds. Use the orange **up/down arrows to move it in 30-second steps**. The down arrow disables itself when the start would get too close to the current time. Set this to your assigned start time for the event.

**COUNTDOWN.** Time remaining until the start.

**TARGET SPEED.** Your target average speed for the run, adjustable in **5 mph steps** with the orange arrows. This is the speed your score is measured against.

**GPS OFFSET.** The distance, in feet (0–22, half-foot steps), from the RaceBox antenna back to the **front of the car**. The finish threshold is shifted by this amount so timing stops when your nose crosses the line, not the antenna. Measure it once for your car and it's remembered.

**ELAPSED TIME.** Reads zero until the start.

**Metrics grid and FINISH LINE card.** Current speed is live even before the start; the other metrics show `--` until the run begins. The finish-line card confirms the axis (latitude or longitude), the threshold coordinate, and the direction of travel for the active course — worth a glance to confirm you've selected the right leg.

Two things to know about the automatic start:

- The race **will not start while the Course or Run Log sheet is open**. This is a safety interlock so the timer can't fire while you're mid-edit.
- **Closing either sheet pushes the start time back** to at least four minutes out. Finish your paperwork first, then set your start time last.

---

## During the Run

At the start moment the display reorganizes for at-speed readability. Distance traveled (in miles) replaces the title in the top bar, and the toolbar buttons gray out.

**ELAPSED TIME** — the running clock, large and centered.

**vs target** — the live variance, flanked by arrows. **Green with down arrows: you are ahead of pace (fast). Red with up arrows: behind pace (slow).** The goal is to bring this to zero at the line. The variance compares you against a "perfect car" that runs exactly the target speed the whole way.

**REC SPEED** — the recommended corrective speed: the average you'd need to hold from here to the finish to arrive at exactly zero variance. It shows `--` until you've covered **5 miles** (early readings would be noise) and hides again inside the final **minute** of the run, when chasing a correction is no longer practical.

**Metrics grid** — current speed, average speed so far, distance to finish, and top speed.

**FINISH LINE card** — live distance to the line. Within **1 mile** it turns red and flashes an APPROACHING warning.

Distance is measured by accumulating GPS positions, then progressively blended toward the known finish coordinate over the final 20 miles, so the reading arrives at the line smoothly and accurately.

## The Finish

There are three ways a run can end, and the results screen labels which one occurred:

**Normal.** You crossed the finish threshold in the course's direction of travel. The exact crossing moment is interpolated between the two straddling GPS packets, and the GPS offset is applied, so the recorded time reflects the front of the car at the line.

**Estimated (dead reckoning).** If the GPS fix is lost for 4 or more seconds within 2 miles of the finish — an underpass, a canyon wall — the app projects your last known speed to the line and records an estimated time, clearly marked as such.

**Manual (emergency stop).** Pressing the home button (or otherwise leaving the app) during a run immediately captures the elapsed time as a manual stop, saves it to the log, and exits. Use this only if you must abort — there is no way to resume.

Backgrounding the app always exits it by design; when idle this is harmless, but during a run it triggers the manual stop above.

## Results

When the run ends, a full-screen summary appears: final time (with a note on how it was determined), the perfect target time, your variance in seconds and as a percentage of perfect time, final average speed, and top speed. The run is saved to the log automatically, and the Bluetooth link disconnects a moment later. Tap **Reset** or **View Run Log** to continue.

---

## Chasing Zero: The Variance Offset

Many events run multiple legs, and what matters is the total. The **Variance Offset** toggle on the idle screen lets the next run deliberately cancel the previous run's error.

Suppose your last run finished 12 seconds slow. Turn the toggle on and the app shifts this run's goal 12 seconds fast: the live variance display starts at −12 and counts toward zero, so driving this run "to zero" nets the two legs out. The card shows both the last run's result and the new goal so there's no ambiguity about which direction to push.

The offset applies to the **display pacing only** — the logged result for each run is always scored against the true, unshifted target. The toggle switches itself off after the run so it can't silently carry over, and the underlying value survives an app restart (including a manual-stop exit), so it's still available at the start line of the next leg.

---

## The Course Library

Open with the crossed-flags icon (toolbar or title screen). The app ships with courses for the Silver State Classic Challenge, NORC legs, and Big Bend legs; you can add your own with **+**.

A course is defined by:

- **Name** — whatever you'll recognize at the start line.
- **Finish Latitude / Longitude** — the finish-line coordinate, in decimal degrees. The **±** button toggles the sign (western longitudes are negative).
- **Course Distance** — official course length in miles.
- **Travel Direction** — northbound, southbound, eastbound, or westbound. This determines which axis (latitude for N/S, longitude for E/W) is used as the finish threshold and which way a "crossing" counts. The form previews the resulting finish axis as you edit.

Tap a course's circle to select it as active; tap the pencil to edit; swipe left to delete. Only one course is active at a time, and it's remembered between sessions.

## The Run Log

Open with the clipboard icon. Every completed run — normal, estimated, or manual — is stored newest-first. Tap an entry to expand its full details; swipe left to delete one; the trash icon clears the whole log (this also clears the stored variance offset).

The share icon exports the log as a **CSV file** containing, for each run: course name, date, UTC start time, start and end coordinates, course and actual distance, target/actual/top speeds, target and actual times, variance, and finish type. The log files (`RaceRunLog.csv` and `RaceRunLog.json`) are also visible in the iOS **Files app** under NSAT's documents, and via Finder/iTunes file sharing when the phone is connected to a computer.

---

## Quick Pre-Race Checklist

1. RaceBox powered and mounted; phone on charger; NSAT launched.
2. Wait for the fix screen to clear (3D fix, sub-meter accuracy, nano sync).
3. Select the correct course leg — confirm direction and distance on the finish-line card.
4. Set target speed.
5. Confirm GPS offset for this car.
6. Decide on the variance offset (multi-leg events only).
7. Set the official start time last, using the ±30 s arrows.
8. Close all sheets and leave the phone alone. The timer handles the rest.

---

## Troubleshooting and Notes

**The start time keeps jumping forward.** That's the app protecting you: reopening and closing the course or log sheets, or a late GPS sync, resets the start to at least four minutes out. Set the start time as your final step.

**"Waiting for accuracy better than 1.0 m…" won't clear.** Move the RaceBox to a clearer sky view — dashboards with metallic tint, roof racks, and tree cover all hurt. Accuracy is deliberately strict because it directly bounds finish-line precision.

**The screen is blindingly bright.** Intentional — the app pins brightness to maximum for sunlight readability and restores your previous brightness when it exits.

**Timer didn't stop at the finish.** Check the course's travel direction and finish coordinate; the crossing test only fires when the correct axis crosses the threshold in the configured direction. The finish-line card on the dashboard shows exactly what the app is watching for.

**All times look "wrong" by several hours.** All times in NSAT are UTC (GPS time), matching how open-road events publish start times. Convert from local time accordingly.
