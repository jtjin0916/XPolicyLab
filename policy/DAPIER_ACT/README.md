# DAPIER_ACT — preserved SO-101 evaluation adapter

This fork preserves the working custom multimodal ACT adapter from RoboTwin.
Action layout: left arm 5, left gripper 1, right arm 5, right gripper 1.
The parent RoboTwin checkout supplies env_cfg/so101_pgripper.yml and dapier_act/.
See parent docs/dapier/REPRODUCE.md for installation, data, training and replay.

The model and deploy.py behavior is preserved, including RGB input handling and
oldest-heavy temporal ensemble. The original train.sh/process_data.sh are demo
stubs, **not working training/conversion entry points**. Use the parent module
`PYTHONPATH="$PWD:$PWD/dapier_act" python dapier_act/train_multimodal.py --help` instead.

```bash
# from RoboTwin root after environment setup; use a trusted local checkpoint dir
bash XPolicyLab/policy/DAPIER_ACT/eval.sh RoboTwin move_woodenblock_randomized \
  "$PWD/checkpoints/dapier_act_multimodal_dp3head256_grid30_far_v4" \
  so101_pgripper joint 0 0 0 "$VIRTUAL_ENV" "$VIRTUAL_ENV"
```

The checkpoint directory must contain policy_best.pt and dataset_stats.pkl.
No checkpoint is distributed; metadata/hashes are in the parent validation docs.
This exact command has not completed GPU closed-loop validation on the migration
host. The existing adapter uses explicit parent-relative checkpoint paths and a
parents[3] RoboTwin root; these historical conventions are preserved and do not
meet all current upstream adapter submission conventions. It is not a benchmark
submission. Inputs are decoded RGB arrays from the XPolicyLab server. Historical
v4 training used BGR-to-RGB swapping; no behavior change is made here.

Changes made for packaging: README clarified; shell helper robot registry adds
so101_pgripper (runtime parent registry separately defines dual_so101_pgripper).
Apache-2.0 notices in XPolicyLab/LICENSE remain in force.
