|name|params|test_f2_score|mean\_train_score|std\_train_score|mean\_test_score|std\_test_score|mean\_fit_time|std\_fit_time|
|--:|---|---|---|---|---|---|---|---|
|SVM_SMOTE + XGBoost|\{}|0\.5462184873949579|0\.7965208239559292|0\.02105298754703513|0\.4935077239704226|0\.05927822445826829|7\.658048033714294|3\.488105077611117|
|FA + SVM_SMOTE + XGBoost|\{'fa__n_components': 4}|0\.5409836065573771|0\.6185728774369207|0\.01602962271317152|0\.4896027678756865|0\.03897191665769936|15\.822591781616211|10\.125177376846517|
|ICA + SVM_SMOTE + XGBoost|\{'ica__algorithm': 'parallel', 'ica__fun': 'logcosh', 'ica__n_components': 36}|0\.5058365758754865|0\.7172742886257952|0\.022590017160928757|0\.456953812441132|0\.050102854488513184|4\.825080704689026|2\.2555864810562998|
|PCA + SVM_SMOTE + XGBoost|\{'pca__n_components': 77}|0\.4713114754098361|0\.7652492350548601|0\.024448659118861123|0\.4279594701313556|0\.044386012923953555|4\.078282809257507|0\.281078856368878|
|LLE + SVM_SMOTE + XGBoost|\{'lle__n_components': 9, 'n_neighbors'=20}|0\.23890784982935154|0\.37309501239867965|0\.012092585648853045|0\.17855865145328567|0\.029368536892508672|14\.824062418937682|5\.245562743028123|
|Isomap + SVM_SMOTE + XGBoost|\{'iso__n_components': 15, 'iso__n_neighbors':10}|0\.16363636363636364|0\.4314972530580296|0\.031651471557007496|0\.13007187487652272|0\.045345691162912344|28\.25498251914978|2\.24548614553849|
|KernelPCA + SVM_SMOTE + XGBoost|\{'kernel_pca__n_components': 80}||0\.525946111938067|0\.024207352892904724|0\.1730578980974989|0\.046186787162240985|18\.67526240348816|4\.173134147443913|


We use F2 score instead of F1 score because the F2 score is more suitable in our application where it’s more important to classify correctly as many positive samples as possible, rather than maximizing the number of correct classifications, In short, we want to lower the importance of precision and increase the importance of recall. 

For dimensional reduction task, we did some research and found out those models: FactorAnalysis (FA), Independent Component Analysis (ICA), Principal Component Analysis (PCA), Locally Linear Embedding (LLE), IsoMap Embedding and Kernal PCA. Applying those models, together with the base model (no features reduction), to our data set, we collect some indicators in the parameter-tunning duration, which is shown in the table above.

The three models LLE, IsoMap and KernelPCA perform really poor since their F2 scores are pretty low, about 0.24 and 0.16 for the first two, respectively, and especially with the KernalPCA, we did not see any promising result in training and validation, so we did not tune the parameters further to find the best possible result of this method. 

With the rest three models, statistically speaking, the PCA produces really good outputs for the training task (0.77 for mean score and really stable since the standard deviation is only 0.024). However, It works a little bit awfully in testing task as it catchs the lowest mean score among the three (absolutely overfitting). Compare to FA and ICA, they give much better generalization results, especially the FA with the F2 score is nearly approximate to that of the base model (about 0.541 and 0.546, respectively). It seems really promising if we spend more time tunning parameters for this model.

Although having the best performance among all the dimension-reducting methods, the FA consumes lots of time during the training and testing, which seems to be against the purpose of reducing the features of data (runs faster but still produces performance as good as the original data). Talking about this criterion, the ICA method seems to be in the spotlight, as a result of not only spending less time doing the tasks (twice as less as base model in both training and testing), but also gains reasonable scores.
