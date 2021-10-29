﻿# Comparison of Attack Images (Table 2)

---
## Quick Start Based on Our Data:

1. Before the experiment, please be sure that the data is well prepared according to the tutorial in the main page of the repository.

2. We have put [commands](https://github.com/DNNTesting/CovTesting/blob/d462c59c1cbc00c2add20ee0eaf7a9966859788b/Table%202/test_example.sh#L32-L41) in 'test_example.sh' script for reference. Please run the following commands to get the comparison results of different models:

   ```python
   python criteria.py  -dataset mnist  -model lenet1
   python criteria.py  -dataset mnist  -model lenet4
   python criteria.py  -dataset mnist  -model lenet5
   
   python criteria.py  -dataset cifar  -model vgg16
   python criteria.py  -dataset cifar  -model resnet20
   
   python criteria.py  -dataset svhn  -model svhn_model
   python criteria.py  -dataset svhn  -model svhn_first
   python criteria.py  -dataset svhn  -model svhn_second
   ```
   
   The results will be stored in 'compare_result.txt'. 
   
   (We only keep the original Images dataset for CIFAR and SVHN models. We re-attack the MNIST models and use the newly generated dataset to do testing. So the results of MNIST is slightly different from those shown in our paper. But as you can see, the difference is acceptable and it will not affect our conclusion.)



## Experiment on Your Own Data:

1. Before the experiment, please be sure that the data is well prepared according to the tutorial in the main page of the repository.



2. Apply (optimization-based) adversarial attacks.

   Please run `bash attack.sh` to apply "bim" "cw" "deepfool" "fgsm" "newtonfool" "pgd" "spatialtransformation" "squareattack" to different models and datasets.

   Adversrial datasets will be stored at `../data/dataset/adv/model/attack`, e.g., `../data/mnist/adv/lenet/bim`

3. Use DeepHunter to generate test cases:

   Use `python deephunter.py`. It generates adversarial examples under `../data/mnist/adv/lenet/deephunter`.

   *Zhou: Here it only generates 1000 deephunter examples for each dataset. I don't know why.*

4. Compare DH and DP:

   Please run the following commands to compare DH and DP for different models. 

   ```python
   python criteria.py  -dataset mnist  -model lenet1
   python criteria.py  -dataset mnist  -model lenet4
   python criteria.py  -dataset mnist  -model lenet5
   
   python criteria.py  -dataset cifar  -model vgg16
   python criteria.py  -dataset cifar  -model resnet20
   
   python criteria.py  -dataset svhn  -model svhn_model
   python criteria.py  -dataset svhn  -model svhn_first
   python criteria.py  -dataset svhn  -model svhn_second
   ```

   The results will be stored in './table_2/dataset/model/compare_result.txt'. 

   

   

   