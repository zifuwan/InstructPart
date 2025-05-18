<div align="center">
<h1> InstructPart: Task-Oriented Part Segmentation with Instruction Reasoning
</h1>

Zifu Wan, Yaqi Xie, Ce Zhang, Zhiqiu Lin, Zihan Wang, Simon Stepputtis, Deva Ramanan, Katia Sycara</sup>

 Robotics Institute, Carnegie Mellon University

[![arXiv](https://img.shields.io/badge/arXiv-2404.04256-b31b1b.svg)](https://arxiv.org/abs/2404.04256) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

</div>

## 👀 Introduction

This dataset contains annotated images and language instructions for task-oriented part segmentation. The dataset is divided into three main folders: `all`, `train`, and `test`.

## 📦 Download
- [Goodle Drive](https://drive.google.com/drive/folders/1b876tX1wdy-jbyLvGSmZkAS4U5DuXlST?usp=drive_link)

## ⏳ Folder Structure

```plaintext
instructpart_part/InstructPart
├── all
│   ├── data_all.json
│   ├── images
│   ├── masks
│   └── visualize_overlap
├── train
│   ├── train200
│   │   ├── images
│   │   ├── masks
│   │   └── data_train.json
│   ├── train600
│   │   ├── images
│   │   ├── masks
│   │   └── data_train.json
│   ├── train1200
│   │   ├── images
│   │   ├── masks
│   │   └── data_train.json
│   └── train1800
│       ├── images
│       ├── masks
│       └── data_train.json
├── test
│   ├── images
│   ├── masks
│   └── data_test.json
└── README.md
```

## 📊 Dataset Statistics
- Total samples: 2400
- Training samples: 1800
    - The training set is further divided into 200, 600, 1200, 1800 samples. Specifically, starting with 200 samples, we gradually increase the number of training samples to 600, 1,200, and finally 1,800. Each increment includes all the previously used training samples.
- Testing samples: 600

## 💡 Annotation Structure
```json
[
    {
        "image_path": "image_name.jpg",
        "part_list": [
            {
                "object": "object_name",
                "part": "part_name",
                "affordance": "affordance_name",
                "action": "action_name",
                "instruction": [
                    "human-written-instruction",
                    "GPT4-rewritten-instruction",
                    "part-of-object sentence",
                    "part-of-object-affordance sentence",
                    "part-of-object",
                    "part-of-object-affordance"
                ]
            }
        ]
    },
    ...
]
```

## 🌰 Example Usage
```python
def read_my_data(instruction_id):
    val_dataset = "InstructPart|test"
    base_image_dir = "xxx"
    splits = val_dataset.split("|")
    if len(splits) == 2:
            ds, split = splits
    if ds == "InstructPart":
        images = glob.glob(
            os.path.join(base_image_dir, "instructpart_part", ds, split, "images", "*.jpg")
        )
    masks_path = os.path.join(base_image_dir, "instructpart_part", ds, split, "masks")
    json_path_reason_part = os.path.join(base_image_dir, "instructpart_part", ds, split, "data_" + split +".json")
    reason_part_data = {}
    with open(json_path_reason_part, 'r') as file:
        data = json.load(file)
        for entry in data:
            part_list = entry["part_list"]
            image_path = entry["image_path"]
            for item in part_list:
                object_name = item['object'].replace(" ", "_")
                part_name = item['part'].replace(" ", "_")
                # Generate filenames
                base_filename = os.path.splitext(image_path)[0]  # Filename without extension
                object_part_filename = f"{base_filename}-{object_name}-{part_name}.jpg"
                
                instruction = item["instruction"][instruction_id]
                reason_part_data[object_part_filename] = instruction
    return reason_part_data, images

instruction_ids = [
                   0, # human-annotated task instruction
                   1, # gpt-rewritten task instruction
                   2, # object-part task instruction
                   3, # object-part-affordance task instruction
                   4, # object-part name
                   5, # object-part-affordance name
                   ]


# get image and instruction content
reason_part_data, images_paths = read_my_data(instruction_ids[i])

for idx in range(len(images_paths)):
    image_path = images_paths[idx]
    instruction = reason_part_data[os.path.basename(image_path)]
    image = Image.open(image_path).convert("RGB")
```

## 🙏 Acknowledgements

Our model is adapted from [LISA](https://github.com/dvlab-research/LISA). Besides, we appreciate the free-licensed images from [Flickr website](https://www.flickr.com/) and [AGD20K](https://github.com/lhc1224/Cross-View-AG).

## 📧 Contact

If you have any questions, please  contact at [zifuw@andrew.cmu.edu](mailto:zifuw@andrew.cmu.edu).

## 📌 BibTeX & Citation

If you find this code useful, please consider citing our work:

```bibtex
@inproceedings{
  wan2024instructpart,
  title={InstructPart: Task-Oriented Part Segmentation with Instruction Reasoning},
  author={Wan, Zifu and  Xie, Yaqi and Zhang, Ce and Lin, Zhiqiu and Wang, Zihan and Stepputtis, Simon and Ramanan, Deva and Sycara, Katia},
  booktitle={The 63rd Annual Meeting of the Association for Computational Linguistics},
  year={2025},
  url={https://openreview.net/forum?id=IMEr4XgJSZ}
}
```