# Improving Visual-Semantic Embeddings with Hard Negatives

Code for the image-caption retrieval methods from ["VSE++: Improving 
Visual-Semantic Embeddings with Hard Negatives" (Faghri, Fleet, Kiros, Fidler.  
2017)](https://arxiv.org/abs/1707.05612).

## Dependencies
We recommended to use Anaconda for the following packages.

* Python 2.7
* [PyTorch](http://pytorch.org/) (>0.2)
* [NumPy](http://www.numpy.org/) (>1.12.1)
* [TensorBoard](https://github.com/TeamHG-Memex/tensorboard_logger)

* Punkt Sentence Tokenizer:
```python
import nltk
nltk.download()
> d punkt
```

## Download data

Download the dataset files and pre-trained models. We use splits produced by [Andrej Karpathy](http://cs.stanford.edu/people/karpathy/deepimagesent/). The precomputed image features are from [here](https://github.com/ryankiros/visual-semantic-embedding/) and [here](https://github.com/ivendrov/order-embedding). To use full image encoders, download the images from their original sources [here](http://nlp.cs.illinois.edu/HockenmaierGroup/Framing_Image_Description/KCCA.html), [here](http://shannon.cs.illinois.edu/DenotationGraph/) and [here](http://mscoco.org/).

```bash
wget http://www.cs.toronto.edu/~faghri/vsepp/vocab.tar
wget http://www.cs.toronto.edu/~faghri/vsepp/data.tar
wget http://www.cs.toronto.edu/~faghri/vsepp/runs.tar
```

We refer to the path of extracted files for `data.tar` as `$DATA_PATH` and 
files for `models.tar` as `$RUN_PATH`. Extract `vocab.tar` to `./vocab` 
directory.

## Evaluate pre-trained models

```python
from vocab import Vocabulary
import evaluation
evaluation.evalrank("$RUN_PATH/coco_vse++/model_best.pth.tar", data_path="$DATA_PATH", split="test")
```

To do cross-validation on MSCOCO, pass `fold5=True` with a model trained using 
`--data_name coco`.

## Training new models
Run `train.py`:

```bash
python train.py --data_path "$DATA_PATH" --data_name coco_precomp --logger_name save/coco_vse++ --max_violation reset_train
```

Arguments used to train pre-trained models:

| Method    | Arguments |
| :-------: | :-------: |
| VSE0      | `--no_imgnorm` |
| VSE++     | `--max_violation` |
| Order0    | `--measure order --use_abs --margin .05 --learning_rate .001` |
| Order++   | `--measure order --max_violation` |


## Reference

If you found this code useful, please cite the following paper:

    @article{faghri2017vse++,
      title={VSE++: Improving Visual-Semantic Embeddings with Hard Negatives},
      author={Faghri, Fartash and Fleet, David J and Kiros, Jamie Ryan and Fidler, Sanja},
      journal={arXiv preprint arXiv:1707.05612},
      year={2017}
    }

## License

[Apache License 2.0](http://www.apache.org/licenses/LICENSE-2.0)
