Genetic Algorithm: A unique way for hyper-parameter tuning of ML models.

The process of evolution and natural selection (Survival of the fittest) used in this project 
to select the best hyper-parameters for certain regression techniques like 
Decision Tree Regression, Random Forest Regression, Light Gradient Boosting Regression 
and Extreme Gradient Boosting Regression.

In computer science and operations research, a genetic algorithm (GA) is a metaheuristic 
inspired by the process of natural selection that belongs to the larger class of evolutionary algorithms. 
Genetic algorithms are commonly used to generate high-quality solutions to optimization 
and search problems by relying on biologically inspired operators such as 
selection, crossover and mutation.


Install the library

    pip install darwin-mendel

Regression:
         
    Following is an example of regression run.


1: Example for Extreme Gradient Boost Regression

    from darwin_mendel.optimize_dtr import optimize_dtr
    from darwin_mendel.optimize_rfr import optimize_rfr
    from darwin_mendel.optimize_lgbmr import optimize_lgbmr
    from darwin_mendel.optimize_xgbr import optimize_xgbr
    import sklearn.datasets as datasets

    iris = datasets.load_boston()
    df = pd.DataFrame(iris.data)
    x_train, x_test, y_train, y_test = train_test_split(df[[2,4,5,6,7,8,9,10,11]], 
                                                        df[12], test_size=0.2, random_state=2021)
    params = {'n_estimators': [100,200,300,400,500,600,700,800,900,1000],
                 'max_depth': [8,9,10,11,12,13,14,15],
                 'learning_rate': [0.001, 0.01, 0.1, 0.2, 0.7, 0.8, 0.9, 1],
                 'booster': ['gbtree','gblinear'],
                 'reg_alpha': [0],
                 'reg_lambda': [1]}

    model, hyp_param = optimize_xgbr(x_train=x_train, y_train=y_train, y_test=y_test, x_test=x_test,
                                     params=params, number_of_generation=10, population_size=30, 
                                     error_metric='RMSE', mutation_rate=0.1)
    print(hyp_param)


2: OutPut: 

        n_estimators        400
        max_depth             9
        learning_rate       0.2
        booster          gbtree
        reg_alpha             0
        reg_lambda            1
        RMSE              16.76
        Name: 0, dtype: object


3. Arguments:


        a. User must provide the x_train, y_train, x_test and y_test in the arguments. They should not 
           contain any missing values and strings values.
        b. User can select the error_metreic between 'MAPE' and 'RMSE', it is used to select the best model.
           Default is 'MAPE' (Mean Absolute Percentage Error).
        c. population_size defines initial number of combination of hyper-parameters from which off-springs
           are produced. Thumb rule is, it should be: 5 * number of variables. Default is 50.
        d. number_of_generation is the number of new batches produced from the initial population.
           The more the number of generation, the better will be the resut but it could increase the 
           time consumption. Default is 10.
        e. mutation_rate is the percentage impurity added in a new batch of off springs, it helps in
           reaching the global minimum. Default is 0.05 i.e. 5%
        f. random_seed has to be fixed to make the results repeatable. Default is 2021.
        g. Default params: 
                {'n_estimators': [2,3,4,5,6,......1000],
                 'max_depth': [2,3,4,5,6....20],
                 'learning_rate': [0.001, 0.01, 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1],
                 'booster': ['gbtree'],
                 'reg_alpha': [0],
                 'reg_lambda': [1]} 
           Above mentioned are the default ranges for each hyperparameter of Random Forest Regression.
           User can give the range according to their need.

4. Default params ranges for other regression algorithms.
        

        a. RFR: 
                {'n_estimators': [2,3,4,.....1000],
                 'max_features': ['sqrt', 'auto', 'log2', None],
                 'min_samples_leaf': [2,3,4,5,6,.....16],
                 'max_depth': [2,3,4,5,6,.....20]}
        b. LGBMR: 
                {'n_estimators': [2,3,4,5,6,......1000],
                 'max_depth': [2,3,4,5,6....20],
                 'learning_rate': [0.001, 0.01, 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1],
                 'boosting_type': ['gbdt'],
                 'num_leaves': [2,3,4,5,6,....15],
                 'reg_alpha': [0],
                 'reg_lambda': [0]}  
        c. DTR: 
                {'min_samples_leaf': [1,2,3,4,5,6....20],
                 'max_depth': [2,3,4,5,6....20],
                 'max_features': ['auto', 'sqrt', 'log2'],
                 'splitter': ['best', 'random'],
                 'criterion': ['mse', 'friedman_mse', 'mae']}  

Classification:

    from darwin_mendel.optimize_dtc import optimize_dtc
    from darwin_mendel.optimize_rfc import optimize_rfc
    from darwin_mendel.optimize_lgbmc import optimize_lgbmc
    from darwin_mendel.optimize_xgbc import optimize_xgbc

    I:   Default error_metric is 'accuracy_score', other available options are 'f1-score' 
         and 'roc_auc_score'. Is is used to select the best models and score them accordingly.
         NOTE: Please don't use 'roc_auc_score' for multi-class models.
    II:  population_size, default value is 50.
    III: number_of_generation, default value is 10.
    IV:  mutation_rate, default value is 0.05.
    V:   random_seed, default value is 2021.
    Vi:  The range of default hyper-parameters are given below, the used can provide a different range
         for each of the parapeter according to their need.


1. Default params ranges for all Classification algorithms.


        a. RFC: 
                {'n_estimators': [2,3,4,.....1000],
                 'max_features': ['sqrt', 'auto', 'log2', None],
                 'min_samples_leaf': [2,3,4,5,6,.....16],
                 'max_depth': [2,3,4,5,6,.....20],
                 'criterion': ['gini', 'entropy'],
                 'oob_score': [True, False]}
        b. LGBMC: 
                {'n_estimators': [2,3,4,5,6,......1000],
                 'max_depth': [2,3,4,5,6....20],
                 'learning_rate': [0.001, 0.01, 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1],
                 'boosting_type': ['gbdt'],
                 'num_leaves': [2,3,4,5,6,....15],
                 'reg_alpha': [0],
                 'reg_lambda': [0]}  
        c. DTC: 
                {'min_samples_leaf': [1,2,3,4,5,6....20],
                 'max_depth': [2,3,4,5,6....20],
                 'max_features': ['auto', 'sqrt', 'log2'],
                 'splitter': ['best', 'random'],
                 'criterion': ['gini', 'entropy']}  
        d. XGBC: 
                {'n_estimators': [2,3,4,5,6,......1000],
                 'max_depth': [2,3,4,5,6....20],
                 'learning_rate': [0.001, 0.01, 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1],
                 'booster': ['gbtree'],
                 'reg_alpha': [0],
                 'reg_lambda': [1]} 