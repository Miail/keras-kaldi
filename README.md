# Keras Interface for Kaldi ASR

## Why these Routines?

This code interfaces Kaldi tools for Speech Recognition and Keras 
tools for Deep Learning. Keras simplifies the latest deep 
learning implementations, unifies the two popular Theano and 
Tensorflow libraries, and has a growing user base. Kaldi, one of 
the best tools for ASR, thus needs an interface with Keras tools, 
and here is one. This code directly interacts with Kaldi style 
directories of data and alignments to build and test Deep 
Learning models in Keras.

## Features

1. Trains DNNs from Kaldi GMM system

2. Works with standard Kaldi data and alignment directories

3. Supports mini-batch training

4. Supports LSTMs, maxout and dropout training

5. Easily extendable to other deep learning implementations in 
  Keras

6. Decodes test utterances in Kaldi style

## Dependencies

1. Python 3.4+

2. Keras with Theano/Tensorflow backend

3. Kaldi

## Using the Code

Train a GMM system in Kaldi. Place steps_kt and run_kt.sh in the 
working directory. Configure and run run_kt.sh. To train LSTMs,
run run_kt_LSTM.sh.

## Code Components

1. train.py is the Keras training script. DNN structure (type of 
  network, activations, number of hidden layers and nodes) can be 
  configured in this script. train_LSTM.py trains LSTMs.

2. dataGenerator.py provides an object that reads Kaldi data and 
  alignment directories in batches and retrieves mini-batches for 
  training. dataGenSequences.py retrieves 3D mini-batches for
  LSTM training.

3. nnet-forward.py passes test features through the trained DNNs 
  and outputs log probabilities (log of DNN outputs) in Kaldi 
  format. nnet-forward-seq.py passes 3D arrays to LSTMs and
  outputs log probabilities.

4. kaldiIO.py reads and writes Kaldi-type binary features.

5. decode.py is the decoding script. decode_seq.py is the script
  for LSTMs.

6. align.sh is the alignment script.

## Training Schedule

The script uses stochastic gradient descent with 0.5 momentum. It 
starts with a learning rate of 0.1 for a minimum of 5 
iterations. When the validation loss reduces by less than 0.002 
between successive iterations, learning rate is halved, and is
contined to be halved after each epoch, 18 times.

## Results on Timit Phone Recognition

Timit database of 8 kHz sampling rate was used to train monophone,
triphone (300 pdfs), LDA+MLLT (500 pdfs), DNN and LSTM models.
Phone error rates are as follows:

1. Monophone: 34.25%

2. Triphone: 30.44%

3. LDA+MLLT: 27.03%

4. DNN (3 hidden layers of 1024 nodes, ReLU activations): 23.71%

5. LSTM (3 hidden layers of 256 nodes, Tanh activations, LDA+MLLT alignments): 23.02%

## Results on WSJ Corpus

WSJ SI-284 corpus of 8 kHz sampling rate was used to train monophone,
triphone (1000 pdfs), DNN and LSTM models. Word error rates are as follows:

1. Monophone - dev93: 37.76%, eval92: 27.95%

2. Triphone - dev93: 23.78%, eval92: 16.37%

3. DNN (3 hidden layers of 1024 nodes, ReLU activations) - dev93: 13.50%, eval92: 9.16%

4. LSTM (3 hidden layers of 256 nodes, ReLU activations) - dev93: 13.25%, eval92: 9.16%

## Notes

1. If using ReLU activations in LSTM, use Tensorflow backend.

2. Initialise Tensorflow with the correct GPU memory fraction.

## Contributors

D S Pavan Kumar

dspavankumar [at] gmail [dot] com

##Acknowledgements

Thanks to Dan Povey, Ram Sundaram, Naresh Kumar, Tejas Godambe and Veera Raghavendra for suggesting improvements and debugging.

## License

GNU GPL v3
