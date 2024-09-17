---
layout: page
title: Continuous Sign Language Translation
description: Natural Language Processing and Computer Vision
img: 
importance: 1
category: work
---


Developed SignFormer-GCN, a novel architecture combining transformer and STGCN to capture both high-level context and low-level spatial-temporal dependencies in sign language translation, integrating keypoint and RGB features to enhance recognition of body configurations and expressions, achieving competitive performance on RWTH-PHOENIX-2014T, How2Sign, and BornilDB v1.0 datasets.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/signformer.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
   (A)An overview of our two-stream fusion methodology, SignFormer-GCN, for sign language translation. The methodology fused rgb feature processed by transformer encoder and keypoint feature processed by STGCN-LSTM encoder and passed the fused output to a transformer decoder for final translation., (B) An overview of transformer encoder., (C) An overview of STGCN-LSTM encoder.
</div>


Project Demo : [Demo](https://drive.google.com/drive/folders/1ykrF3_4a9QtSUaAGZxTbI6ATz6IoftSe)

GitHub Repo : [Code](https://github.com/rabeya-akter/SignLanguageTranslation) 

