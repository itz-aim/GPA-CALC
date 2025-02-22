<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>GPA Calculator</title>
  <style>
    body {
      font-family: 'Roboto' , sans-serif;
      margin: 0;
      padding: 0;
      background-image: url('https://img.freepik.com/free-vector/hand-drawn-soft-earth-tones-pattern_23-2151158457.jpg?t=st=1737297778~exp=1737301378~hmac=7bb14b19427164a7103d91086556ef5ca6d16d797acbc97bb28b2194a48b25ce&w=740'); /* Replace with your image file name or URL */
      background-size: cover;
      background-position: center;
    }
    .container {
      max-width: 600px;
      margin: 20px auto;
      background: rgba(255, 255, 255, 0.5); /* White background with some transparency */
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    }
    h1 {
      text-align: center;
      color: #333;
      font-size: 24px; 
      font-family: 'Montserrat', sans-serif; /* Apply Montserrat font */
    }
    label {
      font-weight: bold;
      margin-top: 10px;
      font-size: 14px;
      font-family: 'Roboto', sans-serif; /* Apply Roboto font */
    }
    input, select {
      width: 100%;
      padding: 10px;
      margin: 10px 0;
      border: 1px solid #ccc;
      border-radius: 4px;
      font-family: 'Roboto', sans-serif; /* Apply Roboto font */
    }
    .button-group {
      display: flex;
      justify-content: space-between;
      margin-top: 10px;
      flex-wrap: wrap;
    }
    button {
      padding: 10px;
      flex: 1;
      margin: 0 5px;
      color: #fff;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      font-size: 16px;
      font-family: 'Montserrat', sans-serif; /* Apply Montserrat font */
    }
    button:hover {
      opacity: 0.9;
    }
    button:first-child {
      background-color: #28a745; /* Green for Add Course */
    }
    button:nth-child(2) {
      background-color: #20283E; /* Blue for Calculate GPA */
    }
    button:last-child {
      background-color: #dc3545; /* Red for Reset */
    }
    .results, .course-list {
      margin-top: 20px;
      padding: 10px;
      background: #f9f9f9;
      border: 1px solid #ddd;
      border-radius: 4px;
    }
    .results p, .course-list p {
      margin: 5px 0;
    }

    /* Media Queries for responsive design */
    @media (max-width: 600px) {
      .container {
        padding: 10px;
        width: 90%;
      }
      .button-group {
        flex-direction: column;
      }
      button {
        margin: 5px 0;
         font-size: 15px; /* Adjusted font size for smaller screens */ 
         } 
         h1 { 
         font-size: 20px; /* Adjusted font size */ 
         } 
         label { 
         font-size: 15px; /* Adjusted font size */
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>GPA Calculator</h1>
    <form id="gpaForm">
      <label for="courseCode">Course Code:</label>
      <input type="text" id="courseCode" placeholder="Enter course code (e.g., BMKT2323)" required>
      <label for="credits">Credit Hours:</label>
      <input type="number" id="credits" placeholder="Enter credit hours" required>
      <label for="grade">Grade:</label>
      <select id="grade" required>
        <option value="" disabled selected>Select grade</option>
        <option value="4.0">A  (4.0)</option>
        <option value="3.7">A- (3.7)</option>
        <option value="3.3">B+ (3.3)</option>
        <option value="3.0">B  (3.0)</option>
        <option value="2.7">B- (2.7)</option>
        <option value="2.3">C+ (2.3)</option>
        <option value="2.0">C  (2.0)</option>
        <option value="1.7">C- (1.7)</option>
        <option value="1.0">D  (1.0)</option>
        <option value="0.0">F  (0.0)</option>
      </select>
      <div class="button-group">
        <button type="button" onclick="addCourse()">Add Course</button>
        <button type="button" onclick="calculateGPA()">Calculate GPA</button>
        <button type="button" onclick="resetForm()">Reset</button>
      </div>
    </form>
    <div id="courseList" class="course-list">
      <h2>Added Courses:</h2>
      <p>No courses added yet.</p>
    </div>
    <div id="results" class="results"></div>
  </div>

  <script>
    const courses = [];

    function addCourse() {
      const courseCode = document.getElementById("courseCode").value.trim();
      const credits = parseFloat(document.getElementById("credits").value);
      const grade = parseFloat(document.getElementById("grade").value);

      if (!courseCode || isNaN(credits) || credits <= 0 || isNaN(grade)) {
        alert("Please enter valid course details.");
        return;
      }

      courses.push({ courseCode, credits, grade });
      updateCourseList();
      alert(`Course "${courseCode}" added successfully!`);
      document.getElementById("gpaForm").reset();
    }

    function updateCourseList() {
      const courseListDiv = document.getElementById("courseList");
      if (courses.length === 0) {
        courseListDiv.innerHTML = `<h2>Added Courses:</h2><p>No courses added yet.</p>`;
        return;
      }

      courseListDiv.innerHTML = `<h2>Added Courses:</h2>`;
      courses.forEach(({ courseCode, credits, grade }, index) => {
        courseListDiv.innerHTML += `<p>${index + 1}. <strong>${courseCode}</strong>: Credits = ${credits}, Grade = ${grade}</p>`;
      });
    }

    function calculateGPA() {
      if (courses.length === 0) {
        alert("No courses added yet!");
        return;
      }

      let totalWeightedPoints = 0;
      let totalCredits = 0;

      courses.forEach(({ credits, grade }) => {
        totalWeightedPoints += credits * grade;
        totalCredits += credits;
      });

      const gpa = totalWeightedPoints / totalCredits;
      displayResults(gpa);
    }

    function displayResults(gpa) {
      const resultsDiv = document.getElementById("results");
      resultsDiv.innerHTML = "<h2>Results:</h2>";
      resultsDiv.innerHTML += `<p><strong>GPA:</strong> ${gpa.toFixed(2)}</p>`;
    }

    function resetForm() {
      document.getElementById("gpaForm").reset();
      courses.length = 0;
      updateCourseList();
      const resultsDiv = document.getElementById("results");
      resultsDiv.innerHTML = "";
    }
  </script>
</body>
</html>
