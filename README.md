<div align="center">
    <img src="icon.png" alt="Logo" width="160" height="160">

<h3 align="center">Camera shifter</h3>

  <p align="center">
    Smoothly transition between cameras in your game.
    <br />
    ·
    <a href="https://github.com/ninetailsrabbit/CameraShifter/issues/new?assignees=ninetailsrabbit&labels=%F0%9F%90%9B+bug&projects=&template=bug_report.md&title=">Report Bug</a>
    ·
    <a href="https://github.com/ninetailsrabbit/CameraShifter/issues/new?assignees=ninetailsrabbit&labels=%E2%AD%90+feature&projects=&template=feature_request.md&title=">Request Features</a>
  </p>
</div>

<br>
<br>

The system utilizes a third, global camera to facilitate the transition between two other cameras. This temporary camera mimics the properties of the camera you want to transition to. Once the transition completes, the target camera becomes the active camera in your scene.

**Benefits:**

- Record Transitions: Easily record transition sequences between cameras for smooth playback.
- Manage Duration: Control the duration of the transition for a polished effect.

---

- [How to Use](#how-to-use)
- [Transition steps](#transition-steps)
- [Accessible variables](#accessible-variables)
- [API reference](#api-reference)
- [Signals](#signals)
- [Other plugins 🕵️](#other-plugins-️)

# How to Use

This class supports transitions for both 2D and 3D cameras. You can trigger a basic transition in two ways:

**Via method call**

- **2D:** `CameraShifter.transition_to_requested_camera_2d(from, to, duration)`
- **3D:** `CameraShifter.transition_to_requested_camera_3d(from, to, duration)`

- Replace `from `with the origin camera node.
- Replace `to` with the target camera node you want to transition to.
- Set the `duration` _(in seconds)_ to control how long the transition takes.

**_Important note:_**
If a camera transition is already in progress, attempting to trigger another transition of the same type _(2D or 3D)_ will not interrupt the ongoing one. Wait for the current transition to finish before initiating a new one.

This system can handle simultaneous transitions between cameras, but with a limitation: **only one transition can be active at a time for each camera type** _(2D or 3D)._

In simpler terms, you can't have multiple transitions happening for the same type of camera _(2D or 3D)_ at once. However, you can:

Transition between two 3D cameras while also transitioning between two separate 2D cameras in a subviewport.
This allows you to have independent transitions occurring in your 2D and 3D environments simultaneously.

# Transition steps

When a transition is recorded it will create a class `TransitionStep2D` or `TransitionStep3D` depending on the transition requested.

Here is the structure:

```bash
class TransitionStep2D:
	var from: Camera2D
	var to: Camera2D
	var duration: float

	func _init(_from: Camera2D, _to: Camera2D, _duration: float):
		from = _from
		to = _to
		duration = absf(_duration)


class TransitionStep3D:
	var from: Camera3D
	var to: Camera3D
	var duration: float

	func _init(_from: Camera3D, _to: Camera3D, _duration: float):
		from = _from
		to = _to
		duration = absf(_duration)
```

You can access the current recorded transitions with the existing variables:

```bash
var transition_steps_2d: Array[TransitionStep2D] = []
var transition_steps_3d: Array[TransitionStep3D] = []

## anywhere...
CameraShifter.transition_steps_2d
CameraShifter.transition_steps_3d
```

# Accessible variables

You can set some default values on this variables:

```bash
@export var transition_duration := 1.5 ## Default transition duration for all transitions requested
@export var remove_last_transition_step_2d_on_back := false
@export var remove_last_transition_step_3d_on_back := false

```

# API reference

`transition_to_requested_camera_2d(from: Camera2D, to: Camera2D, duration: float = transition_duration, record_transition: bool = true)`

- Transitions from the from (current) 2D camera to the to (target) 2D camera.
- Optionally specify the duration (in seconds) to control the transition speed. Defaults to a pre-defined transition_duration.
- Set record_transition to true (default) to record this step for later playback using "next" or "all steps" methods.

---

`transition_to_requested_camera_3d(from: Camera3D, to: Camera3D, duration: float = transition_duration, record_transition: bool = true):`

Similar to the 2D version, but transitions between 3D cameras.

---

`transition_to_next_camera_2d(to: Camera2D, duration: float = transition_duration):`
Transition to the provided camera from the last step recorded on `transition_steps_2d`. If there are no recorded steps, no transition occurs.

---

`transition_to_next_camera_3d(to: Camera3D, duration: float = transition_duration):`
Similar to the 2D version, but applies to 3D camera transitions.

---

`transition_to_previous_camera_2d(delete_step: bool = remove_last_transition_step_2d_on_back):`
Transition to a previous camera from the last one in `transition_step_2d`. If `delete_step` is true, this last recorded transition will be deleted from the variable `transition_step_2d`

---

`transition_to_previous_camera_3d(delete_step: bool = remove_last_transition_step_3d_on_back):`
Similar to the 2D version, but applies to 3D camera transitions.

---

`transition_to_first_camera_via_all_steps_2d(clean_steps_on_finished: bool = false):`
Transition to the first camera recorded on `transition_step_2d`. If `clean_steps_on_finished` is true, the recorded transitions will be deleted after the operation ends.

---

`transition_to_first_camera_via_all_steps_3d(clean_steps_on_finished: bool = false):`
Similar to the 2D version, but applies to 3D camera transitions.

# Signals

```bash
signal transition_2d_started(from: Camera2D, to: Camera2D, duration: float)
signal transition_2d_finished(from: Camera2D, to: Camera2D, duration: float)
signal transition_3d_started(from: Camera3D, to: Camera3D, duration: float)
signal transition_3d_finished(from: Camera3D, to: Camera3D, duration: float)
```

# Other plugins 🕵️

- Check out my [gists](https://gist.github.com/ninetailsrabbit) You might find some interesting components or configuration files there.
- Grab some useful tools for your Godot projects with [GodotWizards](https://github.com/ninetailsrabbit/GodotWizards)
- Effortless loot generation [Lootie](https://github.com/ninetailsrabbit/Lootie)
