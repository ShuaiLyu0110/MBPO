# MBPO: Multi-Branch Policy Optimization via Tree Search for Multimodal Large Language Models

<div align="center">

**Multi-Branch Policy Optimization via Tree Search for Multimodal Large Language Models**

[![Paper](https://img.shields.io/badge/paper-arXiv-red)](https://arxiv.org/abs/XXXX.XXXXX) [![Code](https://img.shields.io/badge/code-GitHub-blue)](https://github.com/ShuaiLyu0110/MBPO) [![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

</div>

## 🌟 Overview

**MBPO** is a tree-based reinforcement learning framework that addresses credit assignment challenges in **multimodal large language model (MLLM)** training. Unlike traditional group-based methods that apply uniform trajectory-level advantages, MBPO enables **branch-level credit assignment** by representing candidate solutions as multimodal reasoning trees, allowing for more precise and informative learning signals in vision-language reasoning tasks.

<div align="center">
  <img src="f1.png" alt="MBPO Framework" style="width: 45%; max-width: 1200px; max-height: 500px; height: 30%;">
</div>

## 🎯 Key Idea

Group-based reinforcement learning methods like GRPO have become popular for training **multimodal large language models**, but they typically rely on **trajectory-level credit assignment** that applies a single advantage to all tokens in a response. In multimodal reasoning tasks, where visual information leads to longer and more complex reasoning processes, this uniform signal becomes progressively less informative as correct reasoning signals can be overwhelmed by incorrect derivations, leading to:

- **Credit ambiguity**: The learning signal cannot clearly discourage mistakes while encouraging corrections in vision-language reasoning
- **Relative-advantage degeneration**: Advantages become increasingly concentrated near zero as rollouts become more similar

MBPO addresses these challenges in multimodal settings by:

1. **Branch-level credit assignment**: Assigns relative advantages to sibling branches at each decision node in multimodal reasoning trees, enabling more accurate global advantage assignment
2. **Temporal replay buffer**: Reuses informative segments while controlling policy staleness, improving sample efficiency for multimodal training

We propose **MMRL18K**, a comprehensive multimodal reasoning dataset containing approximately 18K multimodal reasoning problems, which is integrated from k12-freeform-filtered (obtained by filtering k12-freeform to retain high-quality multimodal reasoning problems) and ThinkLite-VL-Hard-11k. This diverse dataset enables robust evaluation of MBPO's branch-level credit assignment across various vision-language reasoning scenarios.

<div align="center">
  <img src="f2.png" alt="MBPO Advantage Comparison" style="width: 95%; max-width: 1200px; height: auto;">
</div>

## 🚀 Key Features

- **🌳 Multimodal reasoning trees**: Represents candidate solutions as trees where nodes correspond to intermediate vision-language states
- **🎯 Branch-level credit assignment**: Assigns relative advantages to sibling branches at decision nodes in multimodal reasoning
- **💾 Temporal replay buffer**: Reuses informative segments while controlling policy staleness
- **📈 Stable advantages**: Mitigates relative-advantage degeneration throughout multimodal training
- **🖼️ Vision-language integration**: Effectively handles complex multimodal reasoning tasks requiring both visual understanding and language generation

## 📊 Results

MBPO demonstrates significant improvements over baseline methods across multiple multimodal reasoning benchmarks. We evaluate on Geometry3K and MMK12 datasets, comparing against GRPO, DAPO, and GSPO methods.

<div align="center">
  <img src="f3.png" alt="MBPO Performance Analysis" style="width: 70%; max-width: 1200px;">
  <p><em>Figure: Performance analysis showing MBPO's improvements across different metrics and datasets compared to baseline methods.</em></p>
</div>

### Table 2: Performance on Geometry3K Dataset

The following table presents detailed performance metrics on the Geometry3K dataset, evaluating MBPO against GRPO, DAPO, and GSPO methods across Qwen-3B and Qwen-7B models. MBPO consistently achieves the best performance on Geometry3K and ChartQA metrics, demonstrating superior multimodal reasoning capabilities.

| Method | Model | Geometry3K | Math Avg. | HallBench | ChartQA |
|--------|-------|-------|-----------|-----------|---------|
| Baseline | Qwen-3B | 27.83 | 42.35 | 59.83 | 74.54 |
| + GRPO | Qwen-3B | 42.64 (+14.81) | 46.46 (+4.11) | 61.60 (+1.77) | 80.20 (+5.66) |
| + DAPO | Qwen-3B | 45.09 (+17.26) | 48.57 (+6.22) | 62.34 (+2.51) | 79.70 (+5.16) |
| + GSPO | Qwen-3B | 44.53 (+16.70) | 47.68 (+5.33) | 63.12 (+3.29) | 79.40 (+4.86) |
| **+ MBPO** | **Qwen-3B** | **49.91 (+22.08)** | **49.35 (+7.00)** | **63.60 (+3.77)** | **83.52 (+8.98)** |
| Baseline | Qwen-7B | 42.09 | 48.64 | 61.09 | 78.13 |
| + GRPO | Qwen-7B | 51.31 (+9.22) | 53.41 (+4.77) | 66.31 (+5.22) | 79.96 (+1.83) |
| + DAPO | Qwen-7B | 53.26 (+11.17) | 53.62 (+4.98) | 67.10 (+6.01) | 81.33 (+3.20) |
| + GSPO | Qwen-7B | 52.73 (+10.64) | **53.89 (+5.25)** | 67.33 (+6.24) | 79.71 (+1.58) |
| **+ MBPO** | **Qwen-7B** | **56.07 (+13.98)** | 53.79 (+5.15) | **68.52 (+7.43)** | **85.15 (+7.02)** |

### Table 3: Performance on MMK12 Dataset

The following table shows performance results on the MMK12 dataset. MBPO achieves state-of-the-art performance across most metrics, with particularly strong improvements on MMK12, Math Avg., and ChartQA tasks, highlighting the effectiveness of branch-level credit assignment in multimodal reasoning.

| Method | Model | MMK12 | Math Avg. | HallBench | ChartQA |
|--------|-------|-----|-----------|-----------|---------|
| Baseline | Qwen-3B | 43.10 | 42.35 | 59.83 | 74.54 |
| + GRPO | Qwen-3B | 55.82 (+12.72) | 48.04 (+5.69) | 63.84 (+4.01) | 78.31 (+3.77) |
| + DAPO | Qwen-3B | 58.83 (+15.73) | 48.24 (+5.89) | **64.88 (+5.05)** | 78.42 (+3.88) |
| + GSPO | Qwen-3B | 57.63 (+14.53) | 48.29 (+5.94) | 64.48 (+4.65) | 78.20 (+3.66) |
| **+ MBPO** | **Qwen-3B** | **60.14 (+17.04)** | **50.19 (+7.84)** | 64.77 (+4.94) | **83.44 (+8.90)** |
| Baseline | Qwen-7B | 52.06 | 48.64 | 61.09 | 78.13 |
| + GRPO | Qwen-7B | 65.18 (+13.12) | 53.53 (+4.89) | 65.80 (+4.71) | 82.08 (+3.95) |
| + DAPO | Qwen-7B | 67.65 (+15.59) | 54.52 (+5.88) | 68.71 (+7.62) | 81.76 (+3.63) |
| + GSPO | Qwen-7B | 67.23 (+15.17) | 54.24 (+5.60) | 69.02 (+7.93) | 81.16 (+3.03) |
| **+ MBPO** | **Qwen-7B** | **67.68 (+15.62)** | **55.51 (+6.87)** | **69.84 (+8.75)** | **86.36 (+8.23)** |

## 📖 Citation

If you find this work useful, please cite:

```bibtex
@article{lyu2025mbpo,
  title={MBPO: Multi-Branch Policy Optimization via Tree Search for Multimodal Large Language Models},
  author={Lyu, Shuai and Gong, Yuning and Luo, Haoran and others},
  journal={arXiv preprint},
  year={2025}
}
```

## 🔗 Resources

- **📄 Paper**: Coming soon~
- **💻 Code**: [GitHub](https://github.com/ShuaiLyu0110/MBPO)
- **📦 Dataset**: MMRL18K dataset will be released soon~

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🙏 Acknowledgments

We thank the verl framework and the open-source community for their valuable contributions.

---

<div align="center">
  <p>Made with ❤️ by the MBPO Team</p>
</div>

