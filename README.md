<h1>Multiple Linear Regression (MLR) & Deployment on Render</h1>

<hr>

<h2>PART 1️⃣ What is Multiple Linear Regression (MLR)?</h2>

<h3>🔹 Definition (Simple words)</h3>
<p>
<b>Multiple Linear Regression</b> is a supervised machine learning algorithm used to
<b>predict a numeric value</b> using <b>more than one input feature</b>.
</p>

<p>👉 It is an extension of <b>Simple Linear Regression</b>.</p>

<hr>

<h3>🔹 Mathematical Equation</h3>

<p><b>Simple Linear Regression:</b></p>
<p><code>y = mx + c</code></p>

<p><b>Multiple Linear Regression:</b></p>
<p>
<code>
y = b<sub>0</sub> + b<sub>1</sub>x<sub>1</sub> + b<sub>2</sub>x<sub>2</sub> + 
b<sub>3</sub>x<sub>3</sub> + ... + b<sub>n</sub>x<sub>n</sub>
</code>
</p>

<p><b>Where:</b></p>
<ul>
  <li><b>y</b> → Predicted output (target)</li>
  <li><b>x<sub>1</sub>, x<sub>2</sub>, x<sub>3</sub>...</b> → Input features</li>
  <li><b>b<sub>0</sub></b> → Intercept</li>
  <li><b>b<sub>1</sub>, b<sub>2</sub>, b<sub>3</sub>...</b> → Coefficients (weights)</li>
</ul>

<hr>

<h3>🔹 Example (Real-world)</h3>

<p><b>Predict House Price using:</b></p>
<ul>
  <li>Area</li>
  <li>Number of bedrooms</li>
  <li>Location</li>
  <li>Age of house</li>
</ul>

<p><b>Mapping:</b></p>
<ul>
  <li>Area → x<sub>1</sub></li>
  <li>Bedrooms → x<sub>2</sub></li>
  <li>Location → x<sub>3</sub></li>
  <li>Age → x<sub>4</sub></li>
</ul>

<p>
The model learns how <b>each feature contributes</b> to the house price.
</p>

<hr>

<h3>🔹 Why we use MLR?</h3>
<ul>
  <li>✅ Uses multiple factors</li>
  <li>✅ More accurate than Simple Linear Regression</li>
  <li>✅ Widely used in salary, price, and demand prediction</li>
</ul>

<hr>

<h3>🔹 Training an MLR Model (Concept)</h3>
<ol>
  <li>Load dataset</li>
  <li>Split data into <b>X (features)</b> and <b>y (target)</b></li>
  <li>Apply <code>train_test_split</code></li>
  <li>Train model using <code>LinearRegression()</code></li>
  <li>Evaluate using:
    <ul>
      <li>R² Score</li>
      <li>Mean Squared Error (MSE)</li>
    </ul>
  </li>
</ol>

<hr>

<h2>PART 2️⃣ What does “Deploying an MLR model” mean?</h2>

<h3>🔹 Meaning of Deployment</h3>
<p>
Deployment means making your trained ML model
<b>available on the internet</b> so users can give inputs and get predictions.
</p>

<p><b>Example:</b></p>
<p>
User Inputs → Web App → MLR Model → Prediction
</p>

<hr>

<h3>🔹 Project Flow (MLR Deployment)</h3>

<pre>
Dataset
   ↓
Train MLR Model
   ↓
Save model (.pkl)
   ↓
Flask app (app.py)
   ↓
Deploy on Render
   ↓
Public URL
</pre>

<hr>

<h2>PART 3️⃣ Files Required to Deploy MLR on Render</h2>

<h3>1️⃣ app.py (Main Flask App)</h3>
<p>This file:</p>
<ul>
  <li>Loads the saved MLR model</li>
  <li>Takes input from user (HTML / JSON)</li>
  <li>Returns prediction</li>
</ul>

<pre><code>
import pickle
from flask import Flask, request, render_template

app = Flask(__name__)
model = pickle.load(open("MLR_Model.pkl", "rb"))

@app.route("/", methods=["GET", "POST"])
def predict():
    if request.method == "POST":
        x1 = float(request.form["x1"])
        x2 = float(request.form["x2"])
        x3 = float(request.form["x3"])

        result = model.predict([[x1, x2, x3]])
        return render_template("index.html", prediction=result[0])

    return render_template("index.html")

if __name__ == "__main__":
    app.run()
</code></pre>

<hr>

<h3>2️⃣ MLR_Model.pkl</h3>
<ul>
  <li>Saved trained MLR model</li>
  <li>Created using:</li>
</ul>

<pre><code>
pickle.dump(model, open("MLR_Model.pkl", "wb"))
</code></pre>

<hr>

<h3>3️⃣ requirements.txt</h3>

<pre><code>
flask
numpy
pandas
scikit-learn
gunicorn
</code></pre>

<hr>

<h3>4️⃣ Procfile</h3>
<p><b>Very important for Render ❗</b></p>

<pre><code>
web: gunicorn app:app
</code></pre>

<p>
<b>Meaning:</b><br>
app → app.py<br>
app → Flask object name
</p>

<hr>

<h3>5️⃣ templates/index.html</h3>
<p>
HTML page used to take user input values.
</p>

<hr>

<h2>PART 4️⃣ How to Deploy MLR Model on Render</h2>

<h3>STEP 1️⃣ Push Code to GitHub</h3>

<pre>
ML_MLR_Deployment/
│
├── app.py
├── MLR_Model.pkl
├── requirements.txt
├── Procfile
└── templates/
    └── index.html
</pre>

<hr>

<h3>STEP 2️⃣ Create Render Account</h3>
<ul>
  <li>Go to https://render.com</li>
  <li>Sign up using GitHub</li>
</ul>

<hr>

<h3>STEP 3️⃣ Create Web Service</h3>
<ul>
  <li>Click <b>New → Web Service</b></li>
  <li>Select your GitHub repository</li>
  <li>Choose branch → <b>main</b></li>
</ul>

<hr>

<h3>STEP 4️⃣ Configure Render Settings</h3>

<p><b>Environment:</b> Python</p>

<p><b>Build Command:</b></p>
<pre><code>pip install -r requirements.txt</code></pre>

<p><b>Start Command:</b></p>
<pre><code>gunicorn app:app</code></pre>

<hr>

<h3>STEP 5️⃣ Port Binding (Important)</h3>

<pre><code>
import os

app.run(
    host="0.0.0.0",
    port=int(os.environ.get("PORT", 5000))
)
</code></pre>

<p>Render assigns the PORT dynamically.</p>

<hr>

<h3>STEP 6️⃣ Deploy 🚀</h3>
<ul>
  <li>Click <b>Create Web Service</b></li>
  <li>Render installs dependencies</li>
  <li>Loads the MLR model</li>
  <li>Starts Flask application</li>
  <li>Provides a <b>LIVE URL</b></li>
</ul>

<p>🎉 Your MLR model is now live on the internet!</p>

<hr>

<h2>PART 5️⃣ How User Uses Your Deployed MLR Model</h2>

<ol>
  <li>Open Render URL</li>
  <li>Enter feature values</li>
  <li>Click <b>Predict</b></li>
  <li>Model calculates:</li>
</ol>

<p>
<code>
y = b<sub>0</sub> + b<sub>1</sub>x<sub>1</sub> + b<sub>2</sub>x<sub>2</sub> + b<sub>3</sub>x<sub>3</sub>
</code>
</p>

<p>Prediction is displayed on the page.</p>

<hr>

<h2>FINAL SUMMARY 🧠</h2>

<h3>MLR:</h3>
<ul>
  <li>Predicts numeric output using multiple inputs</li>
  <li>Uses a linear equation</li>
  <li>Trained using supervised learning</li>
</ul>

<h3>Deployment:</h3>
<ul>
  <li>Save model (.pkl)</li>
  <li>Load model in Flask</li>
  <li>Deploy Flask app on Render</li>
  <li>Access model via browser</li>
</ul>
