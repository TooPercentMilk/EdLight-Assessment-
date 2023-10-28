# **EdLight Assessment - Bradley Kerr**
## Strategy
This task, image captioning, is a very well studied and documented overlap between NLP and CV. Because of this, I decided the best method to train a model was to utilize transfer learning for the majority of the task. 
- For extracting image features, I used the VGG16 architecture. Although this is a detabably outdated model, it demonstrates consistent and reliable performance, and there is plenty of documentation and tutorials on it. This model is directly accessible through the keras applications kit.
- For extracting caption features, I used the pretrained GloVe embeddings. Token embeddings is a better soution for this task than other representations like bag-of-words, as most NLP models perform better with dense vectors than sparse. I decided to use pretrained embeddings over learning task-specific embeddings for a few reasons. First, there is a lot of literature demonstrating that pretrained embeddings on a massive corpus often outperforms learned embeddings. Second, the guide on image captioning that I was reviewing used the flickr8k dataset, which is twice as large as this dataset. Given how much smaller our dataset is, I was skeptical that I'd have enough data to learn useful embeddings. If the captions are not strong though, I may rework the architecture to use the keras embeddings layer. I used the built-in keras LSTM model for the text processing, as LSTM's seem to be the best current method of addressing the vanishing gradient and exploding gradient problems. 

## Data Augmentation
- The first obvious challenge to me was the varying image size. Most of the work I've done in the past has been with fixed input dimensions. The three main options I'm aware of (barring using a different model altogether) are cropping, resizing, or padding. I determined that all methods have downsides, and that the best approach was to use the resize_with_pad method. Just cropping wouldn't work because some of the images are very thin rectangles while others are square. Given that much of the image is text-based, and the captions seemingly rely on the dimensions of the shaped sometimes, I figure simply resizing would stretch the aspect ratio's too much. Resizing while padding the extra space seemed like the best choice to me.
- I considered greyscaling the images as well, but the VGG16 model only accepts 3-channel input, so I wouldn't gain much from doing so.
- For the text, I decided to tokenize the raw captions with the nltk tokenizer, as I am more familiar with it than the keras tokenizer.

## Hyperparameters
- **Train/Test Split:** I decided to use an 80/20 split. The image captioning guide I was reviewing used 90/10, but I feel that this dataset is much less diverse than the images on flickr8k, so perhaps 80% for training is sufficient. I didn't want to go further like 70/30 since the dataset is still fairly small, and I think 20% gives interpretable enough test data for evaluation.
- **Dropout:** I split the training data into training and validation splits. I then performed a grid search on the dropout value and found ___ to be the best dropout value.
- **Learning Rate:** I split the training data into training and validation splits. I then performed a grid search on the dropout value and found ___ to be the best dropout value.
- **Batch Size** Given my limited access to a GPU, I had some contraints with the batch size. I still wanted to have a fairly large batch size, as that would increase model performance. I settled on 32, which is small enough that my computer could run it, while not sacrificing too much accuracy.

## Evaluation
For evaluation, I primarily used the BLEU benchmark scoring metric.