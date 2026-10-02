---
layout: publication
permalink: /projects/signformer-gcn/
order: 6
title: "SignFormer-GCN: Continuous Sign Language Translation Using Spatio-Temporal Graph Convolutional Networks"
short_title: "SignFormer-GCN"
date: 2025-02-14
categories: research-completed
venue: "PLOS ONE 20(2): e0316298, 2025 · Accepted at the WiML Workshop, NeurIPS 2025"
venue_short: "PLOS ONE 2025"
badge_style: accent
topics: ["Sign Language", "Graph Networks", "Transformers"]
authors: 'Safaeid Hossain Arib, <span class="me">Rabeya Akter</span>, Sejuti Rahman, Shafin Rahman'
authors_full: 'Safaeid Hossain Arib<sup>1</sup>, <span class="me">Rabeya Akter</span><sup>1</sup>, Sejuti Rahman<sup>1</sup>, Shafin Rahman<sup>2</sup>'
affiliations: '<sup>1</sup>Dept. of Robotics and Mechatronics Engineering, University of Dhaka &nbsp; <sup>2</sup>Dept. of Electrical and Computer Engineering, North South University'
paper: https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0316298
code: https://github.com/rabeya-akter/SignLanguageTranslation
thumb: images/papers/signformer-gcn/thumb.webp
image: images/papers/signformer-gcn/thumb.webp
summary: "SignFormer-GCN jointly encodes RGB video with a transformer and skeletal keypoints with an STGCN-LSTM, capturing both broad context and fine-grained body motion. It delivers competitive gloss-free translation on German, American, and Bangla sign language benchmarks with only 9.43M parameters."
tldr: "Transformers over RGB capture context but miss the skeleton's graph structure. SignFormer-GCN adds a spatio-temporal graph stream over keypoints, so the model sees both what the scene looks like and how the body moves."
stats:
  - value: "19.75"
    label: "BLEU-4 on RWTH-PHOENIX-2014T, gloss-free"
  - value: "8.53"
    label: "BLEU-4 on How2Sign test, best among compared methods"
  - value: "9.43M"
    label: "parameters, vs. 115.41M for GFSLT-VLP"
  - value: "3"
    label: "sign languages: German, American and Bangla"
teaser: images/papers/signformer-gcn/method.webp
teaser_caption: "<b>SignFormer-GCN.</b> (A) I3D features and keypoint features are encoded by a transformer encoder (B) and an STGCN-LSTM encoder (C), fused, and decoded into a spoken-language sentence."
bibtex: |
  @article{arib2025signformer,
    title   = {{SignFormer-GCN}: Continuous Sign Language Translation Using
               Spatio-Temporal Graph Convolutional Networks},
    author  = {Arib, Safaeid Hossain and Akter, Rabeya and Rahman, Sejuti and Rahman, Shafin},
    journal = {PLOS ONE},
    volume  = {20},
    number  = {2},
    pages   = {e0316298},
    year    = {2025},
    doi     = {10.1371/journal.pone.0316298}
  }
---

<h2>Abstract</h2>
<p class="abstract">Sign language is a complex visual language system that uses hand gestures, facial expressions, and body movements to convey meaning. It is the primary means of communication for millions of deaf and hard-of-hearing individuals worldwide. Tracking physical actions, such as hand movements and arm orientation, alongside expressive actions, including facial expressions, mouth movements, eye movements, eyebrow gestures, head movements, and body postures, using only RGB features can be limiting due to discrepancies in backgrounds and signers across different datasets. Despite this limitation, most Sign Language Translation (SLT) research relies solely on RGB features. We used keypoint features, and RGB features to capture better the pose and configuration of body parts involved in sign language actions and complement the RGB features. Similarly, most works on SLT research have used transformers, which are good at capturing broader, high-level context and focusing on the most relevant video frames. Still, the inherent graph structure associated with sign language is neglected and fails to capture low-level details. To solve this, we used a joint encoding technique using a transformer and STGCN architecture to capture the context of sign language expressions and spatial and temporal dependencies on skeleton graphs. Our method, SignFormer-GCN, achieves competitive performance in RWTH-PHOENIX-2014T, How2Sign, and BornilDB v1.0 datasets experimentally, showcasing its effectiveness in enhancing translation accuracy through different sign languages.</p>

<h2>Context and skeleton</h2>
<p class="kicker">Motivation</p>
<p>Sign language translation maps a video of signing directly to a spoken-language sentence. It is hard for two reasons. Signers move at different speeds, so videos vary in length. And frames do not line up one-to-one with words, because sign and spoken languages order meaning differently.</p>
<p>Most translators use only RGB video and a transformer. Transformers are good at broad, high-level context, but RGB features are sensitive to backgrounds and signer appearance, and they ignore the topology of the human body. Signing is, at its core, the motion of joints. A spatio-temporal skeleton graph captures exactly that, so SignFormer-GCN reads both.</p>

<h2>Method</h2>
<p class="kicker">Two streams, one decoder</p>
<ol class="steps">
  <li><strong>Appearance stream</strong>An I3D network embeds 16-frame clips. The embeddings get positional encodings and pass through a transformer encoder that models context across the whole video.</li>
  <li><strong>Skeleton stream</strong>MediaPipe keypoints form a graph over joints and time. Stacked STGCN blocks model joint relationships and an LSTM models how they evolve.</li>
  <li><strong>Fusion and decoding</strong>The two encodings are fused by summation, and a transformer decoder generates the sentence directly, with no gloss supervision.</li>
</ol>
<p>Given a video \(V = \{f_1, \dots, f_t\}\), the model learns \(P(S \mid V)\) for a sentence \(S = \{w_1, \dots, w_n\}\). The appearance stream embeds each clip and adds temporal position.</p>
<div class="eq">\[ E_t = P\big(F_{\mathrm{temporal}}(F_{\mathrm{I3D}}(V_{t:t+15}))\big), \qquad \hat{E}_t = E_t + E_{\mathrm{pos}}(t) \]</div>
<p>The whole model has 9.43M parameters and trains in about 2.5 minutes per epoch on a single RTX 3090.</p>

<h2>Datasets</h2>
<div class="table-wrap">
<table>
  <thead><tr><th>Dataset</th><th>Language</th><th>Signers</th><th>Hours (train)</th><th>Vocabulary (train)</th><th>Domain</th></tr></thead>
  <tbody>
    <tr><td>RWTH-PHOENIX-2014T</td><td>German (DGS)</td><td>9</td><td>9.2</td><td>2K</td><td>Weather forecasts</td></tr>
    <tr><td>How2Sign</td><td>American (ASL)</td><td>11</td><td>69.6</td><td>15.6K</td><td>Instructional videos</td></tr>
    <tr><td>BornilDB v1.0</td><td>Bangla (BdSL)</td><td>3</td><td>49.86</td><td>14.1K</td><td>Not specified</td></tr>
  </tbody>
</table>
</div>

<h2>Results</h2>
<p class="kicker">Gloss-free translation · BLEU · higher is better</p>

<h3>RWTH-PHOENIX-2014T (German)</h3>
<div class="table-wrap">
<table>
  <thead><tr><th>Method</th><th>BLEU-1</th><th>BLEU-2</th><th>BLEU-3</th><th>BLEU-4</th></tr></thead>
  <tbody>
    <tr><td>Conv2d-RNN</td><td>27.10</td><td>15.61</td><td>10.82</td><td>8.35</td></tr>
    <tr><td>+ Luong attention</td><td>29.86</td><td>17.52</td><td>11.96</td><td>9.00</td></tr>
    <tr><td>+ Bahdanau attention</td><td>32.24</td><td>19.03</td><td>12.83</td><td>9.58</td></tr>
    <tr><td>Joint-SLT</td><td>30.88</td><td>18.57</td><td>13.12</td><td>10.19</td></tr>
    <tr><td>Tokenization-SLT</td><td>37.22</td><td>23.88</td><td>17.08</td><td>13.25</td></tr>
    <tr><td>TSPNet-Sequential</td><td>35.65</td><td>22.80</td><td>16.60</td><td>12.97</td></tr>
    <tr><td>TSPNet-Joint</td><td>36.10</td><td>23.12</td><td>16.88</td><td>13.41</td></tr>
    <tr><td>GASLT</td><td>39.07</td><td>26.74</td><td>21.86</td><td>15.74</td></tr>
    <tr><td>GFSLT-VLP (115.41M params)</td><td>43.71</td><td>33.18</td><td>26.11</td><td>21.44</td></tr>
    <tr class="ours"><td>SignFormer-GCN (9.43M params)</td><td>41.19</td><td>30.89</td><td>24.23</td><td>19.75</td></tr>
  </tbody>
</table>
</div>
<p class="table-caption">SignFormer-GCN is competitive with the much larger GFSLT-VLP at about one-twelfth of the parameters, and ahead of every other gloss-free method.</p>

<h3>How2Sign (American)</h3>
<div class="table-wrap">
<table>
  <thead><tr><th>Method</th><th>Val rBLEU</th><th>Val BLEU-1</th><th>Val BLEU-4</th><th>Test rBLEU</th><th>Test BLEU-1</th><th>Test BLEU-4</th></tr></thead>
  <tbody>
    <tr><td>slt_how2sign</td><td>2.79</td><td>35.20</td><td>8.89</td><td>2.21</td><td>34.01</td><td>8.03</td></tr>
    <tr><td>asl_video2text</td><td>3.29</td><td>35.25</td><td>9.39</td><td>2.56</td><td>33.20</td><td>7.95</td></tr>
    <tr class="ours"><td>SignFormer-GCN</td><td>3.97</td><td>37.37</td><td>9.90</td><td>2.96</td><td>34.91</td><td>8.53</td></tr>
  </tbody>
</table>
</div>

<h3>BornilDB v1.0 (Bangla)</h3>
<div class="table-wrap">
<table>
  <thead><tr><th>Split</th><th>BLEU-1</th><th>BLEU-2</th><th>BLEU-3</th><th>BLEU-4</th></tr></thead>
  <tbody>
    <tr><td>Validation</td><td>7.62</td><td>3.05</td><td>1.37</td><td>0.72</td></tr>
    <tr><td>Test</td><td>7.37</td><td>2.89</td><td>1.18</td><td>0.58</td></tr>
  </tbody>
</table>
</div>
<p class="table-caption">BornilDB v1.0 is a new, low-resource Bangla benchmark with only three signers, which makes it far harder than the other two datasets.</p>

<h3>Example translations</h3>
<div class="table-wrap">
<table>
  <thead><tr><th>Dataset</th><th style="text-align:left">Reference</th><th style="text-align:left">Prediction</th></tr></thead>
  <tbody>
    <tr><td>How2Sign</td><td style="text-align:left;white-space:normal">the other thing i did was i had a little bit of line out.</td><td style="text-align:left;white-space:normal">another thing i would do is i have a little bit of line out.</td></tr>
    <tr><td>How2Sign</td><td style="text-align:left;white-space:normal">just enough to build up some chest strength.</td><td style="text-align:left;white-space:normal">enough to build some chest strength.</td></tr>
    <tr><td>PHOENIX</td><td style="text-align:left;white-space:normal">guten abend liebe zuschauer <em>(good evening dear viewers)</em></td><td style="text-align:left;white-space:normal">guten abend liebe zuschauer <em>(good evening dear viewers)</em></td></tr>
    <tr><td>BornilDB</td><td style="text-align:left;white-space:normal"><em>(Is everything okay?)</em></td><td style="text-align:left;white-space:normal"><em>(Is everything fine?)</em></td></tr>
    <tr><td>BornilDB</td><td style="text-align:left;white-space:normal"><em>(Didn't he know?)</em></td><td style="text-align:left;white-space:normal"><em>(Does he not know?)</em></td></tr>
  </tbody>
</table>
</div>
<p class="table-caption">Bangla examples are shown with the paper's English translations.</p>

<h2>What matters</h2>
<p class="kicker">Ablations on RWTH-PHOENIX-2014T · test BLEU-4</p>
<div class="table-wrap">
<table>
  <thead><tr><th>Variant</th><th>BLEU-4</th></tr></thead>
  <tbody>
    <tr class="group"><td colspan="2">Skeleton stream</td></tr>
    <tr><td><td>Without STGCN-LSTM</td><td>18.80</td></tr>
    <tr class="ours"><td><td>With STGCN-LSTM</td><td>19.75</td></tr>
    <tr class="group"><td colspan="2">STGCN layers</td></tr>
    <tr><td><td>1 / 2</td><td>18.87 / 19.26</td></tr>
    <tr class="ours"><td><td>3</td><td>19.75</td></tr>
    <tr><td><td>4 / 5</td><td>19.06 / 19.16</td></tr>
    <tr class="group"><td colspan="2">Encoder–decoder layers</td></tr>
    <tr><td><td>2–2</td><td>18.27</td></tr>
    <tr><td><td>3–3</td><td>19.10</td></tr>
    <tr class="ours"><td><td>6–3</td><td>19.75</td></tr>
    <tr class="group"><td colspan="2">LSTM layers</td></tr>
    <tr class="ours"><td><td>1</td><td>19.75</td></tr>
    <tr><td><td>2 / 3</td><td>19.31 / 18.89</td></tr>
  </tbody>
</table>
</div>
<ul>
  <li><strong>The graph stream adds real signal.</strong> Removing the STGCN-LSTM encoder costs about a full BLEU-4 point.</li>
  <li><strong>Moderate depth works best.</strong> Three STGCN blocks and a single LSTM layer beat deeper variants.</li>
  <li><strong>Simple fusion wins.</strong> Summing the two streams outperformed fusing them with a linear layer or an LSTM in validation BLEU-4.</li>
</ul>

<h2>Future work</h2>
<div class="note">
  <p>The next step is to narrow the semantic gap between the three representations involved (video, keypoints and text) so the model can learn even richer translations from sign language video.</p>
</div>
