# DOA Estimation Comparative Analysis: Sharp-Focus vs. Rectangular-Focus: Why is Triangular Distortion More Accurate Than Rectangular? [With MATLAB Code]

Abstract: This paper investigates the problem of direction-of-arrival (DOA) estimation based on coarsely quantized data, focusing on a two-step approach that involves first using covariance estimation to estimate the signal subspace and then employing the ESPRIT algorithm to extract DOA angles. The study specifically analyzes two types of stochastic quantization schemes with jitter: a 1-bit quantizer combined with rectangular jitter, and a multi-bit quantizer combined with triangular jitter. For each quantizer, we rigorously derive the high probability bound between the true and estimated signal subspace, as well as the true and estimated DOA angles. Through analysis, it is found that under certain scenarios, the subspace estimation based on triangular jitter qualitatively outperforms that based on rectangular jitter in terms of performance. Numerical simulations verify the optimality of our proposed estimation bounds in terms of dependence on the smallest non-zero eigenvalue of the target matrix. The obtained subspace estimation ensures its applicability to other spectral estimation algorithms and related problems.
Introduction: Direction-of-Arrival (DOA) estimation, which involves estimating the arrival angle of an incident signal, has a history of over a century of research [38]. However, it remains significant in modern applications such as wireless communications [42]. Generally speaking, this problem is a nonlinear inverse problem: given a set of p Fourier samples collected at n moments in time, detecting the angle between the receiver and s targets. MUSIC [43] and ESPRIT [41] are two fundamental and widely used algorithms for DOA estimation (and other spectral estimation problems), still considered optimal methods [34].
The first step of these algorithms is to approximate the potential signal subspace: given an unknown signal (which concentrates around the s-dimensional subspace, e.g., a complex p-dimensional Gaussian signal with a covariance matrix rank of s and a principal feature space U), from noisy observations  
where vectors represent random noise. In this paper, we assume the elements are independent identically distributed (i.i.d.) zero-mean sub-Gaussian random variables with variance (e.g., complex Gaussian distribution).
Define the observation matrix, a natural estimator for subspace U can be obtained by computing the left singular vectors of the truncation singular value decomposition (SVD), or alternatively, by computing the principal eigenvectors. This estimator is consistent because, in expectation, noise only affects the eigenvalues, not the space [2]. In the context of finite samples, the performance can be measured through appropriate subspace angle distances (see formula (2.1)). Combining existing results on covariance estimation with Davis-Kahan's theorem [13], we obtain an asymptotic error bound regarding oversampling ratio. Interestingly, if noise follows a specific distribution, [4] derived an improved error bound [3], which has been utilized [34] and established correlations between the errors between true and estimated angles obtained from ESPRIT or MUSIC algorithms.
Since DOA estimation applications often involve interfaces between simulation and digital domains, an additional challenge is considering the impact of quantization (i.e., observing values quantization) [23]. At this point, we only have quantized observations (, instead of simulated values. However, it is generally allowed to design quantizers Q while considering hardware limitations. This paper focuses on memoryless scalar quantization, which is a univariate function from the complex plane to a finite alphabet, acting element-wise on the vector in [23]. Especially in modern large-scale applications, coarse quantization schemes (with fewer bits per quantization level) are receiving increasing attention.
A series of studies [2, 53, 28, 36, 48, 54, 45, 35] have investigated DOA estimation based on the simplest coarse quantizer (or its dual-valued form with 2 bits) and such methods often rely on the law of sinusoids [46]. Although symbol quantizers are easy to implement, they lose all scaling information and can only achieve reliable recovery under strict assumptions about the signal and noise. Another category of more recent research [37, 51] has addressed this shortcoming by considering "jittered symbol quantizers" with deliberately added design noise: [37] uses Gaussian noise as jitter while [51] follows the approach of [17], using uniform jitter noise combined with each sample's 2-bit observation value. One core finding of this paper is that triangular jitter quantizers naturally apply to DOA estimation — they create favorable statistical characteristics for quantization noise [24] which are necessary conditions for achieving better subspace estimation bounds [4] and MUSIC/ESPRIT algorithm performance bounds [34]. Related research on quantizing DOA estimation and jitter quantizers will be detailed in Section 1.2.
Method Introduction Core Framework This paper adopts a two-step method of "Subspace Recognition + ESPRIT Spectral Estimation", specifically designed for coarsely quantized data, with the specific process as follows:
1  
Quantization Stage: Apply jitter to the noisy observation values and then quantize them, resulting in the quantized observation values.
2  
Subspace Estimation: Construct a covariance estimator based on the quantized observation values, and extract the signal subspace through the principal feature vectors.
3  
DOA Estimation: Estimated subspace is substituted into the ESPRIT algorithm to calculate DOA angles.
Key assumptions and foundational definitions 1. Core assumption (Assumption 2.1) •
Signal-to-noise independence: Independent of each other;
•  
Noise Characteristics: A K-th order Gaussian vector, with independent identically distributed elements, zero mean, and variance; the real and imaginary parts are independent in the complex domain.
Signal Covariance: If deterministic, define; if stochastic, independent and identically distributed.
2. Definition of subspace distance measures sinusoidal - theta distance to measure differences between subspaces: Assuming U and V are standard orthogonal base matrices for two same-dimensional subspaces, define "main angle sinusoidal matrix" (diagonal elements are the sine values of the principal angles of the two subspaces), then the subspace distance is  
which is the maximum principal angle sine value, which can be calculated using the formula  
(Operator norm).
Two types of jittering quantization schemes:
1. Rectangular jittering quantization (1) The quantization model uses a symbol quantizer combined with uniform jitter, where the quantized observation value is defined as:  
where:  
•  
(Real / Complex domain), is the real / complex symbol function (Real domain:; Complex domain:);  
•  
Jitter vector (uniform distribution, parameter of jitter range);  
•  
Quantization alphabet: Real domain is 1 bit (), Complex domain is 2 bits ().  
(2) Covariance estimator based on quantized observation values:  
where denotes the complex conjugate transpose, this estimator is an unbiased estimate (when covering the signal range).  
(3) Subspace estimation bound (Theorem 2.4) Under high probability (at least), the subspace distance satisfies:  
where:  
•  
is the main s-dimensional feature space, (Operator norm), (maximum element norm);  
•  
is the largest s-th characteristic value, is a constant depending on the parameters K of the subspace.
2. Triangular jittering quantization (1) The quantization model combines a uniform infinite range quantizer (resolution) with triangular jitter, where the quantized observation value is defined as:  
where:  
•  
Jitter follows a triangular distribution (convolution of two): ;  
•  
Quantizer: maps to (), which can be converted into b-bit finite quantization through selection.
(2) Quantifying Noise Characteristics (Lemma 2.8): Quantified noise possesses the following advantageous characteristics:
•  
Independently and identically distributed, zero mean;
•  
Covariance;
•  
CK - Second Gaussian (C is an absolute constant).
(3) Covariance Estimator Based on Quantized Observations:
Since the feature space remains unchanged, the main feature space of this estimator is consistent with that of the original.
(4) Subspace Estimation Bound (Theorem 2.10) Under high probability (at least), the subspace distance satisfies:
The main s-dimensional feature space, which is an absolute constant (dependent on K).
Application of ESPRIT Algorithm 1. DOA Estimation Model (Assumption 3.1) The core of DOA estimation is to recover the angle from the observation values, where:
•  
(Vandermonde matrix, columns correspond to s angles of Fourier basis);
•  
For signal amplitude (deterministic or stochastic), for noise (consistent with Assumption 2.1).
2. ESPRIT algorithm steps (Algorithm 2) 1
Subspace input: Estimates of subspaces under rectangular / triangular jittering are used as (standard orthogonal basis);
2  
Submatrix partitioning: Divides into forward and backward submatrices;
3  
Eigenvalue calculation: Calculate the s eigenvalues (representing pseudo-inverse)
4  
DOA output: (for multiple angular frequencies, take values).
3. The DOA estimation boundary is illustrated with triangular jittering (Theorem 3.4). Under high probability, the matching distance of DOA (,) for permutations satisfies:
The sth largest singular value, the covariance matrix of amplitude, and the absolute constant.
Core Conclusion 1
Quantization scheme performance differences: Triangular jitter significantly outperforms rectangular jitter in low noise (and high bitrate) scenarios, where subspace and DOA estimation performance is noticeably better than rectangular jitter - its zero-mean and controllable covariance characteristics make the error bounds more dependent on the smallest eigenvalue of the subspace covariance matrix (rectangular jitter is, triangular jitter is).
2
Theoretical optimality: Numerical simulations verify that the theoretical error bounds for both jitter schemes are optimal in terms of "dependence on the smallest non-zero eigenvalue of the target matrix" - the subspace distance decreases with decay for rectangular jitter, while it increases with decay for triangular jitter, consistent with theoretical deductions.
3
Generality of the method: The results presented in this paper not only apply to DOA estimation but can also be extended to tasks centered around "subspace identification," such as Factor Analysis, Matrix Denoising, Spectral Clustering; simultaneously, by combining [34] results, they can also provide a performance guarantee for coarse quantization scenarios under MUSIC and other spectral estimation algorithms.
4
Practical application value: Both jitter schemes are memoryless scalar quantizations that can be implemented in hardware (e.g., 1-bit ADC, thermal noise diodes [50, 40]), and the multi-bit design of triangular jitter can balance between "low hardware cost" and "high estimation accuracy," particularly suitable for large-scale DOA estimation (such as wireless communication, radar).
End

## Images

![(DOA Estimation)Comparative Highlighting: Comparative Analysis of Robust DOA Estimation vs. Triangular Distortion! Why is triangular distortion more precise than rectangular distortion? [Includes MATLAB Code]](01.jpg)
![(DOA Estimation)Comparative Highlighting: Comparative Analysis of Robust DOA Estimation vs. Triangular Distortion! Why is triangular distortion more precise than rectangular distortion? [Includes MATLAB Code]](02.jpg)
![(DOA Estimation)Comparative Highlighting: Comparative Analysis of Robust DOA Estimation vs. Triangular Distortion! Why is triangular distortion more precise than rectangular distortion? [Includes MATLAB Code]](03.jpg)
![(DOA Estimation)Comparative Highlighting: Comparative Analysis of Robust DOA Estimation vs. Triangular Distortion! Why is triangular distortion more precise than rectangular distortion? [Includes MATLAB Code]](04.jpg)
![(DOA Estimation)Comparative Highlighting: Comparative Analysis of Robust DOA Estimation vs. Triangular Distortion! Why is triangular distortion more precise than rectangular distortion? [Includes MATLAB Code]](05.jpg)
![(DOA Estimation)Comparative Highlighting: Comparative Analysis of Robust DOA Estimation vs. Triangular Distortion! Why is triangular distortion more precise than rectangular distortion? [Includes MATLAB Code]](06.jpg)
![(DOA Estimation)Comparative Highlighting: Comparative Analysis of Robust DOA Estimation vs. Triangular Distortion! Why is triangular distortion more precise than rectangular distortion? [Includes MATLAB Code]](07.jpg)

<!-- PAYMENT_BLOCK -->

## Get this dataset

- Payment link (Stripe): [Buy now](https://buy.stripe.com/3cs8yP7sY87d0vu9AB)
- Payment QR code: ![Payment QR code](pay.png)

<!-- /PAYMENT_BLOCK -->
