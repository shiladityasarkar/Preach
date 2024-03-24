# Welcome to PREACH: Streamline Your Presentations!
### :sparkles: Your ultimate solution for transforming Chaos into Clarity through your presentations! 🚀

## What is PREACH? 🤔
$PREACH$, with the tagline "From Chaos To Clarity," is an innovative platform designed to revolutionize the way you create presentations. Our project empowers users to effortlessly craft concise PowerPoint presentations generated from Audio 🎤, Video 📹 Or Textual 📰 user inputs, ensuring that their message is conveyed clearly and comprehensively. :confetti_ball:

## Why the Name? :zap:
 The name $PREACH$ embodies our mission to transition from chaos to clarity in presentations. Often lengthy videos, or audios are too tedious to watch or listen to. A short and simple summary goes a long way into understanding the overall appeal of the multimedia in question.  By summarizing lengthy content into key points and structuring it effectively, PREACH ensures that your message is delivered with precision, one where you can Preach your ideas with impact.

## Why use PREACH❓
### The Effectiveness of Summaries and Presentations :memo:
 The effectiveness of summaries and PowerPoint presentations lies in their ability to deliver information concisely. PREACH automates this process, facilitating better retention of information and enhancing audience understanding.  
### Time-saving Automation 🕐
 Employees and students often struggle to create presentations, as it can be tedious and time-consuming. Often, it is challenging to identify key points and gather enough relevant content for slides as well. This project alleviates this burden by automating the process, allowing users to focus on other critical tasks.
### Accessibility :woman_in_tuxedo: :person_in_tuxedo:
 PREACH is accessible to individuals, regardless of their expertise in presentation. It's easy to use procedure ensures that users from diverse backgrounds can benefit from its features.



# 	:flight_departure: Get Started! :tada: 
Are you ready to streamline your presentations and make an impact like never before? Dive into PREACH and transform chaos into clarity today! ❇️

# Project Workflow :car:
- Video-Audio to Transcript Converter 
- Text Preprocessing 
    - Method 1: Preprocessing Text for Summary
    - Method 2: Preprocessing Text for Keyword Extraction
    - Method 3: Preprocessing Text WordCloud Generation 
- WordCloud Generation
- Keyword Generation using TF-IDF 
- Summary Generation 
    - Within 50 Words 
    - Extended Version 
- Presentation Content Generation

# Video-Audio to Transcript Converter 📹 ➡️ 📰
1. **Audio Conversion**:  If the input file is a video, the first step is to extract the audio from the video file. This is necessary because the speech recognition library, SpeechRecognition, works with audio files. The provided function *convert_video_to_transcript* performs this task by utilizing the moviepy library to separate the audio track from the video file and save it as a temporary WAV audio file.
 
 ```
def convert_video_to_transcript(video_path):
    
    audio_path = 'temp_audio.wav'
    video = mp.VideoFileClip(video_path)
    audio = video.audio
    audio.write_audiofile(audio_path)

    
    transcript = convert_audio_to_transcript(audio_path)

   
    os.remove(audio_path)

    return transcript
```
2. **Speech Recognition**:  Once the audio file is obtained (either directly or from the video), the next step is to transcribe the audio into text using the SpeechRecognition library. The *convert_audio_to_transcript* function uses SpeechRecognition to transcribe the audio data into text format. It utilizes the Google Web Speech API for transcription.

```
def convert_audio_to_transcript(audio_path):
    recognizer = sr.Recognizer()
    with sr.AudioFile(audio_path) as source:
        audio_data = recognizer.record(source)
        transcript = recognizer.recognize_google(audio_data)
    return transcript
```
3. **Handling Different Input Types**:  The *handle_input* function detects the type of input data provided by the user. Depending on the input type, the appropriate conversion method is called to generate the transcript. It supports various input formats as follows:
    - ivideo files: .mp4, .avi
    - audio files: .wav, .mp3
    - text files: .txt
  
```
def handle_input(input_data):

    if input_data.endswith('.mp4') or input_data.endswith('.avi'):
        
        transcript = convert_video_to_transcript(input_data)
    elif input_data.endswith('.wav') or input_data.endswith('.mp3'):
       
        transcript = convert_audio_to_transcript(input_data)
    elif input_data.endswith('.txt'):
        
        with open(input_data, 'r') as file:
            transcript = file.read()
    else:
      
        transcript = input_data
    
    return transcript
```

# Text Preprocessing 🗞️
1. **For Summary Generation**: This method is designed to prepare the text for generating summaries. It retains all sentences, including punctuations and stopwords, as it is necessary for creating a concise summary without losing context.

    ```
   def preprocess_text(text):
    
     text = emojis.decode(text)
    
     words = nltk.word_tokenize(text)
    
     preprocessed_text = " ".join(words)
    
     return preprocessed_text
   ```
3. **For Keyword Generation**: This method preprocesses the text for generating keywords. It includes lemmatization to convert words into their base form, which enhances the representation of keywords.

    ```
   def preprocess_text1(text):
     text = emojis.decode(text)
    
     words = word_tokenize(text)
    
     stop_words = set(stopwords.words('english'))
     words = [word for word in words if word.lower() not in stop_words and word not in string.punctuation and word.isalpha()]
    
     lemmatizer = WordNetLemmatizer()
     words = [lemmatizer.lemmatize(word) for word in words]
    
     return words

   ```
4. **For WordCloud Generation**: This method preprocesses the text specifically for generating word clouds. It removes emojis, punctuation, and stopwords, and converts all words to lowercase. However, it does not perform lemmatization because the word cloud visualization benefits from maintaining the original form of words.
   
   ```
   def preprocess_text2(text):
    
     text = emojis.decode(text)
    
     words = nltk.word_tokenize(text)
    
     words = [word for word in words if word not in string.punctuation]
    
     stop_words = set(stopwords.words('english'))
     words = [word for word in words if word.lower() not in stop_words]
    
     words = [word.lower() for word in words]
    
     preprocessed_text = " ".join(words)
    
     return preprocessed_text


   ```
# WordCloud Generation ☁️
Word clouds offer a visually appealing way to represent the frequency of words in a text. They highlight the most prevalent terms, making it easier to identify key themes or topics.

The **generate_wordcloud** function preprocesses the transcript generated and then generates a word cloud visualization using the default font. The resulting word cloud provides a graphical representation of the most frequently occurring words in the text.

```
def generate_wordcloud(text):
    print("Generating Wordcloud...")

    text = preprocess_text2(text)

    wordcloud = WordCloud(width=800, height=400, background_color='white').generate(text)

    plt.figure(figsize=(10, 5))
    plt.imshow(wordcloud, interpolation='bilinear')
    plt.axis('off')
    plt.show()
```
# Keyword Generation using TF-IDF 🔑
Keyword extraction enables the identification of significant terms within a document, providing insights into its main themes or subjects.The generate_keywords function preprocesses the input summary and then utilizes the TF-IDF (Term Frequency-Inverse Document Frequency) technique to extract keywords. 

### Why TF-IDF?
TF-IDF is employed for keyword generation due to its ability to assign weights to words based on their importance within a document relative to a collection of documents. This technique helps in identifying significant terms that are characteristic of the content and distinguish them from common terms. TF-IDF considers both the frequency of a word in the document (Term Frequency) and its rarity across all documents (Inverse Document Frequency), resulting in keywords that are both relevant and distinctive to the document.

```
def generate_keywords(summary, num_keywords=6):
    print("Generating Keywords...")

    words = preprocess_text1(summary)
    processed_text = " ".join(words)
    
    vectorizer = TfidfVectorizer()
    
    vectorizer.fit([processed_text])
    
    tfidf_matrix = vectorizer.transform([processed_text])
    
    feature_names = vectorizer.get_feature_names_out()
    
    scores = tfidf_matrix.toarray().flatten()
    
    keywords = [feature_names[i] for i in scores.argsort()[::-1][:num_keywords]]
    
    return keywords
```
# Summary Generation :red_envelope:
1. 50 Word Summary: For the initial summary generation, we aim to condense the document's content into a concise 50-word summary, capturing its essence effectively. This succinct summary serves as a quick overview, easy to be included in the powerpoint. Leveraging Intel's NeuralChat chatbot technology, we dynamically generate the summary by querying the chatbot with a specific prompt tailored to the input text.

```
def generate_summary(text):
    print("Generating Summary...")
    text=preprocess_text(text)
    question = f"Can you generate a 50 word summary for the following paragraph: {text}?"
    response = chatbot.predict(question)
    return response
```   
2. Extended Summary: For the extended summary generation, we aim to provide a concise yet comprehensive overview of the document content, ideally representing 15% of the total word count in the original text. To achieve this, we begin by calculating the target word count, which amounts to 15% of the total words present in the document. This calculated value serves as a guideline for generating a summary that captures the essence of the input material. Subsequently, we leverage NeuralChat, an AI-powered chatbot, to dynamically generate a summary with the specified word count.

   ```
   def generate_summary1(text,fifteen_percent):
      print("Generating Summary...")
      text=preprocess_text(text)
      question = f"Can you generate a {fifteen_percent} word summary for the following paragraph: {text}?"
      response = chatbot.predict(question)
      return response
   ```

   ```
   def calculate_percentage_of_words(transcript_text):
      words = transcript_text.split()
    
      total_words = len(words)
      percentage=15
      fifteen_percent = int(total_words * (percentage / 100))
    
      return fifteen_percent
   ```
# Presentation Content Generation 🎥 📜
In the subsequent step, we proceed with the generation of PowerPoint presentation content, a crucial aspect of our project. Utilizing the capabilities of Intel's NeuralChat, we dynamically curate the content for the presentation slides. This includes generating the presentation title, crafting a comprehensive table of contents, and providing relevant content for individual slides. By querying the model with specific prompts tailored to each element, we ensure that the generated presentation content accurately reflects the essence of the input document.

```
def generate_title(text):  
    print("Generating Title...")
    system_input = "You are a creative writing assistant. Your mission is to help users generate beautiful and thought provoking powerpoint presentation titles based on the text they input. Generate 1 title along with its meaning in not more than 1 line below ."
    question = f"Can you generate a title based on the following topic:{text} for my powerpoint?"
    response = generate_response(system_input, question)
    return (response)

def generate_tables1(text):
    print("Generating Contents Table...")
    text=preprocess_text(text)
    question = f"I'm writing a document on the summary: {text}, and I need a table of contents with only 5 sections to organize the content effectively starting with '1. Introduction' and ending with    
    '5.Conclusion'. Please generate the table of contents for me with exactly 5 items following the pattern mentioned."
    response = chatbot.predict(question)
    return response

def generate_para(tablec,text): 
    print("Generating paragraphs...")
    system_input = "You are a creative writing assistant. Your mission is to help users generate detailed information and content based on a given table of contents and input topic. Make it creative,structured 
    with bullet points and paragraphs and detailed information."
    question = f"Can you generate one elaborate paragraph each for the table of contents {tablec} and based on the reference to the following summary:{text}"
    response = generate_response(system_input, question)
    return (response)

```

# Intel’s Developer Cloud 🌩️

Utilizing Intel Developer Cloud, particularly through its OneAPI capabilities, significantly expedited our AI model development and deployment processes. By harnessing the power of Intel's cutting-edge CPU and XPU technologies, we experienced unparalleled speed and reliability in our computations and model training. This acceleration allowed us to iterate quickly, experiment with various model architectures, and fine-tune parameters, achieving optimal results in record time.  

This provided a seamless integration of diverse compute architectures, including CPUs, and GPUs, 192 GB RAM, 220 GB of file storage, and even 120 days of free access to all Intel® oneAPI toolkits and components enabling us to leverage the full spectrum of hardware resources for our AI projects. The ease of use and accessibility of Intel Developer Cloud, combined with the comprehensive suite of tools and resources offered by OneAPI, streamlined our workflow and simplified infrastructure management. 

While attempting to install the necessary dependencies on platforms like Google Colab, we encountered recurring errors indicating that the available RAM resources were fully utilized. This limitation significantly hindered our progress and impeded the smooth execution of our project tasks. However, upon transitioning to Intel Developer Cloud, we experienced a stark contrast in performance and reliability. The infrastructure provided by Intel Developer Cloud facilitated seamless implementation without encountering any resource constraints or errors related to memory exhaustion. This ensured a hassle-free development environment, enabling us to focus solely on the task at hand and accelerate our project timelines efficiently.

# Intel’s Neural-Chat in Our Project 💭

Intel's Neural Chat model, designated as **'Intel/neural-chat-7b-v3-1'**, stands out as a powerful conversational AI model designed to understand and respond to natural language queries effectively. In our project, this model played a pivotal role in dynamically generating content for our PowerPoint presentations. 
- The chatbot from intel_extension_for_transformers offers a straightforward yet powerful solution for conversational interactions. With just a few lines of code, we could create a basic chatbot that responds to user input, providing a seamless conversational experience. In our project, this simple "hello world" program proved to be more than just a demonstration. It played a crucial role in content generation, offering a user-friendly interface for querying the Intel Neural Chat model.

- Utilizing the **model_name = 'Intel/neural-chat-7b-v3-1'**, we instantiated the chatbot by loading the pre-trained model and tokenizer. This model, built upon advanced transformer architectures, is capable of generating elaborate responses with nuanced understanding, thanks to its training on extensive conversational datasets. When combined with the tokenizer, it enabled the chatbot to interpret user queries and generate precise, contextually relevant responses.

The synergy between the simple chatbot and the advanced Neural Chat model was evident in our project workflow. Depending on the context and requirements, we leveraged either the basic chatbot or the more sophisticated Intel Neural Chat model to handle user queries effectively. 

1. By querying the Neural Chat model with prompts tailored to our specific needs, such as generating presentation titles, crafting table of contents, and providing slide content, we were able to efficiently curate the presentation content.  
2. Additionally, both components played a crucial role in summary generation, providing concise and informative summaries based on user input. 
3. Moreover, one notable advantage of using Intel's Neural Chat model is its accessibility and cost-effectiveness. Unlike many advanced AI models that require costly subscriptions or licensing fees, Intel's model is freely available for use. This accessibility not only encourages widespread adoption but also enables developers and researchers to leverage its capabilities without financial constraints. 
The model's documentation is clear and straightforward, making it easy for users to understand its functionalities and integrate it into their projects seamlessly. 

