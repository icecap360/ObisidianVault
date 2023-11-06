- - 2 aspects to consider
- Data throughput/training time:
	- generally achieved by utilizing the GPU as much as possible and thus filling GPU memory to its limit.
	- If the desired batch size exceeds the limits of the GPU memory, the memory optimization techniques, such as gradient accumulation, can help.
	- However, if the preferred batch size fits into memory, there’s no reason to apply memory-optimizing techniques because they can slow down the training.


# References
[https://huggingface.co/docs/transformers/perf_train_gpu_one](https://huggingface.co/docs/transformers/perf_train_gpu_one)