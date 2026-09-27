![](https://user-images.githubusercontent.com/47793918/233812617-beab2e71-57b9-479e-8bff-c3931347ca40.png)

## 🚙 Relaxed Driver Monitoring Fork

This fork modifies driver monitoring (DM) to reduce false-positive alerts and prevent intrusive alarms when glancing down at the instrument cluster, navigation screen, or center console for a few seconds.

### 🎯 Key Changes
1. **Silenced Stage 1 Alert (`driverDistracted1`)**:
   - Reverted from `AudibleAlert.preAlert` back to `AudibleAlert.none`.
   - Stage 1 is visual-only ("Pay Attention" prompt) without any sound.
2. **Relaxed Pitch Downward Threshold**:
   - `_POSE_PITCH_THRESHOLD`: `0.3133` (~17.9°) ➔ `0.3800` (~21.8°, slack up to ~23.5°).
   - Downward head tilt when checking speedometer or navigation no longer triggers an immediate distracted pose flag.
3. **Smoother Distraction Filter**:
   - `_DISTRACTED_FILTER_TS`: `0.25`s ➔ `0.50`s so quick downward glances do not instantly deplete awareness.
4. **Extended Vision Policy Timeouts**:
   - Stage 1 Timeout: `3.0`s (or `5.0`s) ➔ **`7.0`s** (silent buffer before visual prompt).
   - Stage 2 Timeout: `5.0`s (or `8.0`s) ➔ **`12.0`s** (audible alarm delayed until 12s of continuous distraction).
   - Stage 3 Timeout: `11.0`s (or `13.0`s) ➔ **`17.0`s** (terminal alert).
5. **Relaxed Phone Detection Sensitivity**:
   - `_PHONE_THRESH`: `0.50` ➔ `0.65` (drastically cuts false alarms from drinking bottles, holding items, or resting hands near the chest/wheel).
6. **Traffic Light & Standstill Exemption**:
   - Awareness countdown is completely frozen at a stop (`standstill`) and recovers naturally so you aren't nagged while waiting at red lights.
7. **Steering Wheel Nudge Reset**:
   - Nudging the steering wheel immediately acknowledges the system and resets awareness to 100% under vision monitoring, letting you dismiss warnings with a gentle wheel touch.
8. **Expanded Lockout Buffer**:
   - Terminal alerts: `3` ➔ `5` strikes; terminal duration: `30`s ➔ `60`s before triggering a lockout disengagement.

---

### 📦 Installation

On your device, go to **Settings ➔ Software ➔ Uninstall Software** and enter the URL for your hardware:

#### 🚗 Comma 3X
```text
https://installer.comma.ai/nkgilley/relaxed-dm-tizi
```

#### 🚙 Comma 4
```text
https://installer.comma.ai/nkgilley/relaxed-dm-mici
```
*(Both branches are based on upstream prebuilt releases—`release-tizi` and `release-mici`—so they boot up in under 30 seconds without needing on-device compilation).*

---

### 🔄 Automatic Sync with Upstream
- **Daily Cloud Sync**: A GitHub Actions workflow ([`.github/workflows/sync-relaxed-dm.yaml`](.github/workflows/sync-relaxed-dm.yaml)) runs daily at `06:00 UTC`.
- It tracks both `release-tizi` (Comma 3X) and `release-mici` (Comma 4). Whenever upstream updates either release, the workflow automatically re-applies the DM patch and updates the respective branch.
- Your comma device will automatically receive updates over Wi-Fi.
- **Manual Trigger**: You can also manually trigger a sync anytime in GitHub under **Actions ➔ "Sync Relaxed DM with Upstream (Comma 3X & Comma 4)" ➔ "Run workflow"**.

---

### 🌿 Branches
- **`master`** (Default branch): Tracks upstream `master` + holds the GitHub Actions auto-sync workflow.
- **`relaxed-dm-tizi`**: Prebuilt release branch for **Comma 3X** with relaxed DM.
- **`relaxed-dm-mici`**: Prebuilt release branch for **Comma 4** with relaxed DM.
- **`relaxed-driver-monitoring`**: Development branch based on `master` with relaxed DM.

---

## 🌞 What is sunnypilot?
[sunnypilot](https://github.com/sunnyhaibin/sunnypilot) is a fork of comma.ai's openpilot, an open source driver assistance system. sunnypilot offers the user a unique driving experience for over 300+ supported car makes and models with modified behaviors of driving assist engagements. sunnypilot complies with comma.ai's safety rules as accurately as possible.

## 💭 Join our Community Forum
Join the official sunnypilot community forum to stay up to date with all the latest features and be a part of shaping the future of sunnypilot!
* https://community.sunnypilot.ai/

## Documentation
https://docs.sunnypilot.ai/ is your one stop shop for everything from features to installation to FAQ about the sunnypilot

## 🚘 Running on a dedicated device in a car
First, check out this list of items you'll need to [get started](https://community.sunnypilot.ai/t/getting-started-using-sunnypilot-in-your-supported-car/251).

## Installation
Next, refer to the sunnypilot community forum for [installation instructions](https://community.sunnypilot.ai/t/read-before-installing-sunnypilot/254), as well as a complete list of [Recommended Branch Installations](https://community.sunnypilot.ai/t/recommended-branch-installations/235).

## 🎆 Pull Requests
We welcome both pull requests and issues on GitHub. Bug fixes are encouraged.

Pull requests should be against the most current `master` branch.

## 📊 User Data

By default, sunnypilot uploads the driving data to comma servers. You can also access your data through [comma connect](https://connect.comma.ai/).

sunnypilot is open source software. The user is free to disable data collection if they wish to do so.

sunnypilot logs the road-facing camera, CAN, GPS, IMU, magnetometer, thermal sensors, crashes, and operating system logs.
The driver-facing camera and microphone are only logged if you explicitly opt-in in settings.

By using this software, you understand that use of this software or its related services will generate certain types of user data, which may be logged and stored at the sole discretion of comma. By accepting this agreement, you grant an irrevocable, perpetual, worldwide right to comma for the use of this data.

## Licensing

sunnypilot is released under the [MIT License](LICENSE). This repository includes original work as well as significant portions of code derived from [openpilot by comma.ai](https://github.com/commaai/openpilot), which is also released under the MIT license with additional disclaimers.

The original openpilot license notice, including comma.ai’s indemnification and alpha software disclaimer, is reproduced below as required:

> openpilot is released under the MIT license. Some parts of the software are released under other licenses as specified.
>
> Any user of this software shall indemnify and hold harmless Comma.ai, Inc. and its directors, officers, employees, agents, stockholders, affiliates, subcontractors and customers from and against all allegations, claims, actions, suits, demands, damages, liabilities, obligations, losses, settlements, judgments, costs and expenses (including without limitation attorneys’ fees and costs) which arise out of, relate to or result from any use of this software by user.
>
> **THIS IS ALPHA QUALITY SOFTWARE FOR RESEARCH PURPOSES ONLY. THIS IS NOT A PRODUCT.
> YOU ARE RESPONSIBLE FOR COMPLYING WITH LOCAL LAWS AND REGULATIONS.
> NO WARRANTY EXPRESSED OR IMPLIED.**

For full license terms, please see the [`LICENSE`](LICENSE) file.

## 💰 Support sunnypilot
If you find any of the features useful, consider becoming a [sponsor on GitHub](https://github.com/sponsors/sunnyhaibin) to support future feature development and improvements.


By becoming a sponsor, you will gain access to exclusive content, early access to new features, and the opportunity to directly influence the project's development.


<h3>GitHub Sponsor</h3>

<a href="https://github.com/sponsors/sunnyhaibin">
  <img src="https://user-images.githubusercontent.com/47793918/244135584-9800acbd-69fd-4b2b-bec9-e5fa2d85c817.png" alt="Become a Sponsor" width="300" style="max-width: 100%; height: auto;">
</a>
<br>

<h3>PayPal</h3>

<a href="https://paypal.me/sunnyhaibin0850" target="_blank">
<img src="https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif" alt="PayPal this" title="PayPal - The safer, easier way to pay online!" border="0" />
</a>
<br></br>

Your continuous love and support are greatly appreciated! Enjoy 🥰

<span>-</span> Jason, Founder of sunnypilot
