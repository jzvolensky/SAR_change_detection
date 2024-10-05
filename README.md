## Change Detector

## Overview

Change Detector is a Python package designed to detect changes between two satellite images, specifically for Synthetic Aperture Radar (SAR) data. By leveraging SAR amplitude information and the Isolation Forest algorithm, this package allows users to identify areas of change effectively. It is particularly useful for those working in remote sensing, environmental monitoring, agriculture, or urban planning.

The package returns a detailed change map that highlights regions of appearance and disappearance, helping users gain insights into landscape dynamics.

## Features

- Detects changes between two SAR images using amplitude information.
- Identifies changes as either appearance or disappearance.
- Uses Isolation Forest to classify areas of significant change.
- Customizable parameters such as filter size and contamination level for fine-tuning.

Inputs are two satellite images (as np arrays) with the same size and from the same area. Then the code computes a ratio using amplitudes SAR information* and therefore Isolation Forest.

* The ratio of the arithmetic mean to the geometric mean of the spatially averaged intensities. The asymmetric term focuses more specifically on amplitude variations.
It helps to assess how the arithmetic mean of the intensities compares to their geometric mean. A value close to 1 would indicate an almost perfect equality between these means, suggesting uniformity in the amplitudes of the analyzed images.

## Références
This ratio computation is inspired by the work described in the following paper:

> J. Ni, C. López-Martínez, Z. Hu, et F. Zhang,  
> "**Multitemporal SAR and Polarimetric SAR Optimization and Classification: Reinterpreting Temporal Coherence**,"  
> *IEEE Transactions on Geoscience and Remote Sensing*, vol. 60, pp. 1-17, 2022, Art no. 5236617.  
> [doi: 10.1109/TGRS.2022.3214097](https://doi.org/10.1109/TGRS.2022.3214097)

## Installation

You can install the package with:

    pip install change_detector


## Requirements

The package depends on the following Python packages:

    numpy
    scikit-learn
    scipy

## Usage

To use the package, you need to import it and apply it to two satellite images represented as NumPy arrays. Here's an example:

    import tifffile
    from change_detector import change_detector

# Load the satellite images
    first_image = tifffile.imread('path/to/first_image.tif')
    second_image = tifffile.imread('path/to/second_image.tif')

# Run the change detection algorithm
    change_map = change_detector(first_image, second_image)

# Display the change map
    import matplotlib.pyplot as plt

    cmap = plt.cm.get_cmap('gray', 3)  # Set color map: black for -1, gray for 0, white for 1
    plt.imshow(change_map, cmap=cmap, vmin=-1, vmax=1)
    cbar = plt.colorbar(ticks=[-1, 0, 1])
    cbar.ax.set_yticklabels(['Disappearance (-1)', 'No Change (0)', 'Appearance (1)'])
    plt.title('Change Detection Map')
    plt.axis('off')
    plt.show()


## Parameters

    first_image, second_image: The two SAR satellite images to compare. These should be np.array objects, typically loaded using a library such as tifffile.

    filter_size: The size of the filter used in the asymmetric term computation (default is (3, 3)).

    contamination: The proportion of points to consider as anomalies for Isolation Forest. Default is 0.02.

## Results

The output is a change map where:

    -1 indicates areas where changes involve disappearance.
    0 indicates no change.
    1 indicates areas where changes involve appearance.

## Contributing

If you want to contribute to change_detector, feel free to fork the repository and submit a pull request. Suggestions, issues, and feature requests are also very welcome!

## License

This project is licensed under the **GNU Affero General Public License v3.0 (AGPL-3.0)**.

You are free to use, modify, and distribute this code, provided that any modifications and derived works are also licensed under AGPL-3.0. If you use this code in a publicly accessible service (such as a web service), you must make your modified source code available under the same terms.

For more details, see the `LICENSE` file or visit [GNU's official site](https://www.gnu.org/licenses/agpl-3.0.en.html).