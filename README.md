# Session 2 Participation — In-Class Assignment

CS6983 Phys AI, World Model and Games — Week 2 in-class activity on Unity physics (Rigidbody forces, triggers, events).

## Scene

`Session2.unity` contains:

| GameObject | Setup |
|---|---|
| Floor | Large plane / scaled cube with a collider |
| Actor | Cube with `Rigidbody` + `InClassActorController.cs` (Target dragged into Inspector) |
| Target | Empty or small marker object placed away from the Actor |
| Payload | Sphere with `Rigidbody`, **Tag = "Payload"** |
| Goal Zone | Cube with collider **Is Trigger = true** + `InClassGoalZone.cs` |
| Camera | Positioned to view the play area |
| Light | Directional light |

At runtime the Actor applies a constant force toward the Target on the ground plane. When the Payload enters the Goal Zone trigger, the run is marked successful and the `PayloadSucceeded` event fires with the time-to-success in seconds (logged to the Console).

## Scripts

- `InClassActorController.cs` — TODO: apply force toward the target → `_body.AddForce(toTarget.normalized * moveStrength)` in `FixedUpdate()`.
- `InClassGoalZone.cs` — TODOs: `IsPayload()` returns `other.CompareTag(payloadTag)`; `OnTriggerEnter` sets `_succeeded = true`, records `TimeToSuccess = Time.time - _runStartTime`, and invokes `PayloadSucceeded?.Invoke(TimeToSuccess)`.

## Experiment Observations

I modified the Rigidbody parameters of the Actor and the Payload and recorded the time-to-success (from the `PayloadSucceeded` event / Console log) for each run.

| # | Object | Parameter changed | Default → New | Time to success | Observation |
|---|---|---|---|---|---|
| 1 | Actor | Mass | 1 → 5 | _fill in_ s | Heavier actor accelerates more slowly under the same force (F = ma), so it takes longer to reach/push the payload toward the goal. |
| 2 | Actor | Linear Damping | 0 → 2 | _fill in_ s | Higher damping acts like drag: top speed drops, motion looks "heavier"/more controlled; time to success increases. |
| 3 | Payload | Mass | 1 → 5 | _fill in_ s | Heavier payload is harder for the actor to push; it resists changes in motion and may stop short of the goal zone. |
| 4 | Payload | Angular Damping | 0.05 → 5 | _fill in_ s | High angular damping kills spin quickly — the payload rolls/slides with less rotation but its linear motion is mostly unchanged. |
| 5 | _(your own)_ | | | _fill in_ s | |

### Notes
- Linear Damping = drag on translational motion; Angular Damping = drag on rotation. Both are in the Rigidbody component in the Inspector.
- Mass only matters in combination with forces (F = ma) — with no force applied, changing mass alone shows little difference.
- Tip: to compare runs fairly, reset the scene (stop/play) between runs so `ResetRun()` re-arms the timer via `OnEnable()`.
