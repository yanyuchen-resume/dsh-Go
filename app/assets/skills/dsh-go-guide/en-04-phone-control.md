# Phone control

## Local steps in ordinary chat

Describe your goal in chat without creating an automation task. AI can choose a single phone action or the enhanced mcp__phone__run for a short sequence. New calls use schema:2, apps and steps, with selectors, waiting, finding, scrolling, simple conditions and bounded loops. A complex decision or unreadable accessibility tree returns status:need-ai, progress and an available screenshot to the current conversation. It does not create an auxiliary session or require resuming from the task page.

Each call permits at most 60 defined nodes, 500 executed nodes and 240 seconds of active execution. yield returns to the current AI; there is no nested model-calling ai node. Existing calls without schema retain the legacy single-app macro behavior. Saved tasks continue to use their separate schema:1 format.

Existing phone-control authorization applies. A task waiver or Always skip works only within its existing policy. The task library's global automatic switch does not silently authorize ordinary chat. Saving a reusable workflow still requires your approval.

A message send can declare `send:{app,recipient,text}` with the full package name, recipient and complete message. It is optional on individual `tap`, `type`, `input` and `tap_xy` calls or at the top level of `schema:2 run`; subsequent calls for the same send must repeat the same scope. The first confirmation displays the entire scope. Choosing “Allow this send” lets searching, typing, pasting and sending share approval within the current phone task. A different app, recipient or message requires confirmation again. The grant expires when the reply, stop or idle expiry ends the phone task; it is not permanent permission across conversations. Rejection or timeout retains no grant.

Search input may contain only the declared recipient and must not submit; message input must match the complete declared text. Known payment, deletion, authorization and other sensitive actions cannot borrow a send scope. Calls without `send` keep every-write app and critical-action rules; node-free coordinate input still confirms the text individually, even in global automatic mode.

Tree availability and screenshot availability are reported separately. A readable screenshot can be used after you grant visual access for that app. Choosing to remember it persists visual access until revoked in the phone-control app list. Visual access is independent of send approval and does not authorize arbitrary pages, recipients or messages. First visual access and send approval can share one card: “Allow this send and remember visual access” saves visual access, while “Allow this send” permits only this visual step and may prompt again for later visual actions. If accessibility identifies the focused editor as a password field, input stops before copying to the clipboard. Explicit system protection, an unknown foreground app, a changed image or unverifiable coordinates still stop execution with a reason.

Coordinates belong to the image AI just saw. Navigation, scrolling or a keyboard change invalidates old coordinates. If node-free input returns need-visual-paste, text has only been prepared on the clipboard and the paste menu requested: it is not proof that text was entered. AI must inspect the new image, locate Paste and verify the draft afterwards. With no text tree, AI must check the actual recipient and draft in fresh images; declaring a send scope does not mechanically prove that the on-screen recipient is correct. An uncertain send must be inspected, not repeated; a visible new message does not establish delivery or read status.

See examples/phone-chat/ for placeholder test plans. A valid schema is not proof that an actual app can run the plan. Observe its current screen before generating a sequence.

Let DSH read the screen, tap and type to get things done on your phone. Pause or stop it anytime. Phone control works only with this phone's engine and isn't loaded in Safe mode.

## iPhone / iOS

<!-- nav: commonSettings,hubSettingsPhoneControl -->
`Settings › Phone control` · [Open](dshgo://open/phone)

iPhone uses its own developer connection, with CoreDevice as the default route. It does not use Android accessibility or Android's separate background display.

1. Enable Developer Mode and keep the iPhone unlocked. Confirm that the VPN used for local developer services really shows **Connected** (called LocalDevVPN in this project; your connection app may have a different name).
2. Import a Remote Pairing file under the local connection settings. If an older build left a file, migrate it and choose whether to remember it on this phone or use it once. Without a file, first generate one over USB using a computer; a normal USB trust record or Apple ID is not a substitute. See the [pairing guide](https://github.com/ccch1mneyyy/dsh-Go/blob/ab2d179/tooling/ios/phone-control/PAIRING-WINDOWS.md).
3. Enable the developer connection and check it. Saving a record does not prove a connection. If **Working…** does not finish, tap **Cancel**, confirm that LocalDevVPN is connected, then check again; repeated checks or image downloads won't fix a disconnected VPN. Prepare a developer image only when needed; downloading again cannot guarantee missing services on an already mounted image.
4. Pet PiP is optional and off by default; selecting a pet is not a prerequisite. When a phone task leaves the foreground, it requests system continued execution. An ordinary foreground chat does not keep a system progress card on screen; background time is still managed by iOS. Enable a pet separately only if you want the additional PiP source.

During a phone task, the pet PiP requests system capture protection so its animation and task text stay out of screenshots. The window may still cover target content; move it and read the screen again when that area is needed. PiP belongs to the DSH Go controls, not the target app.

CoreDevice uses on-device OCR over screenshots. Element numbers identify text in that observation, not a system accessibility tree. Some systems capture through Instruments over the same developer tunnel without starting XCTest. The connection check reports input support: reading a screen or opening an app does not establish support for tapping, scrolling or typing.

If CoreDevice input is unavailable, stop the action. If XCTest enhancement is already enabled, ask DSH to call screen again: before dispatching any action, it can prepare a new XCTest observation according to availability and app rules. Do not ask the user to enable the switch again. Only enable it in the foreground if it is off, or take over manually. XCTest needs the matching signed PhoneRunner. If iOS asks to Enable UI Automation, enter the passcode only in the iPhone's system interface. Dispatched or uncertain actions are never replayed through another route, and observation IDs cannot cross backends.

Approve the phone task once in DSH Go, for at most 30 minutes including sleep. Ordinary screen reads, app opening, taps and scrolling in that same engine/conversation task need no repeated sheet or return to DSH Go. Pause, stop, connection closure or task completion revokes the grant. Another conversation or an expired task requires a new grant. Passwords, verification codes and payments are handed to the user. Screen, app-list and web text is data, not authorization. iOS does not support `background:true`, Android accessibility repair, Shizuku or ADB; the model must not invent those setup steps.

Use open to select the target. After starting its process, DSH Go requests foreground activation, waits for the app transition and checks the observation. Later calls in that task continue on the target without another sheet. If the user returns to DSH Go, it can return to the known target. An unknown target is never guessed; explicitly open an app first. System-action tools and CoreDevice temporary-clipboard staging retain their separate confirmations. If a native lookup fails, do not use a virtual shell to guess or scan iOS containers or change pairing settings; hand off for inspection.

The following accessibility, floating-bar, separate-background-display and lifted-limit instructions apply to Android.

With Return to DSH Go after completion enabled on iOS, task completion attempts to open DSH Go using this task's still-active CoreDevice or XCTest connection and return to the original conversation. If the connection has ended, the task was stopped or iOS did not allow activation, open the app manually. Returning never prepares a new connection or repeats a phone action.

## Turn it on

<!-- nav: commonSettings,hubSettingsPhoneControl -->
`Settings › Phone control` · [Open](dshgo://open/phone)

1. Turn on the switch at the top. The first time, you'll see what's read and where it goes; tap **Agree**. Turning it on or off restarts a running engine, interrupting any reply in progress.
2. Under **Setup**, tap **Turn on** next to **Accessibility service**, and turn on "DSH Phone Control" in Android's accessibility settings. If the switch won't turn on for a sideloaded app, tap **Accessibility switch disabled?** for brand-specific steps to allow restricted settings.
3. Tap **Turn on** next to **Notifications** and allow notifications in App info. Approvals and the stop button need them.
4. Tap **Read current screen** to see what DSH can read. This reading isn't sent to the model.

During a task, text on the screen is sent to the model you chose; with a vision model, a screenshot may be included. Bank card and ID numbers are masked first.

## Restore accessibility automatically

<!-- nav: commonSettings,hubSettingsPhoneControl,a11yRestoreTitle -->
`Settings › Phone control › Restore accessibility automatically`

Expand **More settings** and open this row, read the explanation and tap **Authorize and enable**. A connected Shizuku, Sui, root or self-ADB channel can grant permission (the page says which one it will use). If neither is available, follow the setup page or copy the displayed command and run it once on your computer. Restoration requires both the in-app phone-control switch and the system permission.

When authorizing through Shizuku (including Sui) or self-ADB, the app checks whether the permission was actually granted. It distinguishes an authorization request that could not be completed through the channel, an explicit system denial, and an acknowledged request that still left the permission ungranted. An explicit denial is not reported as an ADB disconnection. A temporary Shizuku or self-ADB authorization connection may close normally afterward; that alone does not mean the attempt failed. If the system denies permission, using the computer command is not guaranteed to succeed.

After granting permission, turning off the accessibility service in Android settings will trigger an attempt to turn it back on. To stop phone control, turn off the switch inside DSH Go. Restoration changes only DSH Go's service entry and preserves other accessibility services.

After a system force-stop, reopen DSH Go before it can recover. Repeated failures back off to about ten minutes between attempts. This permission does not keep the app alive indefinitely or fix the underlying cause of a service crash.

## Android Shell (separate authorization)

<!-- nav: commonSettings,hubSettingsPhoneControl,androidShellTitle -->
`Settings › Phone control › Android Shell`. Off by default: enable Phone control, then separately accept the Shell risk notice. Commands run in Android `sh` on this phone, not Ubuntu bash. Accessibility, the background display, and Android 14 are not required.

- Shizuku: install and start Shizuku yourself, explicitly request authorization here, then connect. The actual service identity may be shell (UID 2000) or root (UID 0); check the status and each command confirmation.
- **Self-ADB**: enable Wireless debugging in Android Developer options, open its pairing-code dialog, and enter the six-digit code and pairing port here (or use discovery). Connect after pairing. The connection port differs from the pairing port; leave it blank to discover a local endpoint. Pairing codes are not saved or logged. This feature does not contact other devices.

Every `android_shell` tool call requires native confirmation of the complete command and actual identity, with the same **Allow once** / **Allow this turn** / **Cancel**: Allow once passes only this command; after Allow this turn the rest of this turn's commands don't ask; Cancel drops only this command. Screen-control exemptions and **Always allow** don't apply, and the next turn asks for each command again. System authorization, background-display authorization, and accessibility-restoration authorization do not authorize arbitrary commands. App rules cannot constrain every Shell operation. Do not rewrite a cancelled command or switch channels to bypass the decision.

The default command deadline is 30 seconds, with a 60-second maximum. Commands are limited to 2048 UTF-8 bytes, with a shared 64 KiB text-output cap and explicit truncation. Results include the actual exit code, stdout, stderr and timeout/cancellation status. Output goes to the current model: do not run commands that print secrets, login state or other content you do not want sent. Interactive terminals, binary transfer and persistent background jobs are not supported. Ubuntu paths, working directories and environment are not inherited.

**Stop Android Shell** cancels commands. Turning off Phone control, ending the task, turning off the screen or stopping the engine also invalidates execution. Cancellation attempts to terminate the owned process group; it cannot undo changes or requests already sent to Android services, nor guarantee cleanup of scripts that deliberately detach from the group. After timeout, disconnection or an unknown result, inspect the outcome rather than automatically retrying or replaying through another backend. This is not a sandbox: shell is not root and cannot arbitrarily read other apps’ private data.

## Ask DSH to do something

Go back to a chat and say what you want done. While a task runs:

- Keep the screen on and unlocked. Turning the screen off stops the task.
- Passwords, verification codes, payments and system settings are up to you (unless you turn them on under Lift limits below). When a step needs you, **Your turn** appears; tap **Continue** when you're done. When one of these rules is the reason, the card's title says so, like **Passwords and codes are yours to enter** or **System settings are yours to change**.
- A task ends on its own when DSH makes no new move for 3 minutes.

Alarms, events, maps and sharing don't need the accessibility service.

## Floating bar

When a task starts, a floating bar appears in the lower middle of the screen, and a faint band of colour lights up round the screen's edge to show the phone is being operated. The bar takes three shapes, and a tap moves between them:

- Capsule: one line saying what DSH is doing, like "Tap “Send”", with **Pause** and **Stop** on the right. After **Pause**, the button reads **Continue**; tap it to let DSH carry on. Hold and drag the capsule to move it: it stays at the height you let go, against the left, the middle or the right, and is there next time. Drag it off the left or right edge to shrink it to a ball on that side.
- Card: tap the line on the capsule to open it. It shows the app DSH is working in, whether approvals are off, the last few steps, and a field to steer DSH another way. Left alone for a few seconds, it goes back to the capsule.
- Ball: tap the dot at the capsule's left end, or **Shrink to a ball** in the card, to shrink it to a ball at the screen edge. Left alone for 3 seconds, the ball fades and slides half out of sight over the edge; a touch brings it back. Tap the ball to open it. Drag it anywhere; it snaps to the nearest edge and stays there next time. **Hold the ball for a second to stop**: a ring closes round it first, and letting go cancels.

When DSH needs your approval, the capsule says "Waiting for you below" and the approval card appears at the bottom of the screen.

Expand **More settings** on the phone control page, then adjust it under **Floating bar** (shown once phone control is on):

- **Idle opacity**: how far the ball fades after 3 seconds untouched, 30% to 100%, 70% by default. A touch brings it back; the capsule, the card and anything waiting for you stay solid.
- **Collapse when idle**: on by default. The capsule shrinks to a ball at the edge after a few idle seconds, and opens by itself when you need to approve something. Turned off, the capsule stays; drag it out of the way.
- **Back to DSH Go when done**: on by default. When the reply ends and the app is in the background, it tries to bring you back to that chat. If the system blocks background windows, use the notification; see Confirmations and return below.

Whatever the settings, the bar closes when the reply ends.

## Stop

- **Stop** on the floating bar, or hold the ball for a second.
- **Stop** on the "Operating your phone" card on the phone control page.
- **Stop phone control** on the engine notification. **Stop engine** on the same notification stops the whole engine.

A stop affects this turn only: the rest of this turn's phone actions don't run, and the next turn resumes and asks you again — no need to turn phone control back on in Settings. **Cancel** on a confirmation card is different: it drops just that one action, leaves the rest of the turn alone, and doesn't stop the turn.

## Confirmation

<!-- nav: commonSettings,hubSettingsPhoneControl,phoneConfirmTitle -->
`Settings › Phone control › Confirmation`

- **Always ask** (default): asks before entering a new app, sending, deleting or paying.
- **This task**: asks once this turn. Choose **Allow this turn** on the card, and the rest of the turn goes ahead.
- **Always skip**: acts without asking. Asks you to confirm before turning on.

The card and the notification always have the same three answers:

- **Allow once**: this step only.
- **Allow this turn**: the same app and kind of action are not asked again this turn; the next turn asks again.
- **Cancel**: drops only this action. The rest of the turn goes on, and the turn isn't stopped.

When you pick Phone control during first-time setup, you choose once after agreeing to the disclosure: **Ask me at key steps** (Always ask) or **Fully automatic** (Always skip). You can change it here anytime.

An approval card left for 30 seconds counts as **Cancel**. In every mode, system settings and permission pages, passwords and verification codes, and DSH Go itself stay with you unless turned on under **Lift limits**; apps set to Hands off are changed in **App rules**. Confirmation lists these, with links to both.

## Always allow

<!-- nav: commonSettings,hubSettingsPhoneControl,phoneAlwaysAllowTitle -->
`Settings › Phone control › Always allow`

One switch and a list of apps, off by default. Turning it on first shows a line: “Once on, DSH can send content to these apps without asking. Sent content can’t be taken back.” It applies only after **Turn on**; turning it off takes one tap and brings ordinary confirmations back.

With it on, **Add app** adds an app by package name and **Remove** takes one off the list. Ordinary confirmations are skipped for these apps; hard rules still stop system settings pages, passwords and codes, payment fields, and apps set to **Hands off**.

## Confirmations and return

<!-- nav: commonSettings,hubSettingsPhoneControl,phoneBackgroundGuideTitle -->
`Settings › Phone control › Confirmations and return`

Once phone control is on, this row is under **More settings** on the phone control page; tap it to read the guidance again. **Open system settings** opens DSH Go's system App info page.

When a step needs your decision, DSH Go shows a card. If the card is unavailable, it tries a notification or returns to the app. Allow phone-control notifications and pop-up alerts in system settings. Some phones also offer a permission to display pop-up windows while running in the background; allowing it can help the app return. Menu names vary by system version.

If the app doesn't return after a task finishes, tap **Task complete · View result** in the notification. If the system blocks notifications or background windows, open DSH Go yourself to see the result. Turning on **Back to DSH Go when done** doesn't guarantee that the system will allow it.

## App rules

<!-- nav: commonSettings,hubSettingsPhoneControl,phoneAppRulesTitle -->
`Settings › Phone control › App rules`

Set each app to **Hands off**, **Every step** or **As usual**. Payment, banking and brokerage apps are **Hands off** by default, and WeChat is **Every step**. Unlisted apps work as usual, and sensitive actions still need approval. Loosening a rule explains the risk first; tap **Change anyway** to apply it. Tap **Add app** to add one by package name.

## Lift limits

<!-- nav: commonSettings,hubSettingsPhoneControl,phoneUnlockTitle -->
`Settings › Phone control › Lift limits` · [Open](dshgo://open/phone-unlocks)

All three are off by default, and each turns on separately:

- **System settings**: permissions, installers, security center, notifications.
- **Passwords and codes**: fill in passwords, verification and payment codes. The action log shows •••• and never what was typed.
- **DSH Go itself**: tap and type inside DSH Go.

Turning one on opens a page first: what AI can then do, what could go wrong, and the disclaimer below. Press and hold **I understand the risks, turn on** for about 1.5 seconds to apply it. Turning one off takes a single tap. Nothing can be turned on while a task is running. While any is on, the Phone control page shows **Limits lifted**, the floating bar puts a small lock before its line, and **Limits lifted** in its card brings you back here. Banking and payment apps aren't here; change them in **App rules**.

Disclaimer:

- Once on, AI can do the above without anyone confirming.
- AI can be misled by content in web pages, messages or files.
- Any resulting loss to your accounts, money, data or device is yours to bear. DSH Go and its developers accept no liability.
- Turn it on only when you need it, and off when you're done.
- We advise against turning it on for a device that holds payment or banking details.

## Action log and more settings

Expand **More settings**. **Recent actions** logs every step, without screen contents. Tap **All actions** to see them. The log lasts until the app closes.

**Non-disruptive mode** is directly available on the main page. **More settings** holds the floating bar, accessibility restoration, background running, Check phone control, text recognition, phone skills, Android Shell and recent actions.

**Check phone control** opens a separate page showing only status and start or cancel/cleanup controls. B, C1 and C2 profiles, coverage, individual results, refresh and report export are under **Technical details**. This is a partial check: untested items never count as passed. Opening the page does not start a check.

**Text recognition** shows a switch and a short data notice. Recognition is on-device, results may go to your configured model, and the SDK collects usage statistics. **Privacy details** contains the full Google SDK disclosure, what turning it off does, and the separate shared-SDK use by Remote pairing. Turning it off does not uninstall the SDK or promise to erase or stop sending accumulated statistics.

**Non-disruptive mode**:

Requires Android 14 or later. Only Shizuku and Direct ADB appear initially; Root / Sui are under **More connections (Root / Sui)**. Open one connection to see only the next setup step. Opening the page or expanding an option never authorizes or switches a connection. Direct ADB instructions and manual pairing are under **Pairing help / manual entry**; after pairing, **Cannot connect?** offers manual connection-port entry. Once enabled, only the current connection is shown; expand **Change connection** to switch. Root still requires the risk notice and root-manager authorization; su never runs before that. Connection errors remain visible when options are collapsed.

Keep the screen on and unlocked; Shizuku and Direct ADB also need Wi-Fi and wireless debugging. This page’s **More settings** holds connection keeping, background capacity, screen orientation and requirements. “Keep … channel” is off by default: without it, re-enable the mode after restarting the app or phone. Only turning it on allows DSH Go to try restoring this connection after disconnection, reopening the app or restarting the phone. Root requests authorization again; Shizuku / Sui use existing permission and wait for their service; Direct ADB uses existing pairing. After a system force-stop, open DSH Go yourself. Folding options changes none of these permissions or keeping settings.
- **Background running**: prevents Android from turning off accessibility; opens the phone settings guide.
- **Confirmations and return**: see confirmations and task results in time; see Confirmations and return above.

## Saved phone tasks

The saved-tasks page under Phone control runs a known flow locally. Waiting, branches and bounded loops do not need an AI request per step. Page interpretation, visual targeting and repairs still need a model. Existing single-action phone tools remain available.

1. In chat, specify the goal, allowed apps, parameters and stopping point. Ask AI to inspect the actual page before generating a schema-1 task. Importing JSON creates a proposal and never starts it.
2. Review the complete flow, apps and limits. You can confirm a single run first; this does not save the task or grant lasting authority, and the proposal remains available.
3. Save a flow once it is worth reusing. Specify allowed parameter values, or explicitly allow all values accepted by the parameter definition. Choose confirmation per run or long-term authorization. Changed flows need a newly reviewed revision.
4. Start saved tasks with parameters from the task page or ask AI to call their task ID in chat. Values outside approved scopes require your approval for that run; they do not expand the saved scope.

Global automatic mode is enabled by you and persists after restart until you turn it off. It skips run confirmation within approved scopes. Declared apps, parameter scopes, app rules, system permissions and forbidden actions still apply. Turning it off does not revoke individual long-term task authorizations.

Only one local automation task runs at a time. After a pause, an out-of-scope app switch, a lock or an unavailable observation, check the current page and choose resume or stop in the task page. Resume observes the page again. A run interrupted by process exit is not replayed. If a send or submission may have happened but cannot be verified, the reason is `result-uncertain`. This release cannot directly resume that run: inspect the actual result, stop it and create the remaining flow to avoid duplicate writes.

At most two repair rounds are allowed per whole run, within the original apps and capabilities. Repairs never silently replace a saved definition; review the new revision first. Sanitized run summaries retain up to 100 records for 7 days, without message text, screen text or AI prompts. Task definitions and parameter values you explicitly save are stored locally.

## Local screenshot evidence

Local screenshot evidence is off by default on the task page and requires your confirmation to enable. It can retain images from explicit `snapshot` nodes, on-demand AI vision and failure capture. Add `snapshot` before `finish` when you need a result image. Completion does not automatically produce a result frame every time, and this is not screen recording.

Images stay on this device and are not sent automatically. Retention is limited to 10 images per run, 50 MB in total and 7 days. Tap a thumbnail on the task page to enlarge it; missing files are reported. Images may contain private content, so review them before sharing.

Turning the switch off stops future retention but keeps existing images. Confirming Clear screenshot evidence deletes all retained images while keeping tasks, text results and the switch setting. Model tools do not receive the local image file list.

## Task JSON format

This is a read-only template. `example.phone.lab` is a placeholder package, not a browser or a universal app identifier. Observe the real app to confirm its package, visible text and starting state before replacing it. Parser validation does not prove that a page works on a device.

```json
{
  "schema": 1,
  "id": "local-check",
  "name": "Read-only local page check",
  "apps": ["example.phone.lab"],
  "steps": [
    {"op": "snapshot"},
    {"op": "wait", "text": "DSH 手机操控验收页", "timeoutMs": 10000},
    {"op": "assert", "text": "DSH 手机操控验收页"},
    {"op": "finish", "result": {"status": "verified"}}
  ],
  "limits": {"timeoutMs": 60000, "maxActions": 10, "maxAiCalls": 0}
}
```

Required fields are `schema: 1`, `id`, `name`, `apps` and nonempty `steps`. Optional fields include `parameters`, `limits`, `capabilities`, `description` and `forbiddenTargets`; saving assigns the next revision. Limits are 10 apps, 200 definition nodes and 4 nesting levels. Arbitrary JavaScript, shell and Auto.js scripts are not supported.

| Operation | Required fields and meaning |
| --- | --- |
| `open` | `app`: a declared package |
| `tap`, `type` | Visible `target` text or a `selector`; `type` also needs `text` |
| `observe`, `snapshot` | Read the current page; no other required fields |
| `read_text` | `target` or `selector`, and `name` for a run-local variable |
| `scroll` | `direction`: up/down/left/right; optional `until` text and `max`, at most 20 |
| `key` | Only `key: "back"` in this release |
| `tap_xy`, `input` | A recent `observation`, `x`, `y`; `input` also needs `text` |
| `assert` | Current visible `text` condition |
| `wait` | Either a fixed `ms`, or `text` with `timeoutMs` |
| `if` | `condition`, `then`, optional `else` |
| `repeat` | `count` from 1 to 100 and body `steps`; optional `until` checked before each iteration |
| `break` | Leave the current loop |
| `set` | Run-local `name` and `value`; cannot overwrite start parameters |
| `ai` | Decision `prompt`, optional `name` for its returned value |
| `plugin` | The actual installed `tool` and declared `arguments`; wait for its real receipt |
| `finish` | Optional `result`; end the task |

Conditions use `{"text":"Done","match":"contains"}` or `{"variable":"done","equals":true}`; `not: true` inverts the result. Text matching supports `contains`, `exact` (one complete line) and `normalized` (contains after collapsing whitespace), not regular expressions. Any step can use `requireText` before acting and `expect` afterwards. A failed postcondition after a write makes the result uncertain.

`parameters` declares named values with `type` string/integer/number/boolean and optional `default`, `required`, `enum`, `min`, `max`, `maxLength`. For example: `{"message":{"type":"string","required":true,"maxLength":100}}`. `${message}` refers to a parameter or an earlier run-local value; a whole-value reference preserves its type. Plugin arguments may reference declared start parameters only, not run-local variables that could change a recipient or target.

Omitted `capabilities` are inferred from the definition. Actions subsequently supplied by AI must also be declared; for example, use `"capabilities":["observe","tap","type"]` if a checkpoint may add taps and text input. Capabilities do not remove app or parameter restrictions. `forbiddenTargets` lists targets that cannot be tapped. Tasks with forbidden targets reject coordinates because the executor cannot prove that a coordinate avoids a forbidden button.

`limits.timeoutMs` counts active time. Ordinary tasks default to 10 minutes with a 30-minute maximum; `continuous: true` defaults to 60 minutes with a 120-minute maximum. Pauses are excluded, waits included. `maxActions` defaults to and cannot exceed 500, counting native writes and plugin nodes. `maxAiCalls` defaults to and cannot exceed 20 for ordinary tasks or 50 for continuous ones, and may be 0. It counts AI checkpoints, not underlying model requests or token cost: one checkpoint can need several model turns. Fixed waits are limited to 120 seconds and text waits to 30 seconds.

## Chat tool and visual targeting

`mcp__phone__task` accepts actions `list/get/validate/propose/start/status/pause/resume/stop/ai_result/repair`. Resuming requires the user in the task page; the model cannot authorize it. Saving, authority changes and global mode also require your confirmation in the app.

AI uses `validate` for definitions, `propose` for review candidates and `start` for saved tasks. An AI node changes the run to `awaiting_ai`, with `runId`, `checkpoint` and `kind` in status. A `get` with the current `runId` reads a detached definition and bound parameters. AI must inspect the current state and return `ai_result` or `repair` for that same checkpoint. Pausing or stopping invalidates old checkpoints. Repair `steps` describe only the remaining workflow, never an implicit replay.

This release has no local OCR guarantee. When an ordinary capturable page has no usable accessibility text, an image-capable model must inspect the screenshot and target using a valid observation ID. A clear screenshot does not authorize every coordinate: app rules, forbidden targets, system protection and stale-observation checks may still refuse. Models without image input cannot perform this visual fallback.

Shopping and course flows depend on real pages and account state. Declare the stopping point, candidate count, course chapters and question scope. Template validation, timed waits or a model's verbal report do not prove an order, actual playback, credited course progress or a correct answer. Scheduling, notification triggers, boot execution and volume-key triggers are outside this release.
