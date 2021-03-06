## Algorithm Description
This [code](https://github.com/buzkent86/EnKCF_Tracking_WACV18/tree/master/C%2B%2B_Implementation) includes the C++ implementation of the Ensemble of Kernelized Correlation Filter Tracker (**EnKCF**). The EnKCF runs multiple KCFs `[1]` to tackle different aspects of tracking such as : **scale**, and **fast motion**.
It also employs a `Particle Filter` to smoothen the interaction among different KCFs. Our tracker achieves higher success and precision rates than the baseline tracker at `416hz` on UAV123 dataset. Below, you can find the hyper-parameters and their optimal values. The EnKCF is inspired by the long-term correlation (LCT) tracker `[2]`,
however, our goal is to utilize multiple KCFs in an efficient way to keep the complexity at each frame similar to the baseline KCF `(O(nlogn))` `[1]`. This way, it maintains (`30fps`) operation rate on **computationally limited embedded systems**.

You can find more details on the EnKCF tracker in our [arxiv version of the paper](https://arxiv.org/pdf/1801.06729.pdf).

### EnKCF Hyperparameters

* sigma_scale = 0.9   // Gaussian Kernel Bandwith in Scale KCF
* sigma_large_roi_translation = 0.7 // Gaussian Kernel Bandwith in Large ROI Trans. KCF
* sigma_small_roi_translation = 0.6 // Gaussian Kernel Bandwith in Small ROI Trans. KCF
* lambda = 0.0001 // Regularization Weight - Same for all the KCFs
* scale_filter_frequency = 5 // Scale Filter Applied every 5 frames
* learning_rate_scale = 0.10 // Make it 0.25 for the UAV123_10fps dataset
* learning_rate_large_roi_translation = 0.20
* learning_rate_small_roi_translation = 0.20
* scale_filter_training_psr_threshold = 4.0 // Threshold to Train Scale Filter
* padding_scale_filter = 0.0                // Area to Consider for Scale Filter
* padding_large_roi_translation = 2.0       // Area to Consider for Large Area Trans. Filter
* padding_small_roi_translation = 1.5       // Area to COnsider for Small Area Trans. Filter
* responsevariance_scale = 0.04	            // Variance for Desired Gaussian Response for Scale KCF
* responsevariance_large_roi_translation = 0.06 // Variance of the Gaussian Response for Large ROI Translation KCF
* responsevariance_small_roi_translation = 0.125 // Variance of the Gaussian Response for Small ROI Translation KCF


### Particle Filter Hyperparameters

It should be highlighted that we recommend that the particle filter to be removed in the existence of global camera motion.

* number_particles = 300		// Number of Particles in the Particle Filter
* number_efficient_particles = 1000/3.0 // Number of Efficient Particles to Enable Resampling
* process_noise_uniform_x = [-10,10]	// Transition Noise X Coordinate
* process_noise_uniform_y = [-10,10] // Transition Noise Y Coordinate
* process_noise_uniform_vx = [-2,2]  // Transition Noise X Velocity
* process_noise_uniform_vy = [-2,2]  // Transition Noise Y Velocity
* transition_noise_uniform_x = [-25,25] // Distribution Interval X Coordinate
* transition_noise_uniform_y = [-25,25] // Y Coordinate
* transition_noise_uniform_vx = [-10,10] // X Velocity
* transition_noise_uniform_vy = [-10,10] // Y Velocity
* beta_weight_function = 0.05 // exp(-dist * beta) - Weight Function Hyperparameter - For Importance Sampling based on spatial Eclidean Distance to Maximum of response map

### Run the Tracker

More information on running the tracker can be found [here](https://github.com/buzkent86/EnKCF_Tracking_WACV18/tree/master/C%2B%2B_Implementation).

For your questions or comments, please contact Burak Uzkent at `uzkent.burak@gmail.com`.

`MATLAB code` for the EnKCF tracker can be found [here](https://github.com/buzkent86/EnKCF_Matlab). A README file for the MATLAB code will be added soon.

You can cite the `EnKCF` tracker as

```
B. Uzkent and Y. Seo, "EnKCF: Ensemble of Kernelized Correlation Filters for High-Speed Object Tracking," 2018 IEEE Winter Conference on Applications of Computer Vision (WACV), Lake Tahoe, NV, 2018, pp. 1133-1141.
doi: 10.1109/WACV.2018.00129
```

#### References
[1] - Henriques, João F., Rui Caseiro, Pedro Martins, and Jorge Batista. "High-speed tracking with kernelized correlation filters." IEEE Transactions on Pattern Analysis and Machine Intelligence 37, no. 3 (2015): 583-596.

[2] - Ma, Chao, Xiaokang Yang, Chongyang Zhang, and Ming-Hsuan Yang. "Long-term correlation tracking." In Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition, pp. 5388-5396. 2015.
