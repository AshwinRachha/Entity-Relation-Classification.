# Entity Relation Classification.

Pytorch implementation of Entityt Relation Classification on NYT21 Dataset. 

Classifying semantic relations between entity pairs in sentences is an important task in Natural Language Processing (NLP). Most previous models for relation classification rely on the high-level lexical and syntactic features obtained by NLP tools such as WordNet, dependency parser, part-of-speech (POS) tagger, and named entity recognizers (NER). In addition, state-of-the-art neural models based on Recurrent Neural Networks do not fully utilize information of entity that may be the most crucial features for relation classification. To address these issues, as Part of the NLP course CS5984 (Natural Language Processing) I developed an end-to-end BERT neural model which incorporates an entity-aware attention mechanism to classify relations between two entites in a sentence.


## Example

#### Sentence Example : a12 new york\/region b1-7 enclave for middle class is put up for sale the owner of stuyvesant town and peter cooper village , two 60-year-old apartment complexes on the east side of manhattan , is auctioning them for a target price of nearly $ 5 billion .

#### Entity Relation Labels : stuyvesant town :: manhattan --> /location/neighborhood/neighborhood_of |
peter cooper village :: manhattan --> /location/neighborhood/neighborhood_of.

Meaning Stuyvesant town relates to manhattan as being its neighborhood and so does peter cooper village according to the train.tup file.

Our task is to take these relations and create a Supervised Neural Net based model to classify the relation between the entities.

## Model Architecture

<p float="left" align="center">
    <img width="600" src="https://user-images.githubusercontent.com/28896432/68673458-1b090d00-0597-11ea-96b1-7c1453e6edbb.png" />  
</p>

### **Method**

1. **Get three vectors from BERT.**
   - [CLS] token vector
   - averaged entity_1 vector
   - averaged entity_2 vector
2. **Pass each vector to the fully-connected layers.**
   - dropout -> tanh -> fc-layer
3. **Concatenate three vectors.**
4. **Pass the concatenated vector to fully-connect layer.**
   - dropout -> fc-layer

- **_Exactly the SAME conditions_** as written in paper.
  - **Averaging** on `entity_1` and `entity_2` hidden state vectors, respectively. (including \$, # tokens)
  - **Dropout** and **Tanh** before Fully-connected layer.
  - **No [SEP] token** at the end of sequence. (If you want add [SEP] token, give `--add_sep_token` option)

## Dependencies

- perl (For evaluating official f1 score)
- python>=3.6
- torch==1.6.0
- transformers==3.3.1

## How to run

```bash
$ python3 main.py --do_train --do_eval
```

- Prediction will be written on `proposed_answers.txt` in `eval` directory.

## Official Evaluation

```bash
$ python3 official_eval.py
# macro-averaged F1 = 88.29%
```

- Evaluate based on the official evaluation perl script.
  - MACRO-averaged f1 score (except `Other` relation)
- You can see the detailed result on `result.txt` in `eval` directory.

## Prediction

```bash
$ python3 predict.py --input_file {INPUT_FILE_PATH} --output_file {OUTPUT_FILE_PATH} --model_dir {SAVED_CKPT_PATH}
```

## References

- [Semeval 2010 Task 8 Dataset](https://drive.google.com/file/d/0B_jQiLugGTAkMDQ5ZjZiMTUtMzQ1Yy00YWNmLWJlZDYtOWY1ZDMwY2U4YjFk/view?sort=name&layout=list&num=50)
- [Semeval 2010 Task 8 Paper](https://www.aclweb.org/anthology/S10-1006.pdf)
- [NLP-progress Relation Extraction](http://nlpprogress.com/english/relationship_extraction.html)
- [Huggingface Transformers](https://github.com/huggingface/transformers)
- [https://github.com/wang-h/bert-relation-classification](https://github.com/wang-h/bert-relation-classification)
