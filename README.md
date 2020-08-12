# **Chatbot with Tensorflow and Keras**

---
This tensorflow and keras Chatbot is a general purpose chatbot that can have general conversation with you like a friend and not designed for targeting some certain specific task.
### **Model**

---
The design of the Chatbot model is based on the ***mulilayer-bidirectional seq2seq architecture with attention***. The seq2seq architecture is an encoder-decoder architecture which consists of two multilayered LSTM networks with attention: the Encoder LSTM and the Decoder LSTM. 

*   The encoder network is ***3-layer-Bidirectional*** LSTM with total of ***1024 units*** and ***dropout(0.3) applied***,
*   while, the decoder network is ***3-layer-Unidirectional*** LSTM with total of ***1024 units*** using only 0.7 of them at a time because of ***dropout(0.3)*** and with a ***Dense layer*** with ***units equal to the number of maximum possible tokens***(18000) mounted over it.

*   The model is evaluated on ***Sparse Categorical Crossentropy Loss*** with masking which means the loss for the padding inputs are considered 0.

**NOTE:**

*   The Encoder is Bidirectional while Decoder is Unidirectional due to the fact that the input is already known while the output is generated at each step.
*   I have applied ***Bahdanau Attention*** inside the Decoder class.



### **Working**

---
**ENCODER**

1.   The input to the Encoder class were the ***tokenized sequences*** obtained using the keras ***Tokenizer class*** which were then padded to certain ***maximum length (18)*** with keras ***pad_sequences function***.
2.   Inside the Encoder class the tokenized and padded inputs were embedded into 300 dimension vectors using the pre-trained ***en_vectors_web_lg word embeddings*** and then passed to the bi-directional LSTM layers with ***return_states and return_sequences*** parameters of each layer set to ***True*** so that both the sequences and states of each layer can be passed to the succedding layer as input and initial states.
3.   The states of last layer of encoder were summed manually and passed as initial state to decoder.





**DECODER**

1.   Since the output in the Decoder class is generated at the each step therefore the input to the decoder is a ***single tokenized "sos"*** (start of string) token only which is passed through 2 stacked LSTM layers after being embedded into 300 dimension vector with return_states and return_sequences parameters set to true. 
2.   The ***Context Vector*** *obtained by applying Bahdanau Attention using hidden state 'h' of previous layer and output of encoder class* is concatenated with the previous layer output and inputted to the last LSTM layer which was further passed to the dense layer and the token is predicted.
3.   This predicted token act as input for predictig next token of the sequence and same process continues until a "eos" (end of string) token is obtained or a certain maximum length (27) is reached.

### **Dataset**

---


The Chatbot is trained on a common but yet useful Cornell Movie Dialogs corpus Dataset. It contains a large metadata-rich collection of fictional conversations extracted from raw movie scripts and thus very useful for our general purpose conversation chatbot. The corpus includes:

*   220,579 conversational exchanges between 10,292 pairs of movie characters
*   involves 9,035 characters from 617 movies
*   in total 304,713 utterances

Both the Text files of the Dataset are included in the repo.
### **Environment**

---
I used the google colab gpu to train the model(VGG16) with tensorflow version 2.2.0. However, for just using the model your PC CPU would be enough but you need -

*   Spacy, and
*   en_vectors_web_lg pre-trained word embedding vectors

to be installed in your environment.



### **Usage**

---
In order to just use the model you required to run all the cells of the notebook except -

1.   the cell with Training Step,
2.   the cell with definition of train_step function, and
3.   the cell with definition of plot_history, as these cells are of no use when testing/using the model.

We are required to run all the cells of the notebook for using the model instead of loading it using single statement since the model is neither sequential nor functional but a multi subclassed model therefore saving of architecture is not supported and thus we required to define the architecture, optimizer etc everytime before restoring the weights of the model.
### **Result**


---
The training is not yet been completed that is why the results are not that good but yaa it is definitely working. The model has been trained on 45 epochs only yet and I am expecting about 300+ epochs for some good results.

I will upload the weights once I will achieve good results.
