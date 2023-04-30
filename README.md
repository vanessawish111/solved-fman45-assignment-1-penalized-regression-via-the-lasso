Download Link: https://assignmentchef.com/product/solved-fman45-assignment-1-penalized-regression-via-the-lasso
<br>
<h1>1           Penalized regression via the LASSO</h1>

In linear regression, the purpose is to estimate the explanatory variable, or weights, <strong>w </strong>∈ R<em><sup>M </sup></em>given a linear relationship to the response variable (data) <strong>t </strong>∈ R<em><sup>N</sup></em>, defined by the regression matrix <strong>X </strong>∈ R<em><sup>N</sup></em><sup>×<em>M</em></sup>, i.e., the hypothesis space. In cases where for example the regression matrix is underdetermined and the model tends to overfit the data, or when a certain type of solution is required, a penalty term is added to the regression problem. In this assignment, you shall work with the so called Least Absolute Shrinkage and Selection Operator (LASSO), a type of penalized regression used when the sought explanatory variable is sparse, i.e., having few non-zero coordinates. The LASSO solves the minimization problem

minimize                   <strong>Xw</strong>                                                       (1)

<strong>w</strong>

where <em>λ </em>≥ 0 is a capacity hyperparameter. For the general regression matrix, there exists no closed-form solution for (1). Instead, consider a coordinate-wise approach, iteratively solving a sequence of optimization problems, wherein each only one coordinate in <strong>w </strong>is optimized at a time, keeping the others fixed at their most recent value. Thus, for the <em>i</em>:th coordinate <em>w<sub>i</sub></em>, the objective is to

minimize(2)

<em>w<sub>i</sub></em>

where <strong>x</strong><em><sub>i </sub></em>and <strong>r</strong><em><sub>i </sub></em>= <strong>t </strong>− <sup>P</sup><em><sub>`</sub></em><sub>6=<em>i </em></sub><strong>x</strong><em><sub>`</sub></em><em>w<sub>` </sub></em>denotes the <em>i</em>:th vector of the regression matrix and the residual vector without the effect of the <em>i</em>:th regressor, respectively. Then, at iteration <em>j</em>, the coordinate descent minimizer of <em>x<sub>i </sub></em>has the closed-form solution

(3)

where <strong>r</strong>                                                                   (4)

<em>`&lt;i                                   `&gt;i</em>

Task 1 (20 p): Verify the first line of (3) by solving (2) for <em>w<sub>i </sub></em>6= 0. You do not need to show the condition for which it holds, i.e., .

Hints: When solving for <em>w<sub>i </sub></em>after differentiating, you may want to get rid of <em>w<sub>i </sub></em>and solve for |<em>w<sub>i</sub></em>| first. Also, the absolute value |<em>w<sub>i</sub></em>| is not differentiable for <em>w<sub>i </sub></em>= 0. However, for <em>w<sub>i </sub></em>6= 0, its derivative is the sign of <em>w<sub>i</sub></em>, i.e.,

(5)

Now consider the simplified case where the regression matrix is an orthonormal basis, i.e., <em>M </em>= <em>N</em>, and <strong>X</strong><sup>&gt;</sup><strong>X </strong>= <strong>I</strong><em><sub>N</sub></em>, where <strong>I</strong><em><sub>N </sub></em>is the <em>N </em>× <em>N </em>identity matrix.

Task 2 (10 p): Given the orthogonal regression matrix, show that the coordinate descent solver in (3) will converge in at most 1 full pass over the coordinates in <strong>w</strong>, i.e., show that <em>w</em>ˆ<em><sub>i</sub></em>(2) − <em>w</em>ˆ<em><sub>i</sub></em><sup>(1) </sup>= 0<em>, </em>∀<em>i</em>.

Hint: Starting from (3), you may use (4) to show that <em>w</em>ˆ<em><sub>i</sub></em><sup>(<em>j</em>)</sup>, for an orthogonal regression matrix, does not depend on previous estimates <strong>w</strong>ˆ, but only on <strong>t</strong><em>,</em><strong>x</strong><em><sub>i</sub></em>, and <em>λ</em>.

When <strong>w </strong>is estimated from data using the LASSO, a bias will be induced into it, i.e,

<em>E </em>(<strong>w</strong>ˆ − <strong>w</strong><sup>∗</sup>) 6= <strong>0</strong>. Assume that the data <strong>t </strong>is truly a noisy (stochastic) example from the hypothesis space defined by the regression matrix, that is

<strong>t </strong>= <strong>Xw</strong><sup>∗ </sup>+ <strong>e</strong><em>, </em><strong>e </strong>∼ N (<strong>0</strong><em><sub>N</sub>,σ</em><strong>I</strong><em><sub>N</sub></em>)                                                               (6)

where <strong>w</strong><sup>∗ </sup>denotes the specific <strong>w </strong>defining the model used to generate the data, and where

N(·), <strong>0</strong><em><sub>N</sub></em>, and <em>σ </em>denote the Gaussian distribution, a length-<em>N </em>column vector of zeroes, and the noise standard deviation, respectively.

Task 3 (10 p): Show that the LASSO estimate’s bias, for an orthogonal regression matrix and data generated by (6), is given by (7) when <em>σ </em>→ 0, and discuss how this result relates to the method’s acronym, LASSO.

(7)

Hint: Starting from (3), you may consider the first line as two separate cases, <strong>x</strong>&gt;<em>i </em><strong>r</strong>(<em>ij</em>−1) <em>&gt; λ</em>, and <strong>x</strong>&gt;<em>i </em><strong>r</strong>(<em>ij</em>−1) <em>&lt; </em>−<em>λ</em>, respectively.

<h1>2           Hyperparameter-learning via K-fold cross-validation</h1>

In this section, you shall consider LASSO regression for the noisy data t in A1_data.mat, consisting of <em>N </em>= 50 data points generated by

(8)

<em>,                                                  </em>(9)

(10)

for <em>n </em>= 0<em>,…,N </em>− 1, i.e., the real part of a sum of two complex-valued sinusoids with frequencies 1<em>/</em>20 and 1<em>/</em>5 revolutions per sample, respectively, plus an additive Gaussian noise with standard deviation <em>σ</em>. The regression matrix you’ll be using is given by X in A1_data.mat, which consists of 500 candidate sine and cosine pairs, i.e.,

<strong>X</strong><strong> X</strong>                                                                                                     (11)

<strong>X</strong> (12) <strong>u</strong>             (13)

&gt;

<strong>v</strong>(14)

and where <em>f<sub>i </sub></em>are the 500 frequency candidates, equally spaced on the interval [0<em>.</em>02<em>,</em>0<em>.</em>48].

Task 4 (20 p): Implement a (cyclic) coordinate descent solver for the coordinate-wise LASSO solution in (3), using data t and regression matrix X in A1_data.mat. Do not use Xinterp for estimation, it is provided for vizualization only. Then solve the following two subtasks and discuss your results.

<ol>

 <li>Produce reconstruction plots with the following formatting: Plot the original <em>N </em>= 50 data points with disconnected (and clearly visible) markers (no connecting lines) vs. the time axis given by n. Overlay these with <em>N </em>= 50 reconstructed data points, i.e., <strong>y </strong>= <strong>Xw</strong>ˆ(<em>λ</em>), also using disconnected markers. In addition, add a solid line without markers for an interpolated reconstruction of the data, vs. an interpolated time axis. The interpolatation can be calculated using a highly sampled regression matrix, given as Xinterp. The interpolated time axis is found as ninterp. Specify legends for the all three lines, label the x-axis, and adjust line widths, font sizes and so on to make the plot clearly interpretable. Make three variants of this plot, one for each of the following values of the hyperparameter:

  <ul>

   <li><em>λ </em>= 0<em>.</em>1. • <em>λ </em>= 10.</li>

   <li>Try other values of <em>λ </em>to see the effect of the hyperparameter, and let <em>λ </em>= <em>λ</em><sub>user </sub>be the one among these you find most suitable, and produce a reconstruction plot for it.</li>

  </ul></li>

 <li>Count the number of non-zero coordinates for the values of the hyperparameter used above, i.e., <em>λ </em>∈ {0<em>.</em>1<em>,</em>10<em>,λ</em><sub>user</sub>}, and compare these to the actual number of nonzero coordinates needed to model the data given the true frequencies, which amounts to two pairs, or four coordinates.</li>

</ol>

Hint: To simplify the implementation, you may fill in and use the function sketch lasso_ccd().

When <em>λ </em>&amp; 0, optimization problem (1) reduces to an ordinary least squares problem, which if underdetermined, i.e., <em>M </em>≥ <em>N</em>, becomes ill-posed, and/or tends to overfit the data. The LASSO’s penalty term promotes sparsity in <strong>w </strong>by setting coordinates with small magnitude to zero, thereby reducing the degrees of freedom for the problem. The choice of the hyperparameter <em>λ </em>thus becomes critical; if set too low then too many coordinates become non-zero, although small, overfitting the data, and if set too high, then too many coordinates at set to zero, underfitting the data. Let <em>λ </em>be a grid of <em>J </em>potential values of the hyperparameter, i.e.,

(15)

where typically <em>λ</em><sub>1 </sub>is selected very small, and <em>λ<sub>J </sub></em>is selected as <em>λ</em><sub>max</sub>, i.e.,

(16)

To select <em>λ </em>optimally among these candidates, one may for instance use a K-fold crossvalidation scheme. In this approach, the training data is randomly split in K non-overlapping and equally sized folds (i.e., subsets), where (K-1) folds are used for estimation, and the remaining fold is used for validation. More specifically, let the set I<em><sub>k </sub></em>⊂ {1<em>,…,N</em>} denote the <em>N</em><sub>val </sub>= |I<em><sub>k</sub></em>| data indices selected for validation in the <em>k</em>:th fold. Thus, the complement of the validation index set, I<em><sub>k</sub><sup>c </sup></em>⊂ {1<em>,…,N</em>} denotes the set of <em>N</em><sub>est </sub>= |I<em><sub>k</sub><sup>c</sup></em>| = <em>N </em>− <em>N</em><sub>val </sub>data indices used for estimation, such that, by definition, I<em><sub>k </sub></em>∩ I<em><sub>k</sub><sup>c </sup></em>= 0. This partitioning is repeated K times for each potential value of <em>λ</em>, cycling through the non-overlapping validation folds and using it for validation, while using the rest for estimation. To measure the performance of the estimated model when used to predict on the validation data, the root mean squared error on validation (<em>RMSE</em><sub>est</sub>) term

(17)

is used, where <strong>w</strong>ˆ <sup>[<em>k</em>]</sup>(<em>λ<sub>j</sub></em>) denote the weight estimate (herein the LASSO estimate) for the <em>k</em>:th fold using hyperparameter <em>λ<sub>j</sub></em>. Note that <strong>X</strong>(I<em><sub>k</sub></em>) refers to the (concatenation of) rows of <strong>X </strong>indexed by I<em><sub>k</sub></em>. The hyperparameter is then selected as

<em>λ</em>ˆ = <em>λ<sub>p</sub>,            p </em>= argmin <em>RMSE</em><sub>val</sub>(<em>λ<sub>j</sub></em>)                                                      (18)

<em>j</em>

The following algorithm outlines a suggested implementation of a K-fold cross-validation scheme for the LASSO estimator:

Algorithm 1 K-fold cross-validation for LASSO Select grid of hyperparameters <em>λ<sub>j </sub></em>≥ 0<em>, </em>∀<em>j</em>, and <em>K &gt; </em>1.

Intitialize <em>SE</em><sub>val</sub>(<em>k,λ<sub>j</sub></em>) = 0<em>, </em>∀<em>k,j</em>.

Randomize validation indices I<em><sub>k</sub>, </em>∀<em>k</em>. for k = 1, …, K do for j = 1, …, J do

Calculate LASSO estimate <strong>w</strong>ˆ(<em>λ<sub>j</sub></em>)<sup>[<em>k</em>] </sup>on the <em>k</em>:th fold’s estimation data.

Set.

end for

end for

Calculate.

Select <em>λ</em><sup>ˆ </sup>as the <em>λ<sub>j </sub></em>which minimizes <em>RMSE</em><sub>val</sub>.

To display the generalization gap, the corresponding root mean squared error on estimation data (<em>RMSE</em><sub>est</sub>) becomes

(19)

Task 5 (20 p): Implement a K-fold cross-validation scheme for the LASSO solver implemented in Task 4. Illustrate your results using the following plots (and make detailed comments for them):

<ul>

 <li>A plot illustrating <em>RMSE</em><sub>val</sub>(<em>λ<sub>j</sub></em>) <em>λ<sub>j</sub>,</em>∀<em>j</em>, using a solid line with markers. Overlaid in the same plot, also plot the corresponding <em>RMSE</em><sub>est</sub>(<em>λ<sub>j</sub></em>) using a solid line with different markers. Also add a dashed vertical line at the location of the selected <em>λ</em><sup>ˆ</sup>. Specify legends for the all three lines, label the x-axis, and adjust line widths, font sizes and so on to make the plot clearly interpretable.</li>

 <li>A reconstruction plot for the selected <em>λ</em><sup>ˆ</sup>. Use the same layout and formatting as described for the reconstruction plots in Task 4.</li>

</ul>

Hint: For simpler implementation, you may fill in and use the function sketch lasso_cv(). Also, for improved vizualization and efficiency, select your grid of candidate <em>λ </em>to be evenly spaced on a log-scale, e.g., lambda_grid = exp( linspace( log(lambda_min), log(lambda_max), N_lambda)).

<h1>3           Denoising of an audio excerpt</h1>

In general, the sounds of importance that we perceive in our daily lives are narrowband, meaning that for any short (<em>&lt; </em>100 ms) sequence, the spectral representation is sparse, i.e., it may be modeled using only a small number of pure frequencies. This is typically true for, e.g., speech and music. As a contrast, sounds of small importance, or even nuisance sounds, are often broadband, such as, e.g., noise and interference, containing many or most frequencies across the spectrum.

In this section, you shall attempt to perform denoising of a noisy recording of piano music using the LASSO and a regression matrix of sine and cosine pairs. The idea is thus that, using a hypothesis space with frequency functions, and by virtue of the sparse estimates obtained using the LASSO, only the strong narrowband components in the music will be modeled, while the broadband noise is cancelled.

The data archive A1_data.mat contains an excerpt of piano music, 5 seconds in total, divided into two data sets, Ttrain, and Ttest, where the former is for estimation and validation, while the latter is used only for testing (not validation). You can listen to the datasets using the Matlab-function soundsc(Ttrain,fs), where fs is the sampling rate, also provided in the data archive. The training data is much too large (and non-stationary) to be modeled in one regression problem. Instead, the training data is to be divided into frames 40 ms long. For each frame, a (possibly unique) LASSO solution may be obtained, which if used to reconstruct the data can be listened to. The same regression matrix, Xaudio is used for all frames. Do not use regression matrix X from the previous section.

Task 6 (10 p): Implement a K-fold cross-validation scheme for the multi-frame audio excerpt Ttrain, similar to Task 5, but modified in order to find one optimal <em>λ</em><sup>ˆ </sup>for all frames. Illustrate the results with a plot of <em>RMSE</em><sub>val</sub>, <em>RMSE</em><sub>est</sub>, and <em>λ</em><sup>ˆ</sup>, using the exact same formatting as in the corresponding plot in Task 5, and discuss your findings.

Hint: The provided function sketch multiframe_lasso_cv() outlines the algorithm, which may be filled in and used. For the data provided, complete training may take 15 minutes or longer, depending on the implementation.

Task 7 (10 p): Using the <em>λ</em><sup>ˆ </sup>you’ve trained in Task 6, use the provided function lasso_denoise() to denoise the test data Ttest. Save your results as

save(’denoised_audio’,’Ytest’,’fs’) and include in your submission. In addition, listen to your output discuss your results. Try whether another choice of <em>λ </em>achieves better noise-cancelling results and detail your findings in the report.

<h1>A            Provided data and functions</h1>

Contents of data archive A1_data.mat used in sections 2 and 3:

<table width="443">

 <tbody>

  <tr>

   <td width="69">t</td>

   <td width="375">Single realization of data <strong>t </strong>∈ R<sup>50 </sup>in (8)</td>

  </tr>

  <tr>

   <td width="69">X</td>

   <td width="375">Regression matrix for Task 4 and 5, <strong>X </strong>∈ R<sup>50×1000 </sup>in (11)</td>

  </tr>

  <tr>

   <td width="69">Xinterp</td>

   <td width="375">Like X but with interpolated time axis <strong>X </strong>∈ R<sup>1000×1000</sup></td>

  </tr>

  <tr>

   <td width="69">n</td>

   <td width="375">Time axis <strong>n </strong>∈ Z<sup>50 </sup>: <em>n </em>∈ [0<em>,N </em>− 1]</td>

  </tr>

  <tr>

   <td width="69">ninterp</td>

   <td width="375">Interpolated time axis <strong>n </strong>∈ R<sup>1000 </sup>: <em>n </em>∈ [0<em>,N </em>− 1]</td>

  </tr>

  <tr>

   <td width="69">Ttrain</td>

   <td width="375">Mono audio data for estimation and validation, <strong>T </strong>∈ R<sup>19404</sup></td>

  </tr>

  <tr>

   <td width="69">Ttest</td>

   <td width="375">Mono audio data for testing, <strong>T </strong>∈ R<sup>24697</sup></td>

  </tr>

  <tr>

   <td width="69">Xaudio</td>

   <td width="375">Regression matrix for Tasks 6 and 7, <strong>X </strong>∈ R<sup>364×2000 </sup>in (11)</td>

  </tr>

  <tr>

   <td width="69">fs</td>

   <td width="375">Sampling frequency, <em>f<sub>s </sub></em>= 8820 Hz.</td>

  </tr>

 </tbody>

</table>

Function sketches (partly contains pseudocode) provided:

<ul>

 <li>what = lasso_ccd(t,X,lambda):</li>

</ul>

Returns LASSO estimate what (i.e., <strong>w</strong>ˆ(<em>λ</em>)) given an input of data t, regression matrix X, and hyperparameter lambda.

<ul>

 <li>[wopt,lambdaopt,RMSEval,RMSEest] = lasso_cv(t,X,lambdavec,K):</li>

</ul>

Returns wopt; the LASSO estimate for the optimal <em>λ</em><sup>ˆ</sup>, as well as lambdahat; the optimal lambda, and RMSEval, RMSEest the <em>RMSE </em>on estimation and validation, respectively. Takes t, regression matrix X, grid of candidate hyperparameters lambdavec, and the number of folds K, as inputs.

<ul>

 <li>[Wopt, lambdahat, RMSEval, RMSEest] = multiframe_lasso_cv(T,X,lambdavec,K)</li>

</ul>

Returns Wopt; an M by N_frames matrix of LASSO estimates for each size-N frame of data in T, calculated for the optimal <em>λ</em><sup>ˆ</sup>, as well as lambdahat; the optimal lambda, and

RMSEval, RMSEest the <em>RMSE </em>on estimation and validation, respectively. Takes multiframe data T, regression matrix X, grid of candidate hyperparameters lambdavec, and the number of folds K, as inputs.

<ul>

 <li>Yclean = lasso_denoise(T,X,lambdaopt):</li>

</ul>

Returns the denoised audio vector Yclean given an input of noisy audio T, regression matrix X, and hyperparameter lambdaopt.

<h1>B              Illustration of data in Task 4 and 5</h1>

Figur 1: Plot of the provided noisy target values (data) t, together with the noise-free data generating function <em>f</em>(<em>n</em>) in (9).