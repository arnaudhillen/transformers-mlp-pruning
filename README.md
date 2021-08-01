The changes to the BERT model are made to src/transformers/models/bert/modelling_bert.py

# Steps to prune a BERT model.

Install via pip.
- pip install git+https://github.com/ArnoutHillen/transformers-mlp-pruning.git@master


Create a tokenizer and a model.
- tokenizer = AutoTokenizer.from_pretrained("bert-base-cased")
- model = AutoModelForSequenceClassification.from_pretrained("bert-base-cased", num_labels=2, avg_pool=True)


Prune individual neurons by providing a dictionary that has for each layer the neurons to keep as a list (e.g., for layer 3, {3:[0,1,2]}).
- model.bert.prune_all_mlp_neurons_except(neurons)


Prune feedforward networks (e.g., in layer 8 and 9).
- model.bert.encoder.layer[8].apply_ffn = False
- model.bert.encoder.layer[9].apply_ffn = False


Or prune entire layers (attention + feedforward + add and norm connections) by providing a tuple of the layer indices.
- model.remove_layers((10,11))
