# Coverage and Robustness VS. Training Datasets (Figure 2 and figure 3)

## Quick Start Based on Our Data:

1. Before the experiment, please be sure that the data is well prepared according to the tutorial in the main page of the repository. The fuzzing data we use to generate figure 2 and figure 3 are stored in 'fuzzing' folder. The retrained model we use to generate figure 3 are stored in 'new_model' folder. 

2. Generate the data used to get figure 2:

   ```$ python compare_coverage.py```  

   You can change the value of [T](https://github.com/DNNTesting/CovTesting/blob/3c73af15df594657dbc67034496b46736c7fcf13/Coverage%20and%20Robustness%20VS.%20Training%20Datasets/compare_coverage.py#L253) (select from 1-10) to get the coverage values for T1-T10. All of the results are stored in 'coverage_result.txt'. We have put all results in 'Figure 2 and figure 3.xlsx' and use them to draw figure 2.

3. Generate the data used to get figure 3:

   ```$ python robustness.py``` 

   You can change the value of [T](https://github.com/DNNTesting/CovTesting/blob/3c73af15df594657dbc67034496b46736c7fcf13/Coverage%20and%20Robustness%20VS.%20Training%20Datasets/robustness.py#L381) (select from 1-10) to get the coverage values for T1-T10. All of the results are stored in 'attack_evaluate_result.txt'. We also put all results from this step in 'Figure 2 and figure 3.xlsx' and use them to draw figure 3.

   

## Experiment on Your Own Data:

1. Before the experiment, please be sure that the data is well prepared according to the tutorial in the main page of the repository.

2. Generate new training data to be added to the original training dataset:

   Using `lenet1`, `mnist` as an example, to randomly generate adversarial examples and then select those that can improve the coerage, you can use the following commands:

   ```
   CUDA_VISIBLE_DEVICES=0 python fuzzing.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 0 &
   CUDA_VISIBLE_DEVICES=0 python fuzzing.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 1 &
   CUDA_VISIBLE_DEVICES=0 python fuzzing.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 2 &
   CUDA_VISIBLE_DEVICES=0 python fuzzing.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 3 &
   CUDA_VISIBLE_DEVICES=0 python fuzzing.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 4 &
   CUDA_VISIBLE_DEVICES=0 python fuzzing.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 5 &
   CUDA_VISIBLE_DEVICES=0 python fuzzing.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 6 &
   CUDA_VISIBLE_DEVICES=0 python fuzzing.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 7 &
   CUDA_VISIBLE_DEVICES=0 python fuzzing.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 8 &
   CUDA_VISIBLE_DEVICES=0 python fuzzing.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 9 &
   CUDA_VISIBLE_DEVICES=0 python fuzzing.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 10 &
   ```
   
   All results will be stored at  `fuzzing/nc_index_{}.npy.format(order_number)`.

3. Test the coverage for T1-T10:

   ```
   CUDA_VISIBLE_DEVICES=0 python compare_coverage.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 1 &
   CUDA_VISIBLE_DEVICES=0 python compare_coverage.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 2 &
   CUDA_VISIBLE_DEVICES=0 python compare_coverage.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 3 &
   CUDA_VISIBLE_DEVICES=0 python compare_coverage.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 4 &
   CUDA_VISIBLE_DEVICES=0 python compare_coverage.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 5 &
   CUDA_VISIBLE_DEVICES=0 python compare_coverage.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 6 &
   CUDA_VISIBLE_DEVICES=0 python compare_coverage.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 7 &
   CUDA_VISIBLE_DEVICES=0 python compare_coverage.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 8 &
   CUDA_VISIBLE_DEVICES=0 python compare_coverage.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 9 &
   CUDA_VISIBLE_DEVICES=0 python compare_coverage.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 10 &
   ```

   Get the coverage values for T1-T10, respectively (under `coverage_coverage_results`). All of the results are stored in 'coverage_result.txt' and can be used to draw figure 2. (refer to 'Figure 2 and figure 3.xlsx')

4. Generate the testing dataset:

   ```$ python fuzzing_testset.py``` 

   ```$ python exchange_testing_dataset.py``` 

   > When running 'fuzzing_testset.py', please modify the [order_number](https://github.com/DNNTesting/CovTesting/blob/fd2a5c649fb73b24826c80ee060e5a0250527e61/Coverage%20and%20Robustness%20VS.%20Training%20Datasets/fuzzing_testset.py#L336) from 0 to 1 in sequence. The results will be stored as 'fuzzing/nc_index_test_0.npy' and 'fuzzing/nc_index_test_1.npy'. After running 'exchange_testing_dataset.py', the new testing dataset will be generated and stored as 'x_test_new.npy' at the main folder. 

   *Zhou: I have no idea why they are doing this....*

   ```
   CUDA_VISIBLE_DEVICES=0 python fuzzing_testset.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 0 &
   CUDA_VISIBLE_DEVICES=0 python fuzzing_testset.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 1 &
   ```

   Then, we run ```python exchange_testing_dataset.py```.

5. Use the new training datasets T1-T10 to retrain the model:

   ```
   CUDA_VISIBLE_DEVICES=0 python retrain_robustness.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 1 &
   CUDA_VISIBLE_DEVICES=0 python retrain_robustness.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 2 &
   CUDA_VISIBLE_DEVICES=0 python retrain_robustness.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 3 &
   CUDA_VISIBLE_DEVICES=0 python retrain_robustness.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 4 &
   CUDA_VISIBLE_DEVICES=0 python retrain_robustness.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 5 &
   CUDA_VISIBLE_DEVICES=2 python retrain_robustness.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 6 &
   CUDA_VISIBLE_DEVICES=2 python retrain_robustness.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 7 &
   CUDA_VISIBLE_DEVICES=2 python retrain_robustness.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 8 &
   CUDA_VISIBLE_DEVICES=2 python retrain_robustness.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 9 &
   CUDA_VISIBLE_DEVICES=2 python retrain_robustness.py --model_name lenet1 --dataset mnist --model_layer 8 --order_number 10 &
   ```

   The retrained models are saved at ('new_model/' + dataset +'/model_{}.h5'.format(0-9)). After every  retraining, this file will also measure the robustness of the corresponding retrained model and results are stored in 'attack_results/attack_evaluate_result.txt', which can be used to generate figure 3. (refer to 'Figure 2 and figure 3.xlsx')





