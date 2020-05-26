+++
title = "Efficient Semantic Segmentation using Gradual Grouping (Oral) (Best paper Runner up Award)"
date = "2018-07-01"

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.

authors = ["**Sriharsha Annamaneni**", "Nikitha Vallurapalli", "Girish Varma", "C V Jawahar", "Manu Mathew", "Soyeb Nagori"]

# Publication type.
# Legend:
# 0 = Uncategorized
# 1 = Conference proceedings
# 2 = Journal
# 3 = Work in progress
# 4 = Technical report
# 5 = Book
# 6 = Book chapter
publication_types = ["1"]

# Publication name and optional abbreviated version.
publication = "IEEE Access."
#publication_short = "ISBI"

# Abstract and optional shortened version.

#abstract = "We propose a generalized focal loss function based on the Tversky index to address the issue of data imbalance in medical image segmentation. Compared to the commonly used Dice loss, our loss function achieves a better trade off between precision and recall when training on small structures such as lesions. To evaluate our loss function, we improve the attention U-Net model by incorporating an image pyramid to preserve contextual features. We experiment on the BUS 2017 dataset and ISIC 2018 dataset where lesions occupy 4.84% and 21.4% of the images area and improve segmentation accuracy when compared to the standard U-Net by 25.7% and 3.6%, respectively."

abstract = "Deep CNNs for semantic segmentation have high memory and run time requirements. Various approaches have been proposed to make CNNs efficient like grouped, shuffled, depth-wise separable convolutions. We study the effectiveness of these techniques on a real-time semantic segmentation architecture like ERFNet for improving runtime by over 5X. We apply these techniques to CNN layers partially or fully and evaluate the testing accuracies on Cityscapes dataset. We obtain accuracy vs parameters/FLOPs trade offs, giving accuracy scores for models that can run under specified runtime budgets. We further propose a novel training procedure which starts out with a dense convolution but gradually evolves towards a grouped convolution. We show that our proposed training method and efficient architecture design can improve accuracies by over 8% with depthwise separable convolutions applied on the encoder of ERFNet and attaching a light weight decoder. This results in a model which has a 5X improvement in FLOPs while only suffering a 4% degradatioon in accuracy with respect to ERFNet."

# Featured image thumbnail (optional)
#image_preview = "static/img/ftl.png"

# Is this a featured publication? (true/false)
featured = true

# Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter the filename (excluding '.md') of your project file in `content/project/`.
#projects = ["example-external-project"]

# Links (optional).
url_pdf = "https://sriharshavenugopal.github.io/sriharshavenugopal.github.io/pdf/Paper1.pdf"
url_preprint = ""
url_code = ""
url_dataset = ""
url_project = ""
url_slides = "https://drive.google.com/file/d/16WnXJbOaqgd6MPw6cvo923DYyhdeN6j_/view"
url_video = ""
url_poster = "https://drive.google.com/file/d/1NnO7Zd_IF4mXSIhZZVZVDwslxWU79XSQ/view"
url_source = ""

# Custom links (optional).
#   Uncomment line below to enable. For multiple links, use the form `[{...}, {...}, {...}]`.
#[[url_custom]]
#name = "Journal"
#url = "https://link.springer.com/article/10.1007/s10584-014-1174-4"

# Does the content use math formatting?
math = true

# Does the content use source code highlighting?
highlight = true
  
# Featured image
# Place your image in the `static/img/` folder and reference its filename below, e.g. `image = "example.jpg"`.
[header]
image = "1.png"
#caption = "My caption :smile:"

+++
