# Extended Berkeley Segmentation Benchmark

**A more comprehensive benchmark can now be found at [davidstutz/superpixel-benchmark](https://github.com/davidstutz/superpixel-benchmark).**

**Update: MatLab R2017a introduced a function `groundTruth` with clashes with some variable names and the field names of the ground truth `.mat` files, see [#1](https://github.com/davidstutz/extended-berkeley-segmentation-benchmark/issues/1).**

This is an extended version of the Berkeley Segmentation Benchmark, available [here](http://www.eecs.berkeley.edu/Research/Projects/CS/vision/grouping/resources.html) and introduced in [1], used to assess superpixel algorithms.

    [1] P. Arbeláez, M. Maire, C. Fowlkes, J. Malik.
        Contour detection and hierarchical image segmentation.
        Transactions on Pattern Analysis and Machine Intelligence, volume 33, number 5, pages 898–916, 2011.

The extended version was implemented in the course of the following work:

    [2] D. Stutz.
        Superpixel Segmentation using Depth Information.
        Bachelor thesis, RWTH Aachen University, Aachen, Germany, 2014.
	[7] D. Stutz.
		Superpixel Segmentation: An Evaluation.
		Pattern Recognition (J. Gall, P. Gehler, B. Leibe (Eds.)), Lecture Notes in Computer Science, vol. 9358, pages 555 - 562, 2015.

When using this benchmark, please cite [1] and [2]. Additional information can also be found on [http://davidstutz.de](http://davidstutz.de).

## Installation / Compiling

To compile the benchmark on 32-but/64-bit Linux follow the instructions found in `source/README`:

> To compile the benchmarking software from source code, run:
>
>   source build.sh
>
> This script should compile the correspondPixels mex file and copy it into the 
> ../benchmarks/ directory.

## Measures and Usage

The original benchmark already includes the following measures:

* Boundary Recall, Boundary Precision and F-measure;
* Probabilistic Rand Index, Segmentation Covering and Variation of Information.

Details on these measures may be found in [1] or [2]. As most of these measures are unsuited for assessing superpixel algorithms (except for Boundary Recall), the extended version of the Berkeley Segmentation Benchmark adds the following measures:

* Undersegmentation Error (UE, implemented as discussed in [3];
* Achievable Segmentation Accuracy (ASA) [4];
* Compactness (CO) [5];
* Sum-Of-Squared Error (SSE);
* Explained Variation (EV) (e.g. as discussed in [6]);

For details, see [3], [4], [5], [6] or [2]:

	[3] P. Neubert, P. Protzel.
        Superpixel benchmark and comparison.
        Forum Bildverarbeitung, 2012.

    [4] M. Y. Lui, O. Tuzel, S. Ramalingam, R. Chellappa.
        Entropy rate superpixel segmentation.
        Proceedings of the Conference on Computer Vision and Pattern Recognition, pages 2097–2104, 2011.

    [5] A. Schick, M. Fischer, R. Stiefelhagen.
        Measuring and evaluating the compactness of superpixels.
        Proceedings of the International Conference on Pattern Recognition, pages 930–934, 2012.

    [6] D. Tang, H. Fu, and X. Cao.
        Topology preserved regular superpixel.
        In Multimedia and Expo, International Conference on, pages 765–768, Melbourne, Australia, July 2012

For details on how to use the benchmark, please consult `test_benchmarks.m` - the script demonstrates the usage of all the above measures. For details on the required file format, test data is provided in the `/data` folder. For example, the `allBench` function will run _all_ measures on test data generated by some superpixel algorithms:

    imgDir = 'data/BSDS500/images';
    gtDir = 'data/BSDS500/groundTruth';
    inDir = 'data/BSDS500/superpixel_segs';
    outDir = 'tests/test_6';
    mkdir(outDir);
    nthresh = 5;

    tic;
    allBench(imgDir, gtDir, inDir, outDir, nthresh);
    toc;

**Note:** The Berkeley Segmentation Dataset provides several ground truth segmentations per image (e.g. at least 5 ground truth segmentations per image). Therefore, all measures can be computed using two different approaches:

1. Per image, the best value of the measure over all available ground truth segmentations is used and then averaged over all images.
2. The measure is averaged over all images and then the best value over all ground truth segmentations is determined.

Among others, the output folder will contain the following files:

* `eval_asa.txt`: Overall results for Achievable Segmentation Accuracy, in this order: index of ground truth segmentation selected for approach 2; Achievable Segmentation Accuracy for approach 2; Achievable Segmentation Accuracy for approach 1.
* `eval_asa_img.txt`: Achievable Segmentation Accuracy per image, in this order: index of image; index of ground truth segmentation with minimum Achievable Segmentation Accuracy; corresponding Achievable Segmentation Accuracy.

**Note:** For Achievable Segmentation Accuracy, with _best value_ the minimum value is meant. This results in a lower bound on the Achievable Segmentation Accuracy. By adapting `collect_eval_asa.m` this behavior can be changed.

* `eval_bdry.txt`: Overall Boundary Recall, Boundary Precision and F-measure, in this order: index of ground truth segmentation selected for approach 2; Boundary Recall for approach 2; Boundary Precision for approach 2; F-measure for approach 2; Boundary Recall for approach 1; Boundary Precision for approach 1; F-measure for approach 1.

**Note:** Both Boundary Precision and F-measure are not suited for evaluating superpixel algorithms, see [2].

* `eval_bdry_img.txt`: Boundary Recall, Boundary Precision and F-measure per image, in this order: index of image; index of ground truth segmentation with best F-measure; corresponding Boundary Recall, corresponding Boundary Precision; corresponding F-measure.
* `eval_compactness.txt`: Overall Compactness, in this order: best Compactness; average Compactness; worst Compactness.
* `eval_compactness_img.txt`: Compactness per image, in this order: index of image; Compactness.
* `eval_superpixels.txt`: In this order: Highest number of superpixels; average number of superpixels; lowest number of superpixels.
* `eval_superpixels_img.txt`: In this order: index of image; number of superpixels.
* `eval_undersegmentation.txt`: Overall Undersegmentation Error, in this order: index of ground truth segmentation selected for approach 2; Undersegmentation Error for approach 2; Undersegmentation Error for approach 1.
* `eval_undersegmentation_img.txt`: Undersegmentation Error per image, in this order: index of image; index of ground truth segmentation with best Undersegmentation Error; corresponding Undersegmentation Error.
* `eval_sse.txt`: Overall Sum-Of-Squared Error, in this order: average Sum-Of-Squared Error for x,y coordinates (may be used as compactness measure); average Sum-Of-Squared Error for r,g,b color.
* `eval_sse_img.txt`: Sum-Of-Squared Error, in this order: index of image; Sum-Of-Squared Error for x,y coordinates; Sum-Of-Squared Error for r,g,b color.
* `eval_ev.txt`: Overall Explained Variation, only contains the average Explained Variation.
* `eval_ev_img.txt.`: Explained Variation per image, in this order: index of image; Explained Variation.

## License

Licenses for source code corresponding to:

D. Stutz. **Superpixel Segmentation using Depth Information.** Bachelor Thesis, RWTH Aachen University, 2014.

D. Stutz. **Superpixel Segmentation: An Evaluation.** Pattern Recognition (J. Gall, P. Gehler, B. Leibe (Eds.)), Lecture Notes in Computer Science, vol. 9358, pages 555 - 562, 2015.

Note that the source code is based on the following projects for which separate licenses apply:

* [Berkeley Segmentation Benchmark](http://www.eecs.berkeley.edu/Research/Projects/CS/vision/grouping/resources.html)

Copyright (c) 2014-2018 David Stutz, RWTH Aachen University

**Please read carefully the following terms and conditions and any accompanying documentation before you download and/or use this software and associated documentation files (the "Software").**

The authors hereby grant you a non-exclusive, non-transferable, free of charge right to copy, modify, merge, publish, distribute, and sublicense the Software for the sole purpose of performing non-commercial scientific research, non-commercial education, or non-commercial artistic projects.

Any other use, in particular any use for commercial purposes, is prohibited. This includes, without limitation, incorporation in a commercial product, use in a commercial service, or production of other artefacts for commercial purposes.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

You understand and agree that the authors are under no obligation to provide either maintenance services, update services, notices of latent defects, or corrections of defects with regard to the Software. The authors nevertheless reserve the right to update, modify, or discontinue the Software at any time.

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software. You agree to cite the corresponding papers (see above) in documents and papers that report on research using the Software.
