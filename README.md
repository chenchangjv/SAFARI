# Collecting Preference Rankings under Local Differential Privacy
 
We show the technical report and source code of **SAFARI**  in "[Collecting Preference Rankings under Local Differential Privacy](https://github.com/cheng-lab-at-bupt/SAFARI/blob/master/Collecting%20Preference%20Rankings%20under%20Local%20Differential%20Privacy%20(technical%20report).pdf)". 


# Experimental Requirement
- g++  5.1.0+

# Usage
Here we present an example of running experimets in the technical report.

## Ranking dataset

We provide the test ranking datasets in our experiments, which are extracted from the [Sushi dataset](http://www.kamishima.net/sushi/) and [Jester dataset](http://goldberg.berkeley.edu/jester-data/). The path of these ranking datasets is:
```bash
./SAFARI_code/dataset
```

## Build code
You can build the code in the following steps:
```bash
cd ./SAFARI_code/
mkdir obj
mkdir bin
make ./bin/naive 
make ./bin/safari 
make clean
```


## Run the naive model 
```bash
cd ./SAFARI_code/
./bin/naive --user_number 1000 --item_number 4 --epsilon 1.0 --input_file ./demo_dataset.txt --output_file out.txt --method rappor
```
> Explanation of the parameters :  
>> **user_number** : the total number of users in dataset file.  
>> **item_number** : the number of item in the item set.  
>> **epsilon** : the different privacy budget.  
>> **input_file** : the file name of dataset file.  
>> **output_file** : the file name of output file.  
>> **method** : the method of local differential privacy. You can chosse "rappor", "sh", "olh", "grr" or "noldp" (i.e. don't use local differential privacy).

## Run the SAFARI model
```bash
cd ./SAFARI_code/
./bin/safari --user_number 1000 --item_number 4 --k 1 --epsilon1 0.5 --epsilon2 0.5 --input_file ./demo_dataset.txt --output_file out.txt --method rappor --use_lasso yes
```
> Explanation of the parameters :  
>> **user_number** : the total number of users in dataset file.   
>> **item_number** : the number of item in the item set.  
>> **k** : construct a k-thin chain model in the **Rule I** (i.e. the cliques are are never allowed to have more than k variables).   
>> **epsilon1** : the different privacy budget in the **Rule I**.  
>> **epsilon2** : the different privacy budget in the **Rule II**.  
>> **input_file** : the file name of dataset file.  
>> **output_file** : the file name of output file.  
>> **method** : the method of local differential privacy. You can chosse "rappor", "sh", "grr" or "noldp" (i.e. don't use local differential privacy).  
>> **use_lasso** : "yes" or "no", which means whether you use lasso regression model (i.e. **Rule I** or **Rule I***).

# Documentation
- **./include/dataset.h ./bin/dataset.cpp** :  
    Synthesize ranking dataset, and examine the accuracy of the first-order marginals and the second-order marginals of the synthetic ranking dataset.
- **./include/lasso.h ./bin/lasso.cpp** :  
    The implementation of lasso regression model.
- **./include/ldp.h ./bin/grr.cpp ./bin/rappor.cpp ./bin/olh.cpp ./bin/sh.cpp** :  
    The implementation of four methods of local differential privacy. (rappor.cpp, sh.cpp, grr.cpp and olh.cpp correspond to Rappor, SH, GRR, OLH)
- **./include/util.h ./src/util.cpp** :  
    Some tools, sush as calculating mutual information and Cantor expansion.
- **./include/safari.h** :  
    The implementation of SAFARI. 
    - **./src/safair/phase_one.cpp** :  
        The implementation of the **Rule I*** (without lasso regression model). 
    - **./src/safair/phase_one_with_lasso.cpp** :  
        The implementation of the **Rule I** (with lasso regression model). 
    - **./src/safair/learning_hierarchi.cpp** :  
        The implementation of learning the k-thin chain model in **Rule I**.
    - **./src/safair/phase_two.cpp** :  
        The implementation of the **Rule II** and synthesize ranking dataset. 
- **./src/naive/naive_main.cpp** :  
    The main function of naive method.
- **./src/safari/safari_main.cpp** :  
    The main function of SAFARI.




