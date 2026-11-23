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

## Credits

Asimov 1 was developed by [Menlo Research](https://github.com/menloresearch/asimov-1). The linked recovery project is by OpenHorizon Labs and the Physical AI & Robotics Lab at Galgotias University. Each upstream project includes its own license terms.
