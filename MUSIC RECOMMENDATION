from flask import Flask, render_template, request, jsonify
import pandas as pd
from sklearn.metrics.pairwise import cosine_similarity
from sklearn.preprocessing import MinMaxScaler
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.metrics.pairwise import cosine_similarity
import numpy as np

app = Flask(__name__)

# Sample music data
music_data = {
    'music_id': [101, 102, 103, 104, 105],
    'title': ["Shape of You" ,"Someone Like You" ,"Bohemian Rhapsody","Billie Jean" ,"Hey Jude" ],
    'artist': [ "Ed Sheeran","Adele","Queen","Michael Jackson","The Beatles"],
    'genre': ['Pop', 'Rock', 'latin','R&B', 'Pop/R&B' ]
}
df = pd.DataFrame(music_data)

# Create a TF-IDF Vectorizer
tfidf = CountVectorizer(stop_words='english')
tfidf_matrix = tfidf.fit_transform(df['genre'])

# Compute the cosine similarity matrix
cosine_sim = cosine_similarity(tfidf_matrix, tfidf_matrix)

# Flask routes
@app.route('/')
def index():
    return render_template('index.html')

@app.route('/recommend', methods=['POST'])
def recommend_music():
    data = request.get_json()
    music_id = int(data['music_id'])
    sim_scores = list(enumerate(cosine_sim[music_id]))
    sim_scores = sorted(sim_scores, key=lambda x: x[1], reverse=True)
    sim_scores = sim_scores[1:6]
    music_indices = [i[0] for i in sim_scores]
    recommendations = df.iloc[music_indices]
    return jsonify(recommendations.to_dict(orient='records'))

# Run the Flask app
if __name__ == '__main__':
    app.run(debug=True)
