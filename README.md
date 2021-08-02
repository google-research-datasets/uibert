# UI understanding datasets for UIBert

This directory contains two datasets that are used in the downstream tasks for
evaluating
[UIBert: Learning Generic Multimodal Representations for UI Understanding](https://arxiv.org/abs/2107.13731):
app similar UI component retrieval data (AppSim) and referring expression
component retrieval data (RefExp).

Both datasets are extended from the public Rico dataset, which contains 72k
mobile app UI data. They add two different types of annotations to these UIs:

1.  In AppSim, each datapoint contains two UIs of similar category and the
    annotation of two semantically similar UI elements on them, such as a “Menu”
    buttons that appear on two UIs.
2.  In RefExp, each datapoint contains a UI and a referring expression of a UI
    element on it, such as “Red button on the top”.

We use unique image ids in both datasets to represent the UI and they can be
used to retrieve the original UI data in
[Rico](https://interactionmining.org/rico).

## Data format

Both data are saved as TFRecords, which makes it easy to be loaded in
Tensorflow.

1.  AppSim: Each TFRecord contains unique image ids of an anchor UI and a search
    UI. The anchor UI has one anchor UI element and the search UI has 10 UI
    elements, one of which is semantically similar to the anchor UI element.
    Detailed features are itemized as below.

    -   `anchor/image/anchor/bbox/{xmax|xmin|ymax|ymin}`: float_list[1].
        Bounding box of the anchor UIelement on the anchor UI.

    -   `{anchor|search}/image/id`: bytes_list[1]. Image id of the anchor and
        search UIs.

    -   `search/image/candidate/bbox/{xmax|xmin|ymax|ymin}`: float_list[10]. 10
        candidate UI elements on the search UI. One of them is semantically
        similar to the anchor UI element of the anchor UI. Its index is saved in
        `search/correct/index`.

    -   `search/correct/index`: int64_list[1]. Index of the semantically simialr
        UI element in the search UI candidates list.

2.  RefExp: Each TFRecord has the following features. Values under
    `image/view_hierarchy` are information extracted from UI’s view hierarchy
    file.

    -   `image/id`: bytes_list[1]. Image id.
    -   `image/ref_exp/label`: int64_list[1]. Index of the UI element that is
        referred to by the referring expression in the `image/object/bbox/…`
        list.
    -   `image/ref_exp/text`: bytes_list[1]. Referring expression.
    -   `image/object/bbox/{xmax|xmin|ymax|ymin}`: float_list of variable
        length. UI elements on the UI. The number of elements is denoted in
        `image/object/num`.
    -   `image/object/num`: float_list[1]. Number of UI elements.

    -   `image/view_hierarchy/bbox/{xmax|xmin|ymax|ymin}`: float list of
        variable length. Bounding boxes of leaf nodes in the view hierarchy.

    -   `image/view_hierarchy/class/label`: int64_list. Label of the class name.

    -   `image/view_hierarchy/class/name`: bytes_list. Processed class name
        string in the view hierarchy.

    -   `image/view_hierarchy/description` bytes_list. Content description
        string in the view hierarchy.

    -   `image/view_hierarchy/id/name`: bytes_list. Processed source id string
        in the view hierarchy.

    -   `image/view_hierarchy/text`: byte_list. Text string in the view
        hierarchy.

## Citation

If you use or discuss this dataset in your work, please cite our paper:

```json
@misc{bai2021uibert,
      title={UIBert: Learning Generic Multimodal Representations for UI Understanding},
      author={Chongyang Bai and Xiaoxue Zang and Ying Xu and Srinivas Sunkara and Abhinav Rastogi and Jindong Chen and Blaise Aguera y Arcas},
      year={2021},
      eprint={2107.13731},
      archivePrefix={arXiv},
      primaryClass={cs.CV}
}
```

## License

Datasets are licensed under
[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).

## Contact us

If you have a technical question regarding the dataset or publication, please
create an issue in this repository.
