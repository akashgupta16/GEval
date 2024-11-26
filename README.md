# GEval
---

GEval-An Google AI Powered Web Application for Project Code Evaluation
Problem Statement
In the ever-evolving world of software development, automating code quality assessments has become a necessity. Evaluating code quality is a critical task in software development, hackathons, and coding competitions. However, when there are many participants or repositories to review, the process can become tedious and time-consuming. 
Our Solution
An AI-powered web application solves that problem by automating code evaluation.
This application integrates Google Firebase for authentication and real-time analytics, connects with GitHub repositories, and leverages Google's Gemini 1.5 Flash API for AI-driven evaluations. Deployed seamlessly on Google Cloud Run ☁️, this scalable solution offers unparalleled performance, making it an essential tool for developers and organizations.
This blog explores the features, use cases, and implementation details of an AI-powered web application designed for automated code evaluation.

Key Features
🤖 AI-Powered Code Evaluation: Automatically evaluates code for quality, structure, and efficiency using Gemini 1.5 Flash API.
🔐 Secure Authentication: Offers login options via Google Sign-In 🟢 and email/password using Firebase.
📊 Real-Time Insights: Tracks user activities and application performance with Firebase Analytics.
🌐 GitHub Integration: Fetches repositories for analysis directly from GitHub.
☁️ Scalable Deployment: Deployed on Google Cloud Run for global availability and cost efficiency.
⏱️ Time-Saving Solution: Reduces the time required to evaluate and compare projects, especially in hackathons and coding competitions.
🛡️Security-Focused Design: Implements robust session management and API key protection.

---

Use Cases
🛠️ Automating Code Reviews: Streamlines quality assessments for large repositories.
📘 Developer Education: Assists developers in understanding code strengths and weaknesses.
🏆 Hackathons and Competitions: Helps organizers evaluate projects from a large number of participants quickly and consistently.
👩‍💻 Team Productivity: Enables managers to assess and compare contributions across teams.
📈 Real-Time Monitoring: Provides organizers with real-time analytics during competitions, such as project submission trends and evaluation summaries.

Addressing Challenges
In hackathons or coding competitions, organizers often face the following challenges:
🔄 Evaluating Numerous Submissions: Reviewing hundreds of projects can be overwhelming.
⚖️ Ensuring Fair Comparisons: It's difficult to standardize evaluations manually.
⏳ Meeting Deadlines: With limited time, thorough reviews are challenging.

This application tackles these problems by automating the evaluation process. Using AI, it analyzes code submissions and provides consistent feedback and scoring. Organizers can save time, focus on event management, and ensure fair assessments.

Workflow/High -Level Architecure

---

Technical Overview
Technologies Used
🌟 Google Firebase: Manages secure authentication and real-time analytics.
🤖 Gemini 1.5 Flash API: Performs AI-based evaluation of code quality.
🔗 GitHub API: Retrieves repositories for analysis.
☁️ Google Cloud Run: Ensures scalable and efficient deployment.

Architecture
Frontend: Collects GitHub repository URLs and displays evaluation results.
Backend Logic: Manages API integrations, user authentication, and AI-driven evaluations.
AI Evaluation: Processes code files with Gemini AI to generate feedback and scores.
Cloud Deployment: Runs in a containerized environment on Google Cloud Run for scalability.

---

Code Implementation
1. User Authentication
Firebase authentication ensures that only authorized users can access the evaluation functionality.
@app.route('/auth', methods=['POST'])
def authorize():
    token = request.headers.get('Authorization')
    if not token or not token.startswith('Bearer '):
        return "Unauthorized", 401

    token = token[7:]
    try:
        decoded_token = auth.verify_id_token(token)
        session['user'] = decoded_token
        return redirect(url_for('evaluate'))
    except:
        return "Unauthorized", 401
2. Fetching Files from GitHub
The application uses the GitHub API to list and retrieve file contents for analysis.
def get_repo_files(repo_url, branch="main"):
    try:
        api_url = f"{repo_url.replace('https://github.com', 'https://api.github.com/repos')}/contents?ref={branch}"
        response = requests.get(api_url)
        response.raise_for_status()
        return [item["download_url"] for item in response.json() if item["type"] == "file"]
    except Exception as e:
        print(f"Error fetching repository files: {e}")
        return []
3. AI-Powered Code Evaluation
Google's generative AI provides an in-depth evaluation of code quality.
def evaluate_code(content):
    prompt = (
        f"Evaluate the following code based on its quality, structure, "
        f"efficiency, and use case implementation. Provide a score out of 10. "
        f"Code:\n\n{content}\n\n"
        f"Explain your evaluation briefly."
    )
    response = model.generate_content(prompt)
    return response.text
4. Extracting Evaluation Scores
Extracts numerical scores from the AI-generated responses for reporting purposes.
def extract_score(result_text):
    try:
        for line in result_text.splitlines():
            if "Score:" in line:
                return float(line.split("Score:")[1].split("/")[0].strip())
    except Exception as e:
        print(f"Error extracting score: {e}")
    return None
5. Evaluation Workflow
The core functionality processes user inputs, fetches repository files, and computes scores.
@app.route("/evaluate", methods=["GET", "POST"])
@auth_required
def evaluate():
    if request.method == "POST":
        repo_url = request.form.get("repo_url")
        files = get_repo_files(repo_url)
        evaluations = [{"file": url, "response": evaluate_code(requests.get(url).text)} for url in files]
        # Process and render results...
    return render_template("evaluate.html")

---

How to Run the Application
Install Dependencies

pip install requests firebase-admin google-generativeai python-dotenv flask-cors
2. Environment Configuration
Place Firebase credentials in firebase-auth.json.
Add SECRET_KEY and other variables to a .env file.

3. Start the Application
python app.py
4. Access the Application Open http://localhost:8080 in a web browser.

Explore the Code Repository
To learn more about how this project works, you can explore the full code repository on GitHub. The repository contains all the code, documentation, and setup instructions you need to understand and implement the project.
👉 GitHub Repository: Explore the Code
Feel free to fork the repository, try the application, and customize it for your own use cases. Contributions are always welcome! 🌐✨
Security Features
Secure Sessions: Configures session cookies for HTTPS, HTTP-only, and SameSite policies.
Token Validation: Authenticates users using Firebase ID tokens.
API Key Protection: Secures sensitive keys with environment variables.

Deployment on Google Cloud Run ☁️
To ensure the application scales with the competition's needs, it's deployed on Google Cloud Run:
📶 Automatic Scaling: Handles spikes in traffic during submission deadlines.
🌍 Global Reach: Reduces latency for users around the world.
💰 Cost Efficiency: Charges only for the resources used, making it ideal for periodic events.Future Enhancements

Reponsive Application Design:️
Login/SignUpMenuLive DEMO


Future Enhancements
📝 Past Evaluations Memory: Store and display previously evaluated repositories upon user login.
🔒 Private Repository Support: Add OAuth for accessing private repositories.
🌍 Multi-Language Support: Expand to evaluate code written in multiple programming languages and file types.
📈 Enhanced Reporting: Provide organizers with comprehensive dashboards and visualizations.

Conclusion
This AI-powered application is designed to address challenges faced by communities and organizations hosting coding competitions and hackathons. By combining Firebase 🔐, Gemini 1.5 Flash API 🤖, and Google Cloud Run ☁️, it ensures a smooth, scalable, and efficient evaluation process. Whether you're hosting a hackathon with hundreds of participants or managing team projects, this tool will save time, enhance fairness, and provide valuable insights. 🚀
Try it today and revolutionize how you evaluate code! 🎉

Want to Know more?
To learn more about Google Cloud services and to create impact for the work you do, get around to these steps right away:
Register for Code Vipassana sessions
Join the meetup group Datapreneur Social
Sign up to become Google Cloud Innovator
