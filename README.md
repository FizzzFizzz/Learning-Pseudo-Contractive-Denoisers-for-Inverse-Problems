# Learning-Pseudo-Contractive-Denoisers-for-Inverse-Problems

This is the code for the paper `Learning Pseudo-Contractive Denoisers for Inverse Problems', accepted by Internation Conference on Machine Learning (ICML 2024). For the papar, see [ICML](https://openreview.net/forum?id=G0vZ5ENrJQ&noteId=G0vZ5ENrJQ), or [Arxiv](https://arxiv.org/abs/2402.05637). Please note that there are some differences between the ICML version and the Arxiv version.

20240620: 
If you want to test it on your own, it would be beneficial if you are familiar with DPIR (https://github.com/cszn/DPIR), LMMO (https://github.com/basp-group/PnP-MMO-imaging), and Prox-PnP (https://github.com/samuro95/Prox-PnP). The code is based on these pioneer projects.

How to test?

Step 1: Create env according to DPIR and Prox-PnP.

Step 2: Download this code, along with the pretrained models at [pretrained models](https://drive.google.com/drive/folders/1-5Fl1sb7iCDYBOJB2KTI58Tgk3SoqJYI?usp=drive_link).

Step 3: Run any code starting with 'PnP_main_xxx.py'. You might need to create some folders in './log'.
For example, try 'python PnP_main_SPC+HQS.py' to remove Poisson noises with PnPI-HQS.

> Please note that, in Line 368 of 'PnP_main_SPC+HQS.py', plot_psnr(level,lambda,peak) is to process single image, and the results will be stored in the folder './images'. 'level' is the denoising level of the denoiser, 'lambda' is the balancing parameter, 'peak' is the peak value of the Poisson ditribution. In other experiments, plot(level,lambda,sigma), sigma is the standard derivation of Gaussian distribution. In Line 180, you can change different test images.
In Line 372, search_args() is to process a test set. In Line 243, you can change the sigma/peak, in Line 257, you can change the test set. You can fine tune the parameters in Line 267, 'search_range[0.12] = (260, 261, 10) # 28.1014, 0.8212'. 0.12 is the denoising level, and 260 is the balancing parameter.
You can write your own code, or modify the PnP algorithm, in about Line 122-167.


How to train?

Step 1: Train a normal DRUNet without any spectral regularization term until convergence. 

Step 2: Train the denoiser with spectral regularization terms to make it pseudo-contractive. You can achieve this by editting a 'xxx.json' file. The parameters need to be fine tuned very carefully. You may mainly check 'train_SPC_drunet_color.json' and 'train_SPC_drunet_gray.json'. The training results will be saved in the folder './denoising'.

> Please note that training a pseudo-contractive with the modified power iterative method is difficult and unstable (we are still working on this......). As a result, we recommend training a $k$-strictly pseudo-contractive denoiser with $k=0.5-0.9$, or $0.99$ (to approximate $k=1$). Note that when the fidelity term is sufficiently cocoercive, the value of $k$ can be enlarged, close to $1$. A larger $k$ typically means a better denoising performance. In my experiments, when $k>=0.8$, the denoising performace is almost not compromised. You can modify the spectral regularization term in the file 'loss_jacobian.py' in Line 155-206, or just define a new regularizer.
We have also found that ensuring strictly pseudo-contractiveness on the residual part seems less harmless, and more stable than ensuring strictly pseudo-contractiveness on the denoiser directly. The provided pretrained models are trained to have a pseudo-contractive residual. Note that, 'the residual part is pseudo-contracitve', is equivalent to say 'the denoiser is monotone'. We are still researching why the monotone contraints on the denoisers are less harmless. 


I'm not a very good coder, and the code is still under improvement. Please feel free to contact me if you find it confusing, boring, terrible, or interesting and helpful!

20240621: 
We are still organizing the codes. The codes will be under improvement, at least before September, 2024.
