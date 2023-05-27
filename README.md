# Resources for _Looking for a Needle in a Haystack: A Comprehensive Study of Hallucinations in Neural Machine Translation_

This is the official repository for the paper _Looking for a Needle in a Haystack: A Comprehensive Study of Hallucinations in Neural Machine Translation_.

<hr />

> **Abstract:** *Although the problem of hallucinations in neural machine translation (NMT) has received some attention, research on this highly pathological phenomenon lacks solid ground. Previous work has been limited in several ways: it often resorts to artificial settings where the problem is amplified, it disregards some (common) types of hallucinations, and it does not validate adequacy of detection heuristics. In this paper, we set foundations for the study of NMT hallucinations. First, we work in a _natural_ setting, i.e., in-domain data without artificial noise neither in training nor in inference. Next, we annotate a dataset of over 3.4k sentences indicating different kinds of critical errors and hallucinations. Then, we turn to detection methods and both revisit methods used previously and propose using glass-box uncertainty-based detectors. Overall, we show that for preventive settings, (i) previously used methods are largely inadequate, (ii) sequence log-probability works best and performs on par with reference-based methods. Finally, we propose DeHallucinator, a simple method for alleviating hallucinations at test time that significantly reduces the hallucinatory rate. To ease future research, we release our annotated dataset for WMT18 German-English data, along with the model, training data, and code.*
<hr />

## The Annotated Corpus
The dataset contains 3,415 structured annotations for different types of pathologies and hallucinations. Specifically, the corpus in `./data/annotated_corpus.csv` contains annotations on six categories: (i) correctness, (ii) mistranslation of named-entities, (iii) omission (undergenerated translation), (iv) repetitions (translations with erroneous oscillatory character), (v) strong-unsupport (strongly-detached hallucinations) and (vi) full-unsupport (full-detached hallucinations).

## The detailed guidelines for annotation
The guidelines for annotation that were used by the translators are available in `./data/annotation_guidelines.pdf`. We highly encourage organizing tutorial sessions with annotators in order to review the guidelines with them.

## The Model and Training Data
We also make available the DE-EN model that was used to produce the translations and the training data (from WMT18) that was used to train it. Those resources are made available [here](https://www.mediafire.com/file/mp5oim9hqgcy8fb/checkpoint_best.tar.xz/file) and [here](https://www.mediafire.com/file/jfl7y6yu7jqwwhv/wmt18_de-en.tar.xz/file), respectively. We also provide the `sentencepiece` models that were used to preprocess the data (in `./sentencepiece_models/`), so you can run the model on additional data.

### How to use our model?
Our model is built on top of [*Fairseq*](https://github.com/facebookresearch/fairseq). We refer to their [repo folder on translation](https://github.com/facebookresearch/fairseq/blob/main/examples/translation) for additional information on how to preprocess, train and generate translations. 

NOTE: To obtain model-based statistics that are faithful to the dataset we release, we advise to force decode the translations in the dataset using `fairseq-generate` with `score-reference` activated.

### How to run MC-dropout on Fairseq models?
Look [here](https://github.com/facebookresearch/fairseq/tree/main/examples/unsupervised_quality_estimation) to find instructions on how to run MC-dropout inference with Fairseq models. You will also find instructions on how to compute similarity between multiple hypotheses with METEOR, which you can use to compute `MC-DSim`.

### How to obtain the translation statistics (e.g. sequence log-probability and attention maps)?
The sequence log-probability is already made available as a by-product of the translation when using `fairseq-generate` (see [here](https://github.com/facebookresearch/fairseq/blob/acd9a53607d1e5c64604e88fc9601d0ee56fd6f1/fairseq_cli/generate.py#L262) -- each `hypo` already contains the translation score and the last-layer attention maps).

### How to score translations using COMET/COMET-QE?
Look [here](https://github.com/Unbabel/COMET) to find detailed instructions on how to score translations with these COMET-based models. In our work, we used the following versions: `wmt20-comet-da` for COMET and `wmt20-comet-qe-da-v2` for COMET-QE.

### How to obtain scores using CHRF2?
Look [here](https://github.com/mjpost/sacrebleu) to find detailed instructions on how to score translations using CHRF2.

## Implementation of the detectors
We have mentioned how to obtain scores with `MC-DSim`, `Seq-Logprob`, `COMET`, `COMET-QE` and `CHRF2`. To obtain scores with `TokHal-Model`, we refer to the [original implementation](https://github.com/violet-zct/fairseq-detect-hallucination). 

## Citation
```bibtex
@inproceedings{guerreiro-etal-2023-looking,
    title = "Looking for a Needle in a Haystack: A Comprehensive Study of Hallucinations in Neural Machine Translation",
    author = "Guerreiro, Nuno M.  and
      Voita, Elena  and
      Martins, Andr{\'e}",
    booktitle = "Proceedings of the 17th Conference of the European Chapter of the Association for Computational Linguistics",
    month = may,
    year = "2023",
    address = "Dubrovnik, Croatia",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2023.eacl-main.75",
}
```
