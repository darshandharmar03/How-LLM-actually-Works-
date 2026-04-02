# How Tokenization actually Works
“An educational resource explaining tokenization and how Large Language Models (LLMs) process and understand text.”

## What You Will Learn

- What tokenization is
- Why tokens are needed for neural networks
- How vocabulary size affects models
- How Byte Pair Encoding (BPE) works
- Risks and limitations of tokenization


What tokenization is ?

Large language models (LLMs), however, use simpler processes than human cognitive processes. They employ algorithms based on neural networks to capture the relationships between words in large amounts of data and then use this information about relationships to interpret and generate sentences. Our discussion of how these algorithms work will begin with their input: sentences of text. In this chapter, we explore how the LLM processes these sentences to become inputs for the model. Just as language is critical for how you think and process information, the inputs to an LLM are crucial in influencing what kinds of concepts and tasks LLMs can perform.

Tokens as numeric representations
you will see that textual sentences are unnatural for the neural network algorithms that power LLMs because neural networks fundamentally employ numbers to do their work.
LLMs must convert human text into a numeric representation before working with it.
Tokens are the representations that LLMs use to break text into pieces that can be encoded as numbers
<img width="960" height="348" alt="image" src="https://github.com/user-attachments/assets/a03cbc61-0182-4cbd-857e-665ec6bfc93d" />

You can think of tokens as the smallest unit of text an LLM processes—an “atom,” if you will, the smallest part from which all other things are built on. Tokenization is the feature engineering of LLMs; it is critically essential because tokens are the only information a model interacts with..Tokens are not words but parts of words referred to as sub words, are presentation that is somewhere between words and letters. Intuitively, a token captures language’s minimum meaningful semantic unit. For example, the word schoolhouse will often get broken into two tokens, school and house, and the word thoughtful as thought and ful
  Ex:-   Tokens for Dis and dis are related, the only difference being that one start with a capital D. However, you can see that the model assigns the identifier 4944 to Dis and the identifier 834 to dis. That is, the model doesn’t inherently see any connection between the tokens representing Dis and dis even if we, as humans, see an obvious connection. The model doesn’t even see Dis or dis. For an LLM to process tokens, we must convert those tokens into numbers so that the model will see the numbers 4944 and 834.Importantly,the model doesn’t have any direct way to know that these tokens are related.
  
THE TOKENIZATION PROCESS

1. Receiving the text to process—This means obtaining text input as a string data type (a collection of letters ,digits ,or symbols) from a user ,the internet ,or whatever source that has the text you want.
2. Transforming the string—This often involves changing the string in some useful way, such as converting uppercase characters into lowercase. This could also be done for security reasons (eg the text came from a user, and we need to remove anything that might look like some malicious input)or to eliminate irrelevant variations in the text to help the algorithm learn better. This process is known as normalization.
3. Breaking the string into tokens—Once a string is available, it needs to be separated into a sequence of discrete substrings; these are the tokens found in the larger string. This is referred to as segmentation.
4. Mapping each token to a unique identifier—The unique identifier is usually an integer number, which produces output that the LLM can understand.

<img width="752" height="678" alt="image" src="https://github.com/user-attachments/assets/827f8ea2-c330-4ae7-83dd-456f97eb2acc" />


CONTROLLING VOCABULARY SIZE IN THE TOKENISATION

The last step of the tokenization process is where the vocabulary is built. The vocabulary of a model is the total number of unique tokens that are seen during training. When we give the algorithm data to learn from. It almost always takes a large amount of data to build a rich vocabulary with many unique tokens. Choosing the vocabulary for a model involves a series of trade-offs: the larger the vocabulary, the more information your model can process successfully.
If you have a vocabulary that is too large, it may make the model slower because of the large number of computations required to use it. The model may also consume an excessive amount of memory or disk storage, which makes it more difficult to transfer or share with other machines—for example, when deploying it as part of a software application.
You build the model’s vocabulary by processing the training data and identifying tokens. Each time you encounter a new token, you assign it a unique identifier based on the number of unique tokens you have already seen.
This process is often as simple as storing a counter set to 0 and incrementing it every time a new token is found. Once the process is complete, you have a tokenizer that effectively acts as an encoder. The tokenizer can receive text as input and return a numeric encoding of that text, which the LLM algorithms can use as their input
Vocabulary size is one factor that contributes to the size of a large language model (LLM). Therefore, discussing methods and trade-offs for controlling vocabulary size is essential. In this section, we will describe how changing the behavior of the tokenization process can influence the vocabulary size and affect the model’s capabilities and accuracy.

<img width="804" height="338" alt="image" src="https://github.com/user-attachments/assets/1ca939af-76c3-4fd9-82e8-60ae611c7312" />

Instead of representing “Hello” and “hello” as two separate tokens, they are mapped to a single unique token. This mapping makes a huge difference because every word that starts a sentence and gets capitalized would otherwise duplicate a word in the vocabulary with a capitalized version. Such normalization can also help handle various typos and misspellings.

TOKENIZATION IN DETAIL 

The normalization and segmentation steps in the tokenization process largely determine the vocabulary size. It show one of the most straightforward strategies for tokenization. This strategy follows a simple rule: whenever a space is encountered in the text, the larger string is split into tokens.
In the case of “hello world,” it is as simple as calling "hello world".split(" ") in Python. This is a reasonable approach because it is similar to how humans read sentences. However, it also introduces some subtle complexity.
What happens when punctuation appears in the text? If we use our whitespace rule to convert the string “hello, world” into ["hello,", "world"], we encounter a problem similar to capitalization. We end up with two distinct tokens for the same concept: "hello" and "hello,"
Traditional approaches often addressed this by removing punctuation and developing more complex rules for splitting strings into tokens. While this is a step in the right direction for reducing vocabulary size, manually specifying tokenization rules does not solve all concerns.
For example, rule-based tokenization strategies struggle significantly with languages like Chinese, which do not use spaces to separate words.

IDENTIFYING SUBWORDS WITH BYTE PAIR ENCODING

The general theme of large language models (LLMs) is to perform less feature engineering by hand and allow algorithms to do most of the work instead. For this reason, an algorithm known as Byte Pair Encoding (BPE) is commonly used to break strings into tokens.                        Byte Pair Encoding is an algorithm that breaks words into common subword sequences of characters. Today, BPE is usually implemented with a custom segmenter and almost no normalization.
Byte Pair Encoding (BPE) uses a heuristic approach to simplify tokenization. It starts by treating individual letters as tokens. Then, it finds pairs of adjacent letters that occur most frequently and combines them into subword tokens.The algorithm repeats this process many times, continuing to merge subword tokens until a certain threshold is reached and the vocabulary becomes “small enough.”
For example, in the first pass, the BPE algorithm examines the frequency of individual letters used in English. It may observe that the letters “n” and “g” appear together more frequently than “i” and “n.” As a result, it might produce tokens such as “i” and “ng.”
In a later pass, the algorithm may combine these tokens into “ing” if the sequence appears frequently compared to other combinations involving “ng.”
Once BPE reaches its stopping point, it may identify complete words such as “eating” and “drinking” as frequently occurring combinations. It can also capture “ing” as a suffix, allowing other words ending with this subword to be represented using the same token.
When the algorithm finishes, the resulting vocabulary contains tokens that represent both complete words and subwords.


<img width="978" height="379" alt="image" src="https://github.com/user-attachments/assets/9e72e2ca-d169-45ed-bafe-d81a534e7fdb" />


 UNDERSTANDING BPE IN TOKENIZATION

The BPE process may seem unusual at first, but it can be understood as a way of identifying common strings in a text corpus. For example, BPE will often learn to represent “New York” as a single token. This is useful because the state and city of New York frequently appear in text. Representing the entire concept as a single token makes it easier for the model to process and understand that information.
In most cases, common words become unique tokens, while rare words are represented as combinations of subwords. For instance, the word “loquacious” may be tokenized by GPT-4 into “lo”, “qu”, and “acious.”
This approach can be both helpful and imperfect. It works well because “acious” is a Latin suffix indicating inclination or tendency, which helps the model interpret unfamiliar words. However, it is also a limitation because the Latin root “loqu” is split into two tokens instead of one, making it slightly harder for the model to learn the meaning.
After BPE generates a vocabulary, model developers often manually add additional tokens for various reasons. For example, certain words may be important within a specific knowledge domain, and including them as tokens can help the model better capture nuanced meanings.
Developers also add special tokens that do not represent word parts but instead provide additional information to the model. Some common examples include:
•	[UNK] (Unknown token): Used when the tokenizer cannot process a symbol correctly.
•	[SYSTM] (System token): Used to distinguish between the model’s internal prompts and user-provided input, as well as other stylistic markers.
In multimodal models that process both text and images, special tokens indicate when the input stream switches between text data and image data.
When developing ChatGPT, OpenAI chose to use BPE to convert text into tokens. They also released their tokenizer as an open-source package called tiktoken.
However, several other tokenization algorithms exist, including WordPiece and SentencePiece, both developed at Google. Each algorithm has different trade-offs. For example:
•	WordPiece uses a different method for calculating the frequency of candidate subwords when building the tokenizer’s vocabulary.
•	SentencePiece can process entire sentences and preserve whitespace when calculating tokens, which may improve performance when building multilingual models.
Despite these alternatives, BPE remains the most widely used tokenization algorithm, including in many modern large language models.
Regardless of the chosen algorithm, the size of the tokenizer’s vocabulary is a critical parameter. It is determined by the data scientist or engineer responsible for training and refining the tokenizer. The following sections usually explore the considerations involved in selecting vocabulary size and other decisions made during the tokenizer development process.

THE RISKS OF TOKENIZTION

Tokenization is the first piece of the puzzle. It is a simple but effective strategy to produce the inputs to LLMs. You have learned how the size of the vocabulary plays a significant role in a model’s deployability, the tradeoff in recognizing nuance versus the unnecessary redundancy associated with making a vocabulary, how the tokenization process influences the size of the vocabulary, and how the token selection process can be automated with BPE. The choices made at tokenization time affect what LLMs can do today and will affect them in the future. These choices involve a few big-picture challenges to be aware of. To explore this topic further, two salient yet nuanced details of BPE are worth sharing some concerns about: the relationship between sentence length and token counts and the potential for LLMs to be confused by characters, known as homoglyphs, that appear identical yet have different binary encodings.
Longer Sentences Do Not Always Mean More Tokens
One unintuitive aspect of Byte Pair Encoding (BPE) is that longer sentences do not necessarily result in more tokens.To understand this, you have  two different strings are tokenized by GPT-3. The string “I’m running” is longer by one character than the string “I’m runnin.” However, it actually produces one fewer token.
 

<img width="853" height="285" alt="image" src="https://github.com/user-attachments/assets/16663da9-90a1-4505-a313-c5c776dbe141" />

This discrepancy occurs because Byte Pair Encoding (BPE) greedily searches for the smallest set of tokens needed to represent any input. In this case, the string “running” appears frequently enough in the training data that it is assigned its own token.
However, when the “g” is missing, there is no token for “runnin” in the vocabulary because that variation likely appeared rarely in the training data. As a result, “runnin” must be split into at least two tokens, such as “run” and “nin.”
This nuance in tokenizer implementation can easily lead to software bugs. Different tokenizers may produce different results when tokenizing the same string. When designing unit tests and infrastructure, it is important to keep this in mind to avoid confusion when upgrading or switching between tokenizer implementations that may generate tokens differently.
It can also affect the evaluation of large language models (LLMs), because many models are highly sensitive to added whitespace. Inconsistent tokenization can unintentionally lead to comparisons that are not truly equivalent.

HOMOGLPHYS CREATES CONFUSION

Homoglyphs are a problem developers may encounter when working with multiple human languages or when considering the security implications of processing externally provided data. When input comes from arbitrary users, it may sometimes be malicious and attempt to trick the model into undesirable behavior. One way this can happen with a large language model (LLM) is through a homoglyph attack.
A homoglyph occurs when two or more characters have different byte encodings but appear identical when displayed on a screen. For example, the Latin letter “H”, commonly used in Western European languages, looks the same as the Cyrillic “H”, which is used in many languages across Eastern Europe and Central Asia.
Algorithms such as Byte Pair Encoding (BPE) encode characters based on their byte representation. Because homoglyphs have different byte encodings, they are converted into different tokens. This can increase the number of tokens in a text, change how an LLM interprets the input, and increase computation costs.
A notable example of a homoglyph-related issue involves the Unicode character U+200B, known as the “zero-width space.” This character is used in typesetting and technically occupies space, but it does not display anything on the screen and does not visibly change how the text appears.The zero-width space is just one of many unusual characters defined in the Unicode specification. These characters can sometimes cause unexpected problems when processing text.
To prevent such issues, many systems perform normalization steps. These steps remove unusual characters and convert homoglyphs into a canonical form—for example, ensuring that anything visually resembling the letter “a” is encoded as the standard “a.”

LLMs  ARE BAD AT WORD GAMES


<img width="738" height="656" alt="image" src="https://github.com/user-attachments/assets/27745daf-1d12-4360-b07e-3f696ba8b5c5" />

Consider an example where you want to build an application that answers questions about a user’s prescription drugs. Drug names are often long and confusing, which makes them difficult for people to remember or spell correctly. Because a large language model (LLM) does not truly understand letters, it may confuse one drug’s name with another that has a similar but unusual spelling.
Drug names are relatively uncommon, so they may be tokenized differently even when there are small spelling mistakes. For example, in GPT-3, the word “Amoxicillin” and the common misspelling “Amoxicillan” do not share any common tokens.
This significantly increases the risk that the LLM might generate an incorrect response. In sensitive domains such as healthcare, this makes it especially important to thoroughly test the application, carefully engineer safeguards, or in some cases avoid relying on an LLM altogether.

 
 SUMMARY 

Tokenization is the fundamental process that large language models (LLMs) use to understand text by converting sentences into tokens.
Tokens are the smallest units of information in text that represent content. Sometimes they correspond to full words, but often they represent parts of words or subwords.
Tokenization also involves normalizing text into a standard representation. This may include converting characters to lowercase or translating the byte encoding of Unicode characters so that visually identical characters use the same encoding.
Another important step is segmentation, which breaks text into words or subwords. Algorithms such as Byte Pair Encoding (BPE) automatically learn how to efficiently segment text based on the statistical frequency of letter combinations in a training dataset.
The result of building a tokenizer is called the vocabulary, which is the unique collection of word and subword tokens that the tokenizer can use to represent processed text.
The size of the tokenizer’s vocabulary affects both the LLM’s ability to accurately represent data and the amount of storage and computational resources required to process and predict text.
Inside an LLM, tokens are represented as numbers. Because of this, the model does not inherently understand relationships between tokens, such as prefixes, suffixes, or similarities between words.
To support specialized knowledge domains, automatically trained tokenizers may be augmented with additional tokens that are important for specific applications.
Finally, tokenizers that do not understand individual letters or digits may struggle with tasks such as arithmetic operations or simple word-based games.


 
