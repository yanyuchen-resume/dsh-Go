# 合并冲突报告：官方 1.3.0 ← 社区版 1.0.19

共 38 个冲突文件。基线用官方 v1.0.13（社区版的起点），做的是 git 三方合并。

每块给双方的第一行意图，完整内容在工作树里看 `<<<<<<<` 标记。


## `.gitattributes` — 1 块

**块 1**
- 官方 1.3.0: `# Nested patches are applied to upstream sources byte for byte; never convert their line e / tooling/ios/phone-self-runner/*.patch -text`
- 社区 1.0.19: `app/lib/l10n/app_en.arb text eol=lf / app/lib/l10n/app_zh.arb text eol=lf`

## `app/android/app/build.gradle.kts` — 1 块

**块 1**
- 官方 1.3.0: `if (sideBuild) applicationIdSuffix = sideSuffix / manifestPlaceholders["appLabel"] = if (sideBuild) "DSH Go 测试" else "DSH Go"`
- 社区 1.0.19: `testInstrumentationRunner = "io.github.ccch1mneyyy.dsh_app.phone.PhoneLabInstrumentation"`

## `app/android/app/src/main/kotlin/io/github/ccch1mneyyy/dsh_app/phone/NotificationGate.kt` — 1 块

**块 1**
- 官方 1.3.0: `// A heads-up fits three answers: 允许一次 / 本轮允许 / 取消. A visual scope only ever / // authorizes one step, so it gets the two-answer form instead of a 本轮 grant.`
- 社区 1.0.19: `// Three buttons fit a heads-up: 本任务免确认 makes "allow" waive the rest of the task. / val allow = when {`

## `app/android/app/src/main/kotlin/io/github/ccch1mneyyy/dsh_app/phone/PhoneContracts.kt` — 5 块

**块 1**
- 官方 1.3.0: `/** Known root packages contributing to this local observation; not pixel/UID attestation. / internal val readPackages: Set<String> = emptySet(),`
- 社区 1.0.19: `/** Real target text after viewport/ancestor clipping; null only for legacy drivers. */ / val visibleTexts: List<String>? = null,`
**块 2**
- 官方 1.3.0: `/** Internal, this BG open only: the original peer confirmed its valid VD and full target  / val backgroundOpenConfirmed: Boolean = false,`
- 社区 1.0.19: `/** Stable machine-readable failure classification when the driver knows the cause. */ / val reasonCode: String? = null,`
**块 3**
- 官方 1.3.0: `/** An omitted image argument may return an image only for this observation's poor tree. / * Old drivers and explicit two-argument callers keep their original image behavior. */`
- 社区 1.0.19: `/** Retains old test/background drivers while main-screen assertions use actual visible ta / fun hasVisibleText(snapshot: ScreenSnapshot, text: String, match: String = "contains"): Bo`
**块 4**
- 官方 1.3.0: `/** Captures only the original committed observation's editor. */ / fun prepareFocusInput(screenId: String): Either<String, FocusInput> =`
- 社区 1.0.19: `/** / * The element of the latest snapshot whose text (or id tail) is [text], for `tap` with `te`
**块 5**
- 官方 1.3.0: `val allowRound: Boolean = true, / /** Exact native task lifetime; never inferred from the visible conversation. */`
- 社区 1.0.19: `val offerTaskWaiver: Boolean = false, / /** Approval covers the explicitly displayed recipient and full message, rather than one s`

## `app/android/app/src/main/kotlin/io/github/ccch1mneyyy/dsh_app/phone/PhoneControl.kt` — 11 块

**块 1**
- 官方 1.3.0: `"phone_status_coordinate" -> R.string.phone_status_coordinate`
- 社区 1.0.19: `"phone_status_gesture" -> R.string.phone_status_gesture / "phone_status_input" -> R.string.phone_status_input`
**块 2**
- 官方 1.3.0: `"phone_confirm_background_coordinate" -> R.string.phone_confirm_background_coordinate / "phone_confirm_visual_main" -> R.string.phone_confirm_visual_main`
- 社区 1.0.19: `"phone_confirm_gesture" -> R.string.phone_confirm_gesture / "phone_confirm_input" -> R.string.phone_confirm_input`
**块 3**
- 官方 1.3.0: `"phone_log_coordinate" -> R.string.phone_log_coordinate`
- 社区 1.0.19: `"phone_log_gesture" -> R.string.phone_log_gesture / "phone_log_input_xy" -> R.string.phone_log_input_xy`
**块 4**
- 官方 1.3.0: `val skillTrace = PhoneTaskTrace() / internal val skillDrafts = PhoneSkillDrafts()`
- 社区 1.0.19: `val core = PhoneCore(this, tasks, log) / private val automation by lazy {`
**块 5**
- 官方 1.3.0: `/** Off stops control tasks after any in-flight keeper settings write; diagnostics keeps i`
- 社区 1.0.19: `private fun phoneTool(name: String, args: Map<String, Any?>): ToolOutput { / if (name == PhoneToolCatalog.TASK) {`
**块 6**
- 官方 1.3.0: `fun setPolicies(map: Map<*, *>?): Boolean { / applyLocalOcrPolicyUpdate(map,`
- 社区 1.0.19: `@Synchronized fun setPolicies(map: Map<*, *>?): Boolean {`
**块 7**
- 官方 1.3.0: `for (pkg in (p.denyPackages + p.confirmEveryWritePackages + p.alwaysAllowPackages + / PhonePolicies().denyPackages + PhonePolicies().confirmEveryWritePackages).distinct()) {`
- 社区 1.0.19: `for (pkg in (p.denyPackages + p.confirmEveryWritePackages + p.visualPackages + PhonePolici`
**块 8**
- 官方 1.3.0: `fun stopTask(reason: String, taskId: Long? = null): Boolean { / val record = if (taskId == null) sessions.all().firstOrNull { it.sessionId == null } ?: re`
- 社区 1.0.19: `fun stopTask(reason: String): Boolean { / automation.stop()`
**块 9**
- 官方 1.3.0: `fun finishTaskScope(expectedTaskId: Long): PhoneTasks.Owner? { / val record = sessions.task(expectedTaskId) ?: return null`
- 社区 1.0.19: `fun finishTaskScope(expectedTaskId: Long): PhoneTasks.Owner? = / if (automation.active) null else tasks.finishScope(expectedTaskId)`
**块 10**
- 官方 1.3.0: `sessions.all().forEach { it.tasks.expireIdle() }`
- 社区 1.0.19: `automation.tick() / tasks.expireIdle()`
**块 11**
- 官方 1.3.0: `override fun shownControls(): TaskControls? = / displaySelection.controls { task, owner, reason -> stopOwner(task, owner, reason) }`
- 社区 1.0.19: `override fun onPauseChanged(paused: Boolean) { / if (automation.active) {`

## `app/android/app/src/main/kotlin/io/github/ccch1mneyyy/dsh_app/phone/PhoneCore.kt` — 15 块

**块 1**
- 官方 1.3.0: `internal fun call(prepared: PreparedCall, executionWorker: SerialWorker, remainingMs: () - / runCall(prepared.name, prepared.args, AtomicReference<PhoneTasks.Owner?>(prepared.owner), `
- 社区 1.0.19: `private fun runCall( / name: String, args: Map<String, Any?>, callOwner: AtomicReference<PhoneTasks.Owner?>,`
**块 2**
- 官方 1.3.0: `return null / }`
- 社区 1.0.19: `val outcome = worker.run(budgetMs(name), MAX_TOTAL_MS) { scope -> / if (automated) automatedCall.set(true)`
**块 3**
- 官方 1.3.0: `return output(name, owner, out) / }`
- 社区 1.0.19: `capture?.invoke(out.screen, out.decision)`
**块 4**
- 官方 1.3.0: `private fun execute(name: String, a: Args, scope: SerialWorker.Scope, callOwner: AtomicRef / val admitted = callOwner.get()`
- 社区 1.0.19: `private fun execute(name: String, a: Args, scope: SerialWorker.Scope, callOwner: AtomicRef / if (automatedAdmission.get()?.invoke() == false) return stoppedOut(name)`
**块 5**
- 官方 1.3.0: `val displayGuard = onExecution(owner) ?: return stoppedOut(name) / executionStatus.set(displayGuard)`
- 社区 1.0.19: `val send = if (name != PhoneToolCatalog.RUN && "send" in a.keys()) { / if (name !in setOf(PhoneToolCatalog.TAP, PhoneToolCatalog.TYPE, PhoneToolCatalog.INPUT, Ph`
**块 6**
- 官方 1.3.0: `if (!current(owner)) return stoppedOut("open") / confirmSystem(plan, scope, owner, summary)?.let { return it }`
- 社区 1.0.19: `if (!tasks.isCurrent(owner) || Thread.currentThread().isInterrupted) return stoppedOut("op / if (automatedAdmission.get()?.invoke() == false) return stoppedOut("open")`
**块 7**
- 官方 1.3.0: `val summary = ui.get("phone_log_open_app", "打开 %1\$s", app.packageName) // l10n:keep test  / confirmSystem(plan, scope, owner, summary)?.let { return it }`
- 社区 1.0.19: `if (automatedAdmission.get()?.invoke() == false) return stoppedOut("open") / return afterSystem(owner, env.system().launch(app, tasks.systemDispatch(owner)), ui.get("p`
**块 8**
- 官方 1.3.0: `val snap = driver.observe(detail = a.bool("detail") == true, image = a.strictBool("image") / automaticImage = !a.provided("image"))`
- 社区 1.0.19: `val detail = a.bool("detail") == true / val image = a.bool("image") == true`
**块 9**
- 官方 1.3.0: `/** Selection insertion is distinct from type's whole-field replacement and never submits. / private fun focusText(a: Args, scope: SerialWorker.Scope, owner: PhoneTasks.Owner): Out {`
- 社区 1.0.19: `/** / * `tap` with `text`: the element is found by the words the person sees, then the ordinary`
**块 10**
- 官方 1.3.0: `fun writeVerdict() = safety.checkWrite(kind, target, pkg, appLabel, typed, submit, point,  / val verdict = writeVerdict()`
- 社区 1.0.19: `val verdict = safety.checkWrite(kind, target, pkg, appLabel, typed, submit, point, note, b / val decision = when (val r = safety.resolve(verdict, scope)) {`
**块 11**
- 官方 1.3.0: `fun actionSourcesAllowed(sources: Set<String>): Boolean { / if (policyFailure.get() != null) return false`
- 社区 1.0.19: `if (expectedPackage != null && driver.foregroundPackage() != expectedPackage) return Out(" / if (automatedAdmission.get()?.invoke() == false) return stoppedOut(name)`
**块 12**
- 官方 1.3.0: `decision = if (res.ok) decision else policyFailure.get()?.decision ?: "failed",`
- 社区 1.0.19: `decision = res.reasonCode ?: if (res.ok) decision else "failed",`
**块 13**
- 官方 1.3.0: `WriteKind.TAP_XY -> ui.get("phone_status_coordinate", "执行截图坐标手势") // l10n:keep test defaul`
- 社区 1.0.19: `WriteKind.TAP_XY -> ui.get("phone_status_tap_xy", "点截图上的位置") // l10n:keep test default / WriteKind.GESTURE -> ui.get("phone_status_gesture", "手势") // l10n:keep test default`
**块 14**
- 官方 1.3.0: `val action = PhoneAction.Key(sid ?: "${which.prefix}0", key) / val ticket = skillTrace.beginAction(owner, action, null)`
- 社区 1.0.19: `if (expectedPackage != null && driver.foregroundPackage() != expectedPackage) return Out(" / if (automatedAdmission.get()?.invoke() == false) return stoppedOut("key")`
**块 15**
- 官方 1.3.0: `private val WRITES = setOf(FOCUS_TEXT, PhoneToolCatalog.TAP, PhoneToolCatalog.TYPE, PhoneT / /** An uncertain effect call tightens draft eligibility; it does not prove input was dispa`
- 社区 1.0.19: `private val WRITES = setOf(PhoneToolCatalog.TAP, PhoneToolCatalog.TYPE, PhoneToolCatalog.T`

## `app/android/app/src/main/kotlin/io/github/ccch1mneyyy/dsh_app/phone/PhonePlugin.kt` — 2 块

**块 1**
- 官方 1.3.0: `"status" -> control.status { complete(result, it) } / "androidShellStatus" -> async(result, shortControl = true) { control.shell.status() }`
- 社区 1.0.19: `"automationList", "automationGet", "automationPropose", "automationSave", / "automationStart", "automationStartOnce", "automationStatus", "automationPause", "automati`
**块 2**
- 官方 1.3.0: `/** Only this local attachment can admit a run; physical work has its own coordinator work / private fun selfCheckCall(call: MethodCall, result: MethodChannel.Result) {`
- 社区 1.0.19: `private fun handoffAutomation(action: String, args: Map<String, Any?>): Map<String, Any?>  / if (action in setOf("start", "start_once")) {`

## `app/android/app/src/main/kotlin/io/github/ccch1mneyyy/dsh_app/phone/PhonePromptUi.kt` — 2 块

**块 1**
- 官方 1.3.0: `fun label(ctx: Context, choice: PromptChoice): String = AppStrings.get(ctx, when (choice)  / PromptChoice.ALLOW_ONCE -> R.string.phone_prompt_allow_once`
- 社区 1.0.19: `fun label(ctx: Context, choice: PromptChoice, content: PromptContent? = null): String = Ap / PromptChoice.ALLOW_ONCE -> if (content?.native?.sendScope == true) R.string.phone_prompt_a`
**块 2**
- 官方 1.3.0: `val b = PromptButton(ctx, armedAt) { p.live }.apply { / text = label(ctx, choice)`
- 社区 1.0.19: `val b = PromptButton(ctx, armedAt, p).apply { / text = label(ctx, choice, p.ticket.content)`

## `app/android/app/src/main/kotlin/io/github/ccch1mneyyy/dsh_app/phone/PhoneTasks.kt` — 5 块

**块 1**
- 官方 1.3.0: `// 本轮允许: app + 动作类别 granted for this turn only; endLocked clears it. / private val grants = LinkedHashSet<String>()`
- 社区 1.0.19: `private var sendApproval: Pair<Long, String>? = null`
**块 2**
- 官方 1.3.0: `synchronized(lock) { lifecycle++; admissionEpoch++ } / fire()`
- 社区 1.0.19: `synchronized(lock) { lifecycle++; sendApproval = null }`
**块 3**
- 官方 1.3.0: `fun confirmIfCurrent(owner: Owner, pkg: String? = null, waiveTask: Boolean = false, grant:`
- 社区 1.0.19: `fun confirmIfCurrent(owner: Owner, pkg: String? = null, waiveTask: Boolean = false, send: `
**块 4**
- 官方 1.3.0: `if (grant != null) grants += grant`
- 社区 1.0.19: `if (send != null) sendApproval = lifecycle to send.fingerprint`
**块 5**
- 官方 1.3.0: `grants.clear() / visualScopes.clear(); visualGrants.clear(); visualLifecycle = lifecycle`
- 社区 1.0.19: `sendApproval = null`

## `app/android/app/src/main/kotlin/io/github/ccch1mneyyy/dsh_app/phone/PhoneToolCatalog.kt` — 3 块

**块 1**
- 官方 1.3.0: `"读当前屏幕：每行一个元素，[编号] 只在本屏有效。detail:true 附坐标，结果可能截断；image:true 请求截图，安全或能力限制时可能不附图（主屏可带编号，后台图不 / linkedMapOf(`
- 社区 1.0.19: `"读当前屏幕：每行一个元素，[编号] 只在本屏有效。detail:true 附元素边界（左上-右下）；image:true 附标号截图；until 是等这段文字出现再返回；back / linkedMapOf(`
**块 2**
- 官方 1.3.0: `"按本任务最近一次实际返回截图的像素坐标操作，只在元素里找不到目标时用。action 默认为 tap；long 长按；swipe 从 (x,y) 滑到 (x2,y2)。主屏用 s `
- 社区 1.0.19: `"按最新截图的像素坐标点击，文字树不可用但截图正常时可用。每 App 首次确认视觉权限，此后遵守普通手机操控授权。截图、前台或几何状态失效时拒绝并要求重新看图。",`
**块 3**
- 官方 1.3.0: `// l10n:keep agent tool catalog / "action" to enum(listOf("tap", "long", "swipe"), "操作类型，省略为 tap；long 为长按，swipe 为滑动"),`
- 社区 1.0.19: `"send" to SEND_SCOPE,`

## `app/android/app/src/main/kotlin/io/github/ccch1mneyyy/dsh_app/phone/SafetyGate.kt` — 10 块

**块 1**
- 官方 1.3.0: `/** 始终允许: the master switch for [alwaysAllowPackages]. Off by default. */ / val alwaysAllowEnabled: Boolean = false,`
- 社区 1.0.19: `val visualPackages: List<String> = emptyList(),`
**块 2**
- 官方 1.3.0: `"alwaysAllowEnabled" to alwaysAllowEnabled, / "alwaysAllowPackages" to alwaysAllowPackages,`
- 社区 1.0.19: `"visualPackages" to visualPackages,`
**块 3**
- 官方 1.3.0: `denyPackages = list("denyPackages", base.denyPackages), / confirmEveryWritePackages = list("confirmEveryWritePackages", base.confirmEveryWritePackag`
- 社区 1.0.19: `list("denyPackages", base.denyPackages), / list("confirmEveryWritePackages", base.confirmEveryWritePackages),`
**块 4**
- 官方 1.3.0: `class Confirm( / val request: ConfirmRequest,`
- 社区 1.0.19: `class Confirm(val request: ConfirmRequest, val packageName: String?, val reason: String, v`
**块 5**
- 官方 1.3.0: `coordinate: PhoneAction.TapXY? = null, / screen: PhoneScreen = PhoneScreen.MAIN,`
- 社区 1.0.19: `/** What a gesture does, in words (it has no single point to name). */ / note: String? = null,`
**块 6**
- 官方 1.3.0: `val eligibleVisual = kind == WriteKind.TAP_XY && coordinate != null && eligibleCoordinate( / val forceOnce = kind == WriteKind.TAP_XY && !eligibleVisual`
- 社区 1.0.19: `if (waived(p) && !batchApproved) return Verdict.Allow / val everyWrite = pkg != null && pkg in p.confirmEveryWritePackages`
**块 7**
- 官方 1.3.0: `if (v.onlyOnce && decision != Decision.ALLOW_ONCE && decision != Decision.DENY && / decision != Decision.STOP && decision != Decision.UNAVAILABLE)`
- 社区 1.0.19: `if (v.reason == "visual-access" && decision == Decision.ALLOW_APP_FOR_TASK) { / val pkg = v.packageName ?: return stopped()`
**块 8**
- 官方 1.3.0: `val approved = pkg?.takeUnless { it in policies().confirmEveryWritePackages } / val waive = policies().confirmMode == ConfirmMode.TASK`
- 社区 1.0.19: `val approved = pkg?.takeUnless { v.send != null || it in policies().confirmEveryWritePacka / if (tasks.confirmIfCurrent(owner, pkg = approved, send = v.send)) Result.Go("confirmed-app`
**块 9**
- 官方 1.3.0: `return s.copy(text = "屏幕 ${s.id}$app\n$refusal", image = null, imageMime = null, focus = n / }`
- 社区 1.0.19: `return s.copy(text = "屏幕 ${s.id}$app\n$refusal", image = null, imageMime = null, visibleTe / }`
**块 10**
- 官方 1.3.0: `WriteKind.TAP_XY -> when (coordinate?.gesture) { / PhoneAction.CoordinateGesture.LONG -> uiText.get("phone_confirm_coordinate_long", "在「%1\$s`
- 社区 1.0.19: `WriteKind.TAP_XY -> uiText.get("phone_confirm_tap_xy", "在「%1\$s」点截图上的位置 (%2\$s, %3\$s)", a / WriteKind.INPUT -> uiText.get("phone_confirm_input", "在「%1\$s」按截图上的位置 (%2\$s, %3\$s) 输入：\"`

## `app/android/app/src/main/kotlin/io/github/ccch1mneyyy/dsh_app/phone/SystemApi.kt` — 1 块

**块 1**
- 官方 1.3.0: `/** Presence differs from the existing non-null has(): explicit null must not request AUTO / fun provided(key: String) = map.containsKey(key)`
- 社区 1.0.19: `fun keys(): Set<String> = map.keys / fun number(key: String): Number? = map[key] as? Number`

## `app/android/app/src/main/kotlin/io/github/ccch1mneyyy/dsh_app/phone/a11y/Actuator.kt` — 1 块

**块 1**
- 官方 1.3.0: ``
- 社区 1.0.19: `/** ACTION_PASTE on the field, when it offers one: a WebView textarea often only takes tex / fun paste(node: AccessibilityNodeInfo): Boolean {`

## `app/android/app/src/main/kotlin/io/github/ccch1mneyyy/dsh_app/phone/a11y/ControlOverlay.kt` — 1 块

**块 1**
- 官方 1.3.0: `// 允许一次 is the main answer; 本轮允许 (same app and kind, this turn only) stays beside 取消. / // A visual scope is its own grant: allow visual taps for this task, or only this one step`
- 社区 1.0.19: `// 本任务免确认: the main choice waives the rest of the task. Otherwise, on the first action / // in an app, the main choice allows that app for the rest of the task so later steps ther`

## `app/android/app/src/main/kotlin/io/github/ccch1mneyyy/dsh_app/phone/a11y/MainScreenDriver.kt` — 13 块

**块 1**
- 官方 1.3.0: `import io.github.ccch1mneyyy.dsh_app.phone.PhoneControl`
- 社区 1.0.19: `import io.github.ccch1mneyyy.dsh_app.phone.Either / import io.github.ccch1mneyyy.dsh_app.phone.GesturePlan`
**块 2**
- 官方 1.3.0: `import io.github.ccch1mneyyy.dsh_app.phone.VisualScope / import io.github.ccch1mneyyy.dsh_app.phone.VisualTarget`
- 社区 1.0.19: `import io.github.ccch1mneyyy.dsh_app.phone.TextSelector / import io.github.ccch1mneyyy.dsh_app.phone.TargetLookup`
**块 3**
- 官方 1.3.0: `val visualEvidence: MainVisualEvidence?, / val readPackages: Set<String>,`
- 社区 1.0.19: `val visibleTexts: List<String>, / val eventCount: Long,`
**块 4**
- 官方 1.3.0: `imageReadPackages = ImageReadSources.knownPackages(cap, shot, ownPackage) / snap.ocrOnlyEpoch = null`
- 社区 1.0.19: `snap.image = shot.geometry / snap.visualFrame = shot.frame`
**块 5**
- 官方 1.3.0: `if (snap.screen.poorTree) text.append("\n").append(formatted?.text ?: LocalOcrBoundary.mes / if (secure) clearFocusCandidate()`
- 社区 1.0.19: `toSnapshot(snap, text.toString()).copy(secure = secure, image = bytes, imageMime = mime, / observationReason = if (secure) "screenshot-protected" else observationReason(snap))`
**块 6**
- 官方 1.3.0: `is PhoneAction.Tap -> tap(action, gen, allowed, actionSourcesAllowed) / is PhoneAction.Type -> type(action, gen, allowed)`
- 社区 1.0.19: `is PhoneAction.Tap -> tap(action, gen) / is PhoneAction.Type -> type(action, gen)`
**块 7**
- 官方 1.3.0: `?: return ActionResult(false, "屏幕 $screenId 里没有编号 $elementId（共 ${snap.screen.elements.size / val cap = capture(typed = typedNavigation && el.packageName == MarkorNavigation.PACKAGE)`
- 社区 1.0.19: `?: return ActionResult(false, "屏幕 $screenId 里没有编号 $elementId（共 ${snap.screen.elements.size / val cap = capture()`
**块 8**
- 官方 1.3.0: `val current = observation(cap) / if (!current.reusableCoordinates(snap.observation)) {`
- 社区 1.0.19: `if (!reusableCoordinates(cap, snap)) return ActionResult(false, "无法确认当前界面仍与上一屏一致，没有滚动。请先看新`
**块 9**
- 官方 1.3.0: `if (snap.hidden || snap.unreadable) return ActionResult(false, "这一屏读不到内容（可能受敏感视图策略限制），不盲点。 / val end = if (a.gesture == PhoneAction.CoordinateGesture.SWIPE) {`
- 社区 1.0.19: ``
**块 10**
- 官方 1.3.0: `val current = observation(cap) / if (!current.reusableCoordinates(snap.observation)) {`
- 社区 1.0.19: `if (!reusableCoordinates(cap, snap, listOf(a.x to a.y))) return ActionResult(false, "无法确认当`
**块 11**
- 官方 1.3.0: `/** Debug-only, read-only material; never publishes a model snapshot or performs input. */ / internal fun navigationMaterial(): NavigationMaterials.Result {`
- 社区 1.0.19: `private fun capture(): Capture = TreeCapture.capture(service, screenBox(), ownPackage, CAP`
**块 12**
- 官方 1.3.0: `visualEvidence(cap), cap.windows.mapNotNull { it.packageName }.toSet(),`
- 社区 1.0.19: `ScreenFormat.index(cap.windows).filter { it.kind != ElementKind.LIST }.map { it.fullText } / cap.eventCount, cap.takenAt,`
**块 13**
- 官方 1.3.0: `ScreenSnapshot(s.id, text, s.appLabel, s.packageName, secure = s.hidden, poorTree = s.scre`
- 社区 1.0.19: `ScreenSnapshot(s.id, text, s.appLabel, s.packageName, poorTree = s.screen.poorTree, / visibleTexts = s.visibleTexts, observationReason = observationReason(s))`

## `app/android/app/src/main/kotlin/io/github/ccch1mneyyy/dsh_app/phone/a11y/PhoneA11yService.kt` — 2 块

**块 1**
- 官方 1.3.0: `internal val focusInputObservation = FocusInputObservation() / private var focusInputMethod: FocusInputMethod33? = null`
- 社区 1.0.19: `internal val foregroundIdentity = ForegroundWindowIdentity()`
**块 2**
- 官方 1.3.0: `// A keyboard coming or going moves the control clear of it. / if (event?.eventType == AccessibilityEvent.TYPE_WINDOWS_CHANGED) overlay?.onWindowsChanged`
- 社区 1.0.19: `if (event != null && event.eventType in setOf(AccessibilityEvent.TYPE_WINDOW_STATE_CHANGED / foregroundIdentity.record(event.windowId, event.packageName?.toString(), android.os.System`

## `app/android/app/src/main/kotlin/io/github/ccch1mneyyy/dsh_app/phone/a11y/ScreenShooter.kt` — 3 块

**块 1**
- 官方 1.3.0: `val observation: ImageObservation? = null, / val localText: LocalOcrResult = LocalOcrResult.Unavailable(LocalOcrResult.Reason.NO_FRAME)`
- 社区 1.0.19: `val frame: VisualFrame? = null, / )`
**块 2**
- 官方 1.3.0: `if (looksBlack(bitmap)) return Shot(null, null, null, true, "截图全黑，可能是禁止截图的页面") / val cancelled = { Thread.currentThread().isInterrupted || localTextCancelled() ||`
- 社区 1.0.19: `if (looksBlack(bitmap)) return Shot(null, null, null, false, "截图接近全黑，无法判断画面内容；尚未确认系统禁止截图")`
**块 3**
- 官方 1.3.0: `draw(scaled, marks, secrets, origin, sx, sy) / val out = ByteArrayOutputStream()`
- 社区 1.0.19: `val sx = w.toFloat() / origin.width.coerceAtLeast(1) / val sy = h.toFloat() / origin.height.coerceAtLeast(1)`

## `app/android/app/src/main/kotlin/io/github/ccch1mneyyy/dsh_app/phone/a11y/TreeCapture.kt` — 4 块

**块 1**
- 官方 1.3.0: `/** Whole enumeration, before the formatting loop can stop at an opaque window. */ / val visualWindowId: Int? = null,`
- 社区 1.0.19: `/** Event generation sampled before traversal; an event during capture prevents reuse. */ / val eventCount: Long = -1,`
**块 2**
- 官方 1.3.0: `fun capture(service: AccessibilityService, screen: Box, ownPackage: String, budgetMs: Long / val st = State(SystemClock.uptimeMillis() + budgetMs, typedTransitions, typedEvents)`
- 社区 1.0.19: `fun capture(service: AccessibilityService, screen: Box, ownPackage: String, budgetMs: Long / val st = State(SystemClock.uptimeMillis() + budgetMs)`
**块 3**
- 官方 1.3.0: `// Conservative whole-window facts. Only our own confirmed overlay is ignored; small / // system chrome is retained as an occluder, never silently treated as target-free space.`
- 社区 1.0.19: `// A docked IME can cover editors even when their accessibility bounds stay unchanged. / val keyboardTop = all.orEmpty().filter { it.type == AccessibilityWindowInfo.TYPE_INPUT_MET`
**块 4**
- 官方 1.3.0: `visualWindowId = visual?.first, / visualComplete = visual?.second == true && st.visualComplete && !st.truncated && !st.parti`
- 社区 1.0.19: `eventCount = eventCount,`

## `app/android/app/src/main/kotlin/io/github/ccch1mneyyy/dsh_app/phone/a11y/UiTree.kt` — 1 块

**块 1**
- 官方 1.3.0: `/** Secret fields in the whole captured tree, including nodes omitted from the text listin / internal fun secretFieldBounds(windows: List<UiWindow>): List<Box> {`
- 社区 1.0.19: `/** Clips descendants before their words can be merged into a visible clickable parent. */ / internal object VisibleTree {`

## `app/android/app/src/main/kotlin/io/github/ccch1mneyyy/dsh_app/phone/background/BackgroundDisplay.kt` — 3 块

**块 1**
- 官方 1.3.0: `import io.github.ccch1mneyyy.dsh_app.phone.PhoneAppIdentity`
- 社区 1.0.19: `import io.github.ccch1mneyyy.dsh_app.phone.Either`
**块 2**
- 官方 1.3.0: `import io.github.ccch1mneyyy.dsh_app.phone.VisualScope / import io.github.ccch1mneyyy.dsh_app.phone.VisualBounds`
- 社区 1.0.19: `import io.github.ccch1mneyyy.dsh_app.phone.TextSelector`
**块 3**
- 官方 1.3.0: `is PhoneAction.TapXY -> return performCoordinates(action, target, allowed, after)`
- 社区 1.0.19: `// l10n:keep agent screen protocol / is PhoneAction.TapXY -> return ActionResult(false, "后台屏按元素编号操作，不用坐标")`

## `app/android/app/src/main/res/values-zh/phone_prompt_strings.xml` — 1 块

**块 1**
- 官方 1.3.0: `<string name="phone_prompt_allow_app">本轮允许</string> / <string name="phone_prompt_reject">取消</string>`
- 社区 1.0.19: `<string name="phone_prompt_allow_send">允许本次发送</string> / <string name="phone_prompt_allow_send_visual">允许本次发送并记住视觉权限</string>`

## `app/android/app/src/main/res/values/phone_prompt_strings.xml` — 1 块

**块 1**
- 官方 1.3.0: `<string name="phone_prompt_allow_app">Allow this turn</string> / <string name="phone_prompt_reject">Cancel</string>`
- 社区 1.0.19: `<string name="phone_prompt_allow_send">Allow this send</string> / <string name="phone_prompt_allow_send_visual">Allow this send and remember visual access</`

## `app/android/app/src/test/kotlin/io/github/ccch1mneyyy/dsh_app/phone/PhoneToolSchemaTest.kt` — 1 块

**块 1**
- 官方 1.3.0: `assertEquals(15, PhoneToolCatalog.tools.size) / val tap = PhoneToolCatalog.tools.single { it.name == PhoneToolCatalog.TAP_XY }`
- 社区 1.0.19: `assertEquals(17, PhoneToolCatalog.tools.size) / val tap = PhoneToolCatalog.tools.single { it.name == PhoneToolCatalog.TAP_XY }`

## `app/assets/skills/dsh-go-guide/README.md` — 3 块

**块 1**
- 官方 1.3.0: `> dsh-go 1.3.0`
- 社区 1.0.19: `> dsh-go 1.0.19`
**块 2**
- 官方 1.3.0: `- [手机操控](zh/04-phone-control.md)：开启、悬浮条、停止、确认方式、始终允许、后台确认与返回、应用名单、解除限制、不打扰模式`
- 社区 1.0.19: `- [手机操控](zh/04-phone-control.md)：开启、悬浮条、确认方式、应用名单、不打扰模式、自动任务保存与复用、任务 JSON 和视觉定位`
**块 3**
- 官方 1.3.0: `- [Phone control](en/04-phone-control.md): turning it on, floating bar, stopping, confirma`
- 社区 1.0.19: `- [Phone control](en/04-phone-control.md): turning it on, floating bar, confirmations, app`

## `app/assets/skills/dsh-go-guide/zh-04-phone-control.md` — 1 块

**块 1**
- 官方 1.3.0: `## iPhone / iOS / <!-- nav: commonSettings,hubSettingsPhoneControl -->`
- 社区 1.0.19: `## 普通聊天中的连续执行 / 直接在聊天里说明目标即可，不必先建立自动任务。例如：“通过微信界面向云星发送你好，先核对联系人；看不到元素时用截图判断。”机器人插件是另一个可选通道，不代表微信界面已经操作成功。`

## `app/lib/l10n/app_en.arb` — 1 块

**块 1**
- 官方 1.3.0: `"androidShellTitle": "Android Shell", / "androidShellEntryHint": "Separate system-command channel, off by default",`
- 社区 1.0.19: `"automationTitle": "Saved phone tasks", / "automationSubtitle": "Save a working flow and run it with new parameters",`

## `app/lib/l10n/app_zh.arb` — 1 块

**块 1**
- 官方 1.3.0: `"androidShellTitle": "Android Shell", / "androidShellEntryHint": "独立的系统命令通道，默认关闭",`
- 社区 1.0.19: `"automationTitle": "自动任务", / "automationSubtitle": "保存成功流程，填写参数后重复运行",`

## `app/lib/l10n/gen/app_localizations.dart` — 1 块

**块 1**
- 官方 1.3.0: `/// No description provided for @androidShellTitle. / ///`
- 社区 1.0.19: `/// No description provided for @automationTitle. / ///`

## `app/lib/l10n/gen/app_localizations_en.dart` — 1 块

**块 1**
- 官方 1.3.0: `String get androidShellTitle => 'Android Shell'; / @override`
- 社区 1.0.19: `String get automationTitle => 'Saved phone tasks'; / @override`

## `app/lib/l10n/gen/app_localizations_zh.dart` — 1 块

**块 1**
- 官方 1.3.0: `String get androidShellTitle => 'Android Shell'; / @override`
- 社区 1.0.19: `String get automationTitle => '自动任务'; / @override`

## `app/lib/phone/phone_channel.dart` — 2 块

**块 1**
- 官方 1.3.0: `this.alwaysAllowEnabled = false, / this.alwaysAllowPackages = const [],`
- 社区 1.0.19: `this.visualPackages = const [],`
**块 2**
- 官方 1.3.0: `// Only an explicit true turns it on; a missing or malformed value is off. / alwaysAllowEnabled: m['alwaysAllowEnabled'] == true,`
- 社区 1.0.19: `visualPackages: _strings(m['visualPackages']),`

## `app/lib/ui/phone/phone_control_screen.dart` — 2 块

**块 1**
- 官方 1.3.0: `import 'android_shell_page.dart';`
- 社区 1.0.19: `import 'automation_page.dart';`
**块 2**
- 官方 1.3.0: `), / if (status.taskInterruption.visible) ...[`
- 社区 1.0.19: `], / ),`

## `app/pubspec.yaml` — 1 块

**块 1**
- 官方 1.3.0: `version: 1.3.0+1`
- 社区 1.0.19: `version: 1.0.19+1`

## `docs/changelog.md` — 2 块

**块 1**
- 官方 1.3.0: `## 1.3.0 · 2026-11-21 / ### 中文`
- 社区 1.0.19: `## 1.0.19 · 2026-11-16 候选 / Candidate / ### 中文`
**块 2**
- 官方 1.3.0: `- **更新前请注意：这一版的运行时有更新，所以还是下整包。** 内置引擎仍是 DSH 0.2.2-rc.1，变的是我们给它打的补丁和桥接层。升级后 Linux 环境里只保留 `/ / - **这一版没有在真机上验证过。** 后台屏实时预览、多个后台任务、按截图坐标操作、本机文字识别、扫码配对和伙伴小屋的新功能都只经过自动测试和编译检查。遇到问题请开 issue `
- 社区 1.0.19: `- **这一版是社区 Fork，基线是上游 v1.0.13。** 运行环境与上游 v1.0.13 一致（rootfs 取自官方 v1.0.13 的 arm64 安装包），所以不用重 / - **手机操控新增第 14 个工具 `gesture`：只按标号截图的像素坐标模拟手指。** 支持任意毫秒长按（`long` 的 `duration` 可写 9000，以前长按被`

## `docs/guide/README.md` — 3 块

**块 1**
- 官方 1.3.0: `> dsh-go 1.3.0`
- 社区 1.0.19: `> dsh-go 1.0.19`
**块 2**
- 官方 1.3.0: `- [手机操控](zh/04-phone-control.md)：开启、悬浮条、停止、确认方式、始终允许、后台确认与返回、应用名单、解除限制、不打扰模式`
- 社区 1.0.19: `- [手机操控](zh/04-phone-control.md)：开启、悬浮条、确认方式、应用名单、不打扰模式、自动任务保存与复用、任务 JSON 和视觉定位`
**块 3**
- 官方 1.3.0: `- [Phone control](en/04-phone-control.md): turning it on, floating bar, stopping, confirma`
- 社区 1.0.19: `- [Phone control](en/04-phone-control.md): turning it on, floating bar, confirmations, app`

## `docs/guide/zh/04-phone-control.md` — 1 块

**块 1**
- 官方 1.3.0: `## iPhone / iOS / <!-- nav: commonSettings,hubSettingsPhoneControl -->`
- 社区 1.0.19: `## 普通聊天中的连续执行 / 直接在聊天里说明目标即可，不必先建立自动任务。例如：“通过微信界面向云星发送你好，先核对联系人；看不到元素时用截图判断。”机器人插件是另一个可选通道，不代表微信界面已经操作成功。`

## `runtime/build-runtime.sh` — 4 块

**块 1**
- 官方 1.3.0: `UBUNTU_DEB_FALLBACK="https://launchpad.net/ubuntu/+archive/primary/+files"`
- 社区 1.0.19: `# Preserve locked packages after the live archive removes superseded versions. / UBUNTU_SNAPSHOT="https://snapshot.ubuntu.com/ubuntu/20261101T000000Z"`
**块 2**
- 官方 1.3.0: `# Handle transport failure here: callers include command substitutions, where / # errexit is disabled, and a conditional fetch call would disable it too.`
- 社区 1.0.19: `log "download $url" / curl -fL --retry 3 --retry-delay 2 --connect-timeout 20 -sS -o "$dest.part" "$url" || retu`
**块 3**
- 官方 1.3.0: `local file / if [[ -n "$pin" ]]; then`
- 社区 1.0.19: `local input="$HERE/inputs/termux/$name" / if [[ -f "$input" ]]; then`
**块 4**
- 官方 1.3.0: `# Superseded versions can leave the mirror; keep the locked bytes and hash. / fetch "$mirror/$file" "$CACHE/ubuntu/debs/$name" "$sum" "$UBUNTU_DEB_FALLBACK/$name"`
- 社区 1.0.19: `fetch "$mirror/$file" "$CACHE/ubuntu/debs/$name" "$sum" \ / || fetch "$UBUNTU_SNAPSHOT/$file" "$CACHE/ubuntu/debs/$name" "$sum"`

## `runtime/tests/test_runtime_fetch.py` — 2 块

**块 1**
- 官方 1.3.0: `"""Exercise the production shell download path with deterministic local transport failures / import hashlib`
- 社区 1.0.19: `#!/usr/bin/env python3 / """Exercise the real runtime download functions with curl replaced; no network/build."""`
**块 2**
- 官方 1.3.0: `SCRIPT = Path(__file__).resolve().parents[1] / "build-runtime.sh" / class RuntimeFetchTest(unittest.TestCase):`
- 社区 1.0.19: `def find_bash(): / if os.name == 'nt':`

---
冲突块合计：114
