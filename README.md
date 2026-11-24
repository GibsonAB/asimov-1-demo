# Asimov 1 Demo

Resources for the Asimov Reel by Gibson Antony Binoy. The MuJoCo close-ups show the leg, shoulder, elbow, and waist being posed manually.

## Open the robot model

- [Official Asimov 1 repository](https://github.com/menloresearch/asimov-1) — CAD, electrical design, and MuJoCo model.
- [Model fork used for the Reel](https://github.com/svgop/asimov-v1) — the version used for the joint close-ups.
- [Menlo’s assembly manual and parts list](https://docs.menlo.ai/asimov/1) — start here if you want to build the robot.

To reproduce the joint close-ups, load the model XML in the standalone MuJoCo viewer. Pause the simulation, open **Joint**, and adjust one joint at a time. Keep the model’s mesh assets alongside the XML in their original directory structure.

## Train or try a policy

| Goal | Resource |
| --- | --- |
| Train a locomotion controller | [Menlo’s Cyclotron training framework](https://github.com/menloresearch/cyclotron) |
| Try safe-fall and get-up in simulation | [OpenHorizon’s demo, runner, and training code](https://github.com/openhorizon-labs/asimov-v1-policies) |
| Download the trained recovery policies | [OpenHorizon’s ONNX files](https://huggingface.co/openhorizon-labs/asimov1-getup-safefall) |

The OpenHorizon recovery policies are simulation-only. Follow their setup instructions for the compatible model, policy configuration, and runner.

## Run the walking demo

This CPU MuJoCo demo walks using Menlo’s policy, applies a simulated push, and uses OpenHorizon’s safe-fall and get-up policies when needed. Use the Menlo model revision pinned by the OpenHorizon policies repository. That revision contains the MJCF path used below. See the [upstream demo and policy files](https://huggingface.co/openhorizon-labs/asimov1-getup-safefall#run-it).

### One-time setup for both launch options

On Ubuntu, have Git and Python with venv support installed. These commands use a fresh checkout under your home directory.

```bash
cd ~
python3 -m venv .venvs/asimov-walk
source .venvs/asimov-walk/bin/activate
python -m pip install numpy mujoco onnxruntime huggingface_hub
hf download openhorizon-labs/asimov1-getup-safefall \
  run_policy_mujoco.py policies/safefall.onnx policies/safefall_policy_spec.json \
  policies/getup.onnx policies/getup_policy_spec.json \
  --local-dir asimov1-getup-safefall
cd asimov1-getup-safefall
hf download Menlo/asimov1-locomotion-0818 policy.onnx --local-dir menlo-walk
GIT_LFS_SKIP_SMUDGE=1 git clone --no-checkout --filter=blob:none \
  https://github.com/menloresearch/asimov-1.git
GIT_LFS_SKIP_SMUDGE=1 git -C asimov-1 checkout --detach 35ae7b3581ce36762bfddb00b8fc5e7c957ee7fa
```

### Option 1 · Robotics Ecosystem 2027

In **Tools → MuJoCo**, select **Run project controller or policy**. Use absolute paths, replacing `<home>` with your home directory.

| Field | Value |
| --- | --- |
| Project runner | `<home>/asimov1-getup-safefall/run_policy_mujoco.py` |
| Working directory | `<home>/asimov1-getup-safefall` |
| Python launcher under Advanced | `<home>/.venvs/asimov-walk/bin/python` |

Leave the optional ROS 2 telemetry bridge off for this standalone simulation. Paste the following into **Arguments**, one entry per line, then select **Review and launch**.

```text
--mjcf
asimov-1/sim-model/xmls/asimov_1.xml
--mode
walk
--walk-onnx
menlo-walk/policy.onnx
--onnx
policies/safefall.onnx
--spec
policies/safefall_policy_spec.json
--getup-onnx
policies/getup.onnx
--getup-spec
policies/getup_policy_spec.json
--view
```

### Option 2 · Terminal

From a new terminal, activate the same environment and start the runner.

```bash
source "$HOME/.venvs/asimov-walk/bin/activate"
cd "$HOME/asimov1-getup-safefall"
python run_policy_mujoco.py \
  --mjcf asimov-1/sim-model/xmls/asimov_1.xml \
  --mode walk \
  --walk-onnx menlo-walk/policy.onnx \
  --onnx policies/safefall.onnx \
  --spec policies/safefall_policy_spec.json \
  --getup-onnx policies/getup.onnx \
  --getup-spec policies/getup_policy_spec.json \
  --view
```

## Credits

Asimov 1 was developed by [Menlo Research](https://github.com/menloresearch/asimov-1). The linked recovery project is by OpenHorizon Labs and the Physical AI & Robotics Lab at Galgotias University. Each upstream project includes its own license terms.
