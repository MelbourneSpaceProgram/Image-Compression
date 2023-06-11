# Image-Compression
Some existing code and thought documentation for ACRUX-2 image compression module

In MSP, we are interested in **JPEG** application only

In this repo:
- K-Means: A way of classifying pixels into clusters by colour intensity, thus "blurring" the image but also saving information from each cluster, "out2.png" is a successfully compressed example, although can be compressed further

- DCT Compression: Using the Fast Fourier Transform algorithm in SciPy, directly applying transform to image pixels, "test_output.jpg" is an example, albeit is black and white

- ICER Compression: simplification of ICER, need to fix colour as "compressed.jpg" experiences an overall colour shift

- test_imageX.jpg files are several test cases with quality images in colours or BW

- A guetzli folder is included

Challenges:
- Making the algorithm **progressive**, minimising loss of information, and keeping size minimum for satellite transmission

- Existing libraries in C / C++ do not provide full solution, and are generally for different applications (web browser etc)

- Uncertainty about image sizes taken by camera, therefore need to work out how much loss is permitted


Possible future work:
- Implement existing algorithms but with a focus on splitting images into *layers* that have part of the "full picture" that is the full size

- Modify **guetzli** by Google but consider its non-progressive nature and the preference for higher quality images


General pipeline for **JPEG** image compression (similar between methods):
1. Colour space conversion: `RGB` to `YCbCr` for example to convert to luminance scale
2. Chromiance down-sampling: this is the stage that removes most data
3. Discrete Cosine Transform
4. Quantisation
5. Run length and huffman encoding (this outputs the final image and varies between compression algorithms)


ICER (Improved Codebook Enhanced Reversible) method pipeline:
- Based on **context modelling**: a bit is categorised based on the relationship between the pixel it belongs to and the neighbouring pixels

- Used by NASA Mars Rovers for progressive image compression, effective for lossless image transmission over unreliable channels

- Prediction: In the prediction step, ICER estimates the value of each pixel based on its neighboring pixels or previously encoded pixels. The difference between the predicted value and the actual value is calculated, which typically results in smaller numbers, as the prediction tends to be accurate for smooth or regular image regions.

- Quantization: The quantization step reduces the precision of the difference values obtained from prediction. By reducing the number of bits used to represent each difference value, the overall size of the encoded data is reduced.

- Codebook Construction: ICER builds a codebook that contains unique combinations of quantized difference values. The codebook is generated based on the patterns and frequencies of quantized differences found in the image.

- Entropy Coding: ICER performs entropy coding on the quantized difference values using techniques like Huffman coding or arithmetic coding. Entropy coding assigns shorter codes to more frequently occurring difference values and longer codes to less frequent ones. This further reduces the size of the encoded data.


