# Blinkti-Analysis
This dashboard provides a comprehensive analysis of Blinkit's retail performance metrics, helping stakeholders make data-driven decisions.
import fitz  # PyMuPDF for PDF text extraction
import re  # For text cleaning
import matplotlib.pyplot as plt  # For visualization
from collections import Counter  # To count word frequencies
from wordcloud import WordCloud  # To generate a word cloud
import nltk
from nltk.tokenize import word_tokenize
from nltk.corpus import stopwords

# Ensure required NLTK resources are downloaded
nltk.download('punkt')
nltk.download('stopwords')

# Function to extract text from PDF
def extract_text_from_pdf(pdf_path):
    doc = fitz.open(pdf_path)
    text = ""
    for page in doc:
        text += page.get_text("text") + "\n"
    return text

# Function to clean and preprocess text
def clean_text(text):
    text = text.lower()  # Convert to lowercase
    text = re.sub(r'\s+', ' ', text)  # Remove extra spaces
    text = re.sub(r'[^a-zA-Z0-9 ]', '', text)  # Remove special characters
    words = word_tokenize(text)
    words = [word for word in words if word not in stopwords.words('english')]
    return ' '.join(words)

# Function to analyze word frequency
def word_frequency_analysis(text):
    words = word_tokenize(text)
    word_counts = Counter(words)
    return word_counts

# Function to generate a word cloud
def generate_word_cloud(word_counts):
    wordcloud = WordCloud(width=800, height=400, background_color='white').generate_from_frequencies(word_counts)
    plt.figure(figsize=(10, 5))
    plt.imshow(wordcloud, interpolation='bilinear')
    plt.axis("off")
    plt.show()

# Main execution
doc_path = "Blinkit analysis.pdf"  # Replace with actual file path
extracted_text = extract_text_from_pdf(doc_path)
cleaned_text = clean_text(extracted_text)
word_counts = word_frequency_analysis(cleaned_text)

# Print top 10 frequent words
print("Top 10 Words:", word_counts.most_common(10))

# Generate Word Cloud
generate_word_cloud(word_counts)
