Download Link: https://assignmentchef.com/product/solved-cv1-assignment-5
<br>
<strong>Task 1: </strong>Clustering for image retrieval (pen &amp; paper)

In 2017, Flickr launched a feature called “Similarity Search” which allows users to find similar images given a query image. A demo and technical description can be found at: <a href="https://code.flickr.net/2017/03/07/introducing-similarity-search-at-flickr/">http://code.flickr.net/2017/03/07/introducing-similarity-search-at-flickr/</a><a href="https://code.flickr.net/2017/03/07/introducing-similarity-search-at-flickr/">.</a>

Based on this article, briefly answer the following questions about how clustering algorithms for large-scale nearest neighbor image search can be implemented in practice.

<ol>

 <li>What is the “feature vector” for clustering in this application, and how is it obtained?</li>

 <li>Consider the direct <em>k</em>-nearest neighbours method to return <em>k </em>images most similar to a query image: extract the feature vector of the query image, and compare it to the feature vectors in the database to find the <em>k </em>feature vectors corresponding to the most similar images. Why is a direct implementation of this method not computationally feasible at a large scale (in the order of billions, 10<sup>9</sup>, of images to search among)? Mention at least two reasons.</li>

 <li>The idea of product quantization is to split every large feature vector <em>~x </em>into subvectors</li>

</ol>

<em>~x<sub>i</sub></em>, <em>i </em>= 1<em>,…,n</em>, so that  and then use <em>k</em>-means clustering on the subvectors separately. Then given a query vector <em>~q</em>, its nearest cluster center can be found by clustering each subvector <em>~q<sub>i </sub></em>independently. Why does this help speed up nearest neighbour search?

<strong>Task 2: </strong>Undersegmentation error (pen &amp; paper)

We have developed a segmentation algorithm that has partitioned an image into segments <em>S<sub>i</sub></em>, <em>i </em>= 1<em>,…,n </em>as described on the lecture slides. For the same image, we have a <em>ground truth segmentation G<sub>j</sub></em>, <em>j </em>= 1<em>,…,m</em>, which specifies how the image ideally should be partitioned.

Intuitively, the <em>undersegmentation error </em>for an individual ground truth segment <em>G<sub>j </sub></em>measures the amount of “bleeding” that a segmentation exhibits beyond <em>G<sub>j</sub></em>. The undersegmentation error for a ground truth segment <em>G<sub>j </sub></em>is defined as

#

Area(<em>S<sub>i</sub></em>) − Area(<em>G<sub>j</sub></em>)

UE(<em>G<sub>j</sub></em>) =                                       (1)

Area(<em>G<sub>j</sub></em>)

where the summation is over all segments <em>S<sub>i </sub></em>which have any overlap (non-empty intersection) with <em>G<sub>j</sub></em>, and the function Area returns the area, i.e., the number of pixels, of the corresponding segment.

Our image has 12 pixels in it, and our segmentation algorithm produces the segments <em>S<sub>i </sub></em>shown in Figure 1. Calculate the undersegmentation errors UE(<em>G</em><sub>1</sub>) and UE(<em>G</em><sub>2</sub>) of the ground truth segments <em>G</em><sub>1 </sub>and <em>G</em><sub>2 </sub>shown in Figure 2.

<table width="75">

 <tbody>

  <tr>

   <td width="19">1</td>

   <td width="19">4</td>

   <td width="19">4</td>

   <td width="19">4</td>

  </tr>

  <tr>

   <td width="19">1</td>

   <td width="19">2</td>

   <td width="19">4</td>

   <td width="19">4</td>

  </tr>

  <tr>

   <td width="19">1</td>

   <td width="19">2</td>

   <td width="19">3</td>

   <td width="19">3</td>

  </tr>

 </tbody>

</table>

Figure 1: A segmentation <em>S<sub>i</sub></em>, <em>i </em>= 1<em>,</em>2<em>,</em>3<em>,</em>4, with <em>i </em>indicated on each pixel.

Figure 2: Two ground truth segments <em>G</em><sub>1 </sub>(left) and <em>G</em><sub>2 </sub>(right) highlighted in gray.

<strong>Task 3: </strong>Undersegmentation error (programming)

Figure 3: Input image (left), and a ground truth segmentation result (right), where the colors indicate the segments. Segments correspond to semantic classes such as “wall”. Source: NYUv2 dataset <a href="https://cs.nyu.edu/~silberman/datasets/nyu_depth_v2.html">https://cs.nyu.edu/</a><a href="https://cs.nyu.edu/~silberman/datasets/nyu_depth_v2.html">~</a><a href="https://cs.nyu.edu/~silberman/datasets/nyu_depth_v2.html">silberman/datasets/nyu_depth_v2.html</a><a href="https://cs.nyu.edu/~silberman/datasets/nyu_depth_v2.html">.</a>

Download the input image 0001 rgb.png and the ground truth segmentation 0001 label.png from Moodle. These images are shown in Figure 3.

In the label image <em>L</em>, the pixel intensity value indicates which ground truth segment a pixel belongs to. For example, if <em>L</em>(<em>x,y</em>) has a value of <em>i</em>, the pixel (<em>x,y</em>) belongs to the <em>i</em>th segment.

Use the implementation of the SLIC algorithm from skimage.segmentation.slic to segment the color image. Select reasonable values for the parameters <em>n </em>(number of segments) and <em>c </em>(compactness). Visualize the segmentation and revise the parameters if necessary. Leave the other parameters at their default values. <strong>Note: </strong>Because SLIC is not a <em>semantic segmentation </em>method, you should not expect its output to match that shown in Figure 3.

Implement calculation of the undersegmentation error as defined in Eq. (1) for each ground truth segment in <em>L</em>. Compute and print out the undersegmentation error for every ground truth segment, and the average undersegmentation error over all ground truth segments.

How does the average undersegmentation error change when you increase the desired number of superpixels <em>n</em>? Why? Answer in 1-2 sentences as a comment in your code.

Hints:

Use comparisons such as L==i to obtain binary masks areas of the label image <em>L </em>that belong to segment <em>i</em>.

Use logical indexing with binary masks to extract corresponding areas of the segmentation result.

Use np.unique and np.count nonzero to find overlapping superpixels and to calculate their areas, respectively. You may also use np.unique to discover all labels that are present in <em>L</em>.

Note that the label image has value 0 for pixels that are unlabeled (e.g., parts of boundaries). You can either ignore this or skip the label 0, as the undersegmentation error for this segment is not very meaningful.