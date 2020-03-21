# Adjacent-Encoder
This is the tensorflow implementation of the AAAI-2020 paper "[Topic Modeling on Document Networks with Adjacent-Encoder](/figure/AAAI20-Adjacent-Encoder.pdf)" by [Ce Zhang](https://sites.google.com/site/delvincezhang/) and [Hady W. Lauw](http://www.hadylauw.com/home).

Adjacent-Encoder is a neural topic model that extracts topics for networked documents for document classification, clustering, link prediction, etc.

![](/figure/model_comparison.jpg)

## Implementation Environment
- Python == 3.6
- Tensorflow == 1.9.0
- Numpy == 1.17.4

## Run
`python main.py`

### Parameter Setting
- -lr: learning rate, default = 0.001
- -ne: number of epoch, default = 1000
- -ti: transductive or inductive learning, transductive means we input all documents and links for unsupervised training, inductive means we split 80% for training, 20% for test, default = inductive
- -dn: dataset name
- -nt: number of topics, default = 64
- -x: 0 == Adjacent-Encoder, 1 == Adjacent-Encoder-X, default = 0 (Adjacent-Encoder)
- -tr: training ratio, this program will automatically split 10% among training set for validation, default = 0.8
- -ms: minibatch size, default = 128
- -sm: gaussian std.dev. (sigma) used for Denoising Adjacent-Encoder(-X), if do not want to use denoising variant, set this value to 0, default = 0
- -c: contractive regularizer for Contractive Adjacent-Encoder(-X), best performance = 1e-11, if do not want to use contractive variant, set this value to 0, default = 0
- -sp: 0 == no k-sparse, 1 == K-Sparse Adjacent-Encoder(-X), default = 0
- -k: number of nonzero hidden neurons of K-Sparse Adjacent-Encoder(-X), if do not use k-sparse variant, set above argument -sp to 0, default = 0.5 * num_topics

## Data
We extracted four independent citation networks (DS, HA, ML, and PL) from source Cora (http://people.cs.umass.edu/~mccallum/data/cora-classify.tar.gz). Note that the well-known benchmark Cora dataset as used in GAT is actually the ML subset. In addition to ML, we further created three new citation networks.

In `./cora` file we release these datasets, each of which contains adjacency matrix, content, label, label name, and vocabulary.

- adjacency matrix (NxN): a 0-1 symmetric matrix (A^T==A), and its diagonal elements are supposed to be 1.
- content (Nx|V|): each row is a Bag-of-Words representation of the corresponding document, and each column is a word in the vocabulary. Documents are represented by the word count.
- label (Nx1): label or category of each document. Labels are used only for evaluation, not for learning in our model.
- label name: the name of each label or category.
- vocabulary (|V|x1): words.

## Output
The document embeddings are output to the `./results` file. Each row represents one document embedding, and each column represents one dimension of the embedding, or one topic.

In transductive learning, training embeddings are the same as test embeddings. In inductive learning, training embeddings are those of training documents (no validation documents), and testing embeddings are inferred for testing documents.

## Reference
If you use our paper, including code and data, please cite

```
@inproceedings{adjenc,
    title={Topic Modeling on Document Networks with Adjacent-Encoder},
    author={Zhang, Ce and Lauw, Hady W},
    booktitle={Thirty-fourth AAAI conference on artificial intelligence},
    year={2020}
}
```
