# The Department of Unnecessary Procedures

An offline, single-file, interactive bureaucracy simulator. Uses a low-glare, dark **Night Shift** color scheme by default.

## Launch

Double-click `index.html` in any modern browser. **No server, installation, network, or dependencies are required.** The page is self-contained; the files in this folder may be moved anywhere.

## Try it

1. The first petition (ring a small bell) is already filed. Click **Begin Processing** or **Advance One Tick**.
2. Watch eight departments approve, stall, and rescind paperwork, including the Department of Silly Walks immediately before the Committee on Minor Acts. The Department of Decorum can now reject a walk that the Committee already approved. Click any office to see its current authorization dependencies.
3. Select **Terminal** under **Petition Regulations** to introduce circular requirements between Registry and Dispatch, and between Safety and Decorum. These rules belong only to the currently selected petition. The app detects actual dependency cycles, including the Silly Walks/Committee contradiction, and explains the deadlock.
4. Choose **Issue Extraordinary Dispensation** on a blocked department to break an approval cycle. A cycle with two mutually dependent departments can be broken by overriding just one of them.
5. Switch to **Lenient** if you want a straightforward authorization, then click **Ring the Authorized Bell** after eight approvals. (The bell sounds even when office sounds are otherwise off.)
6. Submit another absurdly small action while the first is deadlocked. The new case starts with **fresh Official** regulations, independently of the original Terminal petition. Select Lenient for the new petition and observe it proceed while the original remains trapped under Terminal. To start a new case with the selected case’s rules instead, check **Copy selected petition’s regulations** before submitting.
7. Use **Revisit Oldest Deadlock** in the Docket Register to return to a stranded case. Alternatively, use **Unfinished** to find any petition not yet performed and select it manually.
8. Switch between petitions in the docket to verify that each retains its own regulation preset, switches, forms, and deadlock. Export the event ledger to see the governing rules and standing of **each** petition.

## The Department of Silly Walks (new eighth chamber)

Every petition must pass through the **Department of Silly Walks (F-041)**, placed immediately ahead of the **Committee on Minor Acts (F-047)**. The intention is that even a glass of water or one ring of a bell demands an unnecessary ceremonial walk:

1. Decorum and Fiscal review the petition.
2. Silly Walks submits **F-046-W** to the Committee on Minor Acts for a *provisional* walking permit. The Committee may endorse the gait provisionally **before** holding its final session.
3. The authorized silly walk is performed and inspected; the Silly Walks office stamps **F-041**.
4. The Committee can now convene to grant **F-047**, allowing Final Dispatch to consider the original trivial request.

**Circular mandates** add a supplementary requirement: Silly Walks cannot proceed without the Committee's *final* approval, despite the Committee needing the silly-walk certificate first. The simulator recognizes this direct Committee ↔ Silly Walks loop; a suitable dispensation or a change to that petition’s regulations is required to resolve it. Normal/Lenient/Official modes retain a possible route to completion.

The event ledger separately records the Committee's provisional permit, the certified silly walk, and its final vote. Each petition still retains its own independent regulation settings. Audits and misfilings can force the process to be repeated.

## Decorum's Final Say — Form F-023-G

The **Department of Decorum** now has authority to *overrule the Committee on Minor Acts after the Committee has already approved a completed silly walk*. Its original approval of the petition (F-023) and this **post-Committee conduct review (F-023-G)** are separate decisions.

1. Silly Walks obtains the provisional permit **F-046-W**, performs the walk, and certifies **F-041** as before.
2. The Committee on Minor Acts approves **F-047**.
3. **Final Dispatch now waits** while Decorum conducts a separate two-tick **F-023-G** review of the gait's appropriateness.
4. If Decorum **affirms** the walk, the Committee's approval stands and Dispatch may resume.
5. If Decorum **vetoes** it, the Committee's F-047 approval is rescinded; the Silly Walks F-041 certificate and provisional F-046-W permit are void. **The applicant must obtain a fresh provisional permit, perform the silly walk again, and secure another Committee approval.** Earlier unrelated approvals (Intake, Registry, Safety, Decorum's original F-023, Fiscal) remain valid unless a separate audit or filing incident invalidates them.
6. The next formally approved walk receives a *new* conduct review. If it passes, the petition can finally proceed.

The **Decorum's final say** switch is **enabled in Official and Terminal** presets; **Lenient** disables it. You can toggle it per petition, independently of other petitions. The reviewer is procedurally capricious but deterministic: not every walk is vetoed, and repeat submissions can receive a different verdict. A dispensation that waives the silly walk, the Committee, or Decorum's underlying authority bypasses this particular conduct review; such dispensations remain subject to other existing regulations.

The active docket, dossier, and office inspection show the review stage, attempt count, and last ruling. The event ledger records each F-023-G verdict, annulment, reissued permit, and eventual authorization. The footer counts **Decorum vetoes** separately from paperwork revocations.

## Office of Procedural Inquiry — separate investigative report (v1.6)

The **Office of Procedural Inquiry** creates an optional narrative **for one selected petition**. It does not replace, filter, rewrite, or otherwise modify the Certified Event Ledger. Both the `log()` recorder and the **Export Log** operation remain unchanged from version 1.5.

1. Select any petition in the Docket Register (including an older deadlocked case or one already finished).
2. Under **Office of Procedural Inquiry**, click **Examine Selected Petition**. A separate, scrollable report opens with the current status, the governing rules, counts of procedural interruptions, a causal chronology, and the eight forms' current standings.
3. The report groups ordinary approvals and explains when an audit, misfiling, or Decorum veto reverses earlier approvals. It omits routine clerk chatter, which remains available in the original ledger.
4. Click **Refresh Findings** to reconstruct the same petition again if the simulator advanced while the report was open.
5. Click **Export Report .TXT** to save an independent case-specific text document, or **Print / PDF** to print the report or save it as a PDF using your browser.

The investigation is a *snapshot* of existing records, not a simulation action: opening, refreshing, printing, or exporting it creates no new ledger entries and does not advance procedural time. Only data still present in the session's event history can be reconstructed (older entries may be pruned after 6,000 total events); the report notes when it cannot find the original receipt. Exporting a text ledger does not itself make it a reloadable case.

## Simulation rules

- Each of eight departments has explicit prerequisite departments and a review duration. Decorum also performs a post-Committee conduct review (F-023-G), without creating a ninth chamber.
- A department can only begin review once every prerequisite has been approved or explicitly waived.
- When regulations create mutual dependencies, directed prerequisite loops are detected in the *currently unresolved dependency graph* of **every open petition**. Waiting alone cannot resolve such a cycle.
- Deadlock detection and warning messages are scoped to each individual petition, independently of the case being viewed. Switching dockets does not reset a warning or inflate the contradiction count.
- Each docket retains its own regulation settings, forms, approvals, dispensations, completion state, first deadlock time, and current circular dependencies. Completing or changing a later petition does not alter an earlier one.
- New petitions start with a fresh, independent **Official** regulation configuration. If **Copy selected petition’s regulations** is explicitly checked at intake, that new petition receives an independent *copy* of the selected rules (including custom switches). Editing either petition later affects **only that petition**, immediately.
- Following final authorization, the petition’s regulations are sealed (read-only); other open petitions remain editable.
- The docket register retains **all** petitions during the session rather than silently moving older ones to inaccessible storage after ten cases. Unfinished petitions are listed before finished ones.
- The "no actionable motion" message is emitted only for an *unfinished* petition with neither an active examination nor an eligible office. It is not logged simply because no stamp was issued during an active review or a selected petition has been completed.
- Changing a petition’s regulations applies immediately to **that petition only**. Its approvals may be rescinded if newly required prerequisites become invalid. Rules on other petitions are never changed implicitly.
- Retroactive audits and misfiling incidents periodically invalidate previously approved paperwork **only for petitions whose individual settings enable those incidents**. Terminal and Official use different audit intervals, independently per petition; Lenient disables both disruptions.
- Dispensations are deliberately powerful manual overrides. They satisfy one department's requirements but not unrelated requirements.
- Completed petitions are no longer subject to audits. Authorization and actually *performing* the tiny act are separate events.

## Controls

- **Begin Processing / Suspend**: run or pause automatic evaluation.
- **Advance One Tick**: process one discrete clock tick.
- **1× / 3× / 8×**: simulation speed.
- **Sound**: opt-in quiet office clicks. The bell plays by direct action after authorization.
- **Decorum’s final say**: enable or disable the post-Committee F-023-G conduct veto for the selected petition; on a veto, all silly-walk paperwork is reissued while unrelated approvals are preserved.
- **Petition Regulations — Lenient / Official / Terminal**: presets and switches apply **to the selected case only**. Switching to another case shows its own settings. Newly filed cases start under fresh Official settings unless **Copy selected petition’s regulations** was checked at intake; copying is one-time and creates independent settings. Certified/authorized cases cannot be edited retroactively.
- **Copy selected petition’s regulations**: optional checkbox in Petition Intake, unchecked by default. Check it only when you want a new petition to start with the selected petition’s rules without linking their future changes.
- **Docket Register**: click any older petition to return to its independent state. **All / Unfinished / Finished** filters keep the register navigable as it grows. "Unfinished" includes authorized acts that have not yet been performed.
- **Revisit Oldest Deadlock**: select the earliest petition with outstanding circular prerequisites and inspect a conflicting office.
- **Extraordinary Dispensation**: manually override the currently inspected office for the **selected petition only**.
- **Export Log**: save the entire event ledger as a `.txt` file, including each case’s own regulation preset, switches, standing, and unresolved loops; rule revisions are attributed to their specific petition.
- **Shred All Files**: reset the simulation and pre-file a new bell petition.

## Design note

State is retained in the open browser tab while the simulation is running, **not** automatically saved across reloads or browser restarts. A text ledger is an audit document, not a restorable save file.

Version 1.6 · supplementary case investigation; independent case rules remain unchanged.

This is an illustrative rules-based simulation, not a complete government or administrative process model. Approval times and incident intervals are intentionally deterministic so you can compare the results of different regulatory settings.
