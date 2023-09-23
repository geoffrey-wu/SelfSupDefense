# Adversarial Attacks are Reversible via Natural Supervision

ICCV2021

## Citation

```
@InProceedings{Mao_2021_ICCV,
    author    = {Mao, Chengzhi and Chiquier, Mia and Wang, Hao and Yang, Junfeng and Vondrick, Carl},
    title     = {Adversarial Attacks Are Reversible With Natural Supervision},
    booktitle = {Proceedings of the IEEE/CVF International Conference on Computer Vision (ICCV)},
    month     = {October},
    year      = {2021},
    pages     = {661-671}
}
```

# setup

- Create the environment from the environment.yml file:
- `conda env create -f environment.yml`
- `conda activate myenv`

## CIFAR-10 Experiment

- Choose the right normalization function in `cifar10_defense.py` L23-26

- File `cifar10_defense.py` is for both training SSL branch and test reversal defense. If you would like
  to train SSL, do not use `--eval_only`, and vice versa.

### Example Command for running our method:

#### Semi-SL Carmon et. al.

- Do not do std, mean normalize, they just use 0-1.
- Download Carmon et. al.'s model: [RobustBackboneClassifier: cifar10_rst_adv.pt.ckpt](https://cv.cs.columbia.edu/mcz/ICCVRevAttack/cifar10_rst_adv.pt.ckpt), [Our SSL Model: ssl_model_130.pth](https://cv.cs.columbia.edu/mcz/ICCVRevAttack/ssl_model_130.pth)
- Train SSL: `CUDA_VISIBLE_DEVICES=0 python cifar10_defense.py --fname unlab_cifar10_srn28-10_carmon --md_path /local/rcs/mcz/2021Spring/RobPretrained/unlabeled-rob/cifar10_rst_adv.pt.ckpt --carmon`, if you use our checkponit, you can pass this step.
- Test: `CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7 python cifar10_defense.py --fname test --md_path /local/rcs/mcz/2021Spring/RobPretrained/unlabeled-rob/cifar10_rst_adv.pt.ckpt --carmon --eval_only --ssl_model_path /local/rcs/mcz/2021Spring/SSRobdata/unlab_cifar10_srn28-10_carmon/March1/ssl_model_130.pth`

- We offer PGD, CW, and BIM attack
- For AutoAttack, run the following: `CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7 python cifar10_defense_rebAA.py --fname test --md_path /proj/vondrick/mcz/SSRobust/Pretrained_model/unlabeled-rob/cifar10_rst_adv.pt.ckpt --carmon --eval_only --ssl_model_path /proj/vondrick/mcz/SSRobust/Ours/unlab_cifar10_srn28-10_carmon/March1/ssl_model_130.pth --attack-iters 1 --n_views 4`

### Geoffrey's Command

Download cifar data into a folder called `cifar-data`.
Download `cifar10_rst_adv.pt.ckpt` and `ssl_model_130.pth` into the same folder as `cifar10_defense.py`.

```bash
CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7 python cifar10_defense.py --fname test --md_path cifar10_rst_adv.pt.ckpt --carmon --eval_only --ssl_model_path ssl_model_130.pth --contrastive_bs 4 --save_root_path ./results --data-dir ./cifar-data/
```
