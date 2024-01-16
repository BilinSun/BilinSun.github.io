---
layout: post
title: "Saving non-tensor parameters in PyTorch"
date: 2024-01-14
tags: [PyTorch]
---

All tensor parameters created with `nn.Parameter` are automatically registered as *buffers* that will be included in the `state_dict` of the `nn.Module`, so saving the `state_dict` of a layer or the model makes sure all such parameters are saved. However, if we make use of "raw" NumPy tensors, then we need to manually `register_buffer`. If we use other types of parameters like strings, the situation will be more nuanced; we need to use *hooks*.

[TBA]