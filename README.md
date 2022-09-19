# Pickled ML File Types
This is simply a list of files that are used in ML and contain pickles. This is intended to be used when creating detections of files that could be used to take over any system openning thanks to [fickling](https://github.com/trailofbits/fickling) and [pickle_wrapper](https://github.com/MythicAgents/pickle_wrapper). For the most part these extensions are purely conventions, and people can use whatever they feel like, so anytime you see the library used.

TODO: add samples of these file's to test injection and detection techniques. Feel free to provide merge requests with some, however please try to keep them smaller than normal, because I'll have to manually review the opperations of the pickle to ensure it's not truely malicious.

## File Types
### PyTorch (.pt, .pth, .ckpt)
PyTorch models and state dicts are commonly saved using torch.save with the extension .pt and .pth. And pytorch checkpoints are commonly saved with the extension .ckpt.

Pytorch files created after version 1.6 are a zipfile-based format. These still contain a pickle, however it's not just a strait pickle. torch.load still parses strait pickle files and `_use_new_zipfile_serialization=False` can be used to create a file in the old format that is just ap pickle.

### Numpy (npy, npz)
`numpy.save` and `numpy.savez` both fallback to using pickles if you save something that they can't encode otherwise. 

In my limitted tests, `save` appends some numpy meta data to the beginning of the file so that the linux `file` command will say it's a NumPy array, however if you partition the file contents on `\n` everything after the first new line is a standard pickle. The `save` command does support `allow_pickle=False` to ensure this doesn't happen, and `numpy.load` by default has `allow_pickle=False`.

`savez` simply creates a zip folder as the name implies that allows multiple npy files to be combined into one. While this is incredibly useful, there is no option for `allow_pickle` on savez, so it's extra important to make sure to not change `numpy.load`s default to `allow_pickle=False`.



### Sklearn (joblib)
Not entirely sure what `joblib.dump` does differently from pickle, but the end result is a pickle.

### Pickle (pkl, pickle, bin, data, dat)
A pickle's a pickle

Some libraries tend to have content saved using pickle.save directly, bellow is a list of ML libraries which this applies to according to stack overflow results. 
- NLTK

### Riva and NeMo (riva, nemo)
Archive that contains other formats and configs. The contained formats include some that themselves are pickles or contain pickles.
