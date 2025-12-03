1(i) Load from json
(index.html)
<!DOCTYPE html>
<html>
<body>
<button onclick="load()">Load JSON</button>
<div id="out"></div>
<script>
async function load() {
  let data = await (await fetch("s1.json")).json();
  let arr = data.student;
  let keys = Object.keys(arr[0]);
  let table = "<table border='1'><tr>" +
              keys.map(k => "<th>" + k + "</th>").join("") +
              "</tr>";
  arr.forEach(obj => {
    table += "<tr>" + keys.map(k => "<td>" + obj[k] + "</td>").join("") + "</tr>";
  });
  table += "</table>";
  document.getElementById("out").innerHTML = table;
}
</script>
</body>
</html>

(students.json)
{
  "student": [
    { "name": "Bhavana", "age": 20, "college": "KMIT", "year": 3, "sem": 1 },
    { "name": "Ram", "age": 21, "college": "JNTU", "year": 4, "sem": 2 },
    { "name": "John", "age": 26, "college": "KMEC", "year": 1, "sem": 1 },
    { "name": "Reena", "age": 19, "college": "NGIT", "year": 3, "sem": 1 }
  ]
}




1(j) display student GPA
<!DOCTYPE html>
<html>
<body>
<h3>Enter Student Details</h3>
<form id="studentForm" onsubmit="show(event)">
  Name: <input name="name"><br><br>
  Roll No: <input name="roll"><br><br>
  Subject1 Marks: <input name="m1"><br><br>
  Subject2 Marks: <input name="m2"><br><br>
  Subject3 Marks: <input name="m3"><br><br>
  <button type="submit">Submit</button>
</form>
<div id="data"></div>
<script>
function show(event) {
  event.preventDefault(); // Prevent page reload
  // Access form
  const form = document.getElementById("studentForm");
  // Get values from form fields
  let name = form.name.value;
  let roll = form.roll.value;
  let m1 = Number(form.m1.value);
  let m2 = Number(form.m2.value);
  let m3 = Number(form.m3.value);
  // Calculations
  let total = m1 + m2 + m3;
  let gpa = (total / 30).toFixed(2); // Assuming 100 marks → scale to 10
  // Output table
  let table = `
    <h3>Student Marks Sheet</h3>
    <table border='1'>
      <tr><td>Name</td><td>${name}</td></tr>
      <tr><td>Roll No</td><td>${roll}</td></tr>
      <tr><td>Subject 1</td><td>${m1}</td></tr>
      <tr><td>Subject 2</td><td>${m2}</td></tr>
      <tr><td>Subject 3</td><td>${m3}</td></tr>
      <tr><td>GPA</td><td>${gpa}</td></tr>
    </table>
  `;
  document.getElementById("data").innerHTML = table;
}
</script>
</body>
</html>



2)AJAX Weather details
(ajax.html)
<!DOCTYPE html>
<html>
<body>
<h3>Select Location</h3>
<select id="loc" onchange="loadWeather()">
  <option value="">--Select--</option>
  <option value="hyd">Hyderabad</option>
  <option value="mumbai">Mumbai</option>
  <option value="delhi">Delhi</option>
</select>
<div id="out"></div>
<script>
function loadWeather() {
  let city = document.getElementById("loc").value;
  if (!city) return;
  let xhr = new XMLHttpRequest();
  xhr.open("GET", "weather.json", true);   // LOCAL JSON FILE
  xhr.onload = function () {
    if (xhr.status == 200 || xhr.status == 0) { // accept 0 for local file:// loads
      let data = JSON.parse(xhr.responseText);
      console.log(xhr)
      console.log(data)
      let info = data[city];   // city object
      let out = `
        <h3>Weather Report</h3>
        <table border="1">
          <tr><td>City</td><td>${info.name}</td></tr>
          <tr><td>Temperature</td><td>${info.temp}</td></tr>
          <tr><td>Humidity</td><td>${info.humidity}</td></tr>
          <tr><td>Condition</td><td>${info.condition}</td></tr>
        </table>
      `;
      document.getElementById("out").innerHTML = out;
    }
  };
  xhr.send();
}
</script>
</body>
</html>

(weather.json)
{
  "hyd": {
    "name": "Hyderabad",
    "temp": "32°C",
    "humidity": "55%",
    "condition": "Sunny"
  },
  "mumbai": {
    "name": "Mumbai",
    "temp": "29°C",
    "humidity": "70%",
    "condition": "Cloudy"
  },
  "delhi": {
    "name": "Delhi",
    "temp": "34°C",
    "humidity": "48%",
    "condition": "Hot"
  }
}



3) Node Js program that accepts port from the user(server.js)
var http = require('http');
var server = http.createServer(function (req, res){
    if (req.url == '/'){ //check the URL of the current request
        res.writeHead(200, { 'Content-Type': 'text/html' });
        //set response content
        res.write('<html><body><p>This is home Page.</p></body></html>');
        res.end();
    }
    else if (req.url == "/student") {
        res.writeHead(200, { 'Content-Type': 'text/html' });
        res.write('<html><body><p>This is student Page.</p></body></html>');
        res.end();
    }
    else if (req.url == "/admin") {
        res.writeHead(200, { 'Content-Type': 'text/html' });
        res.write('<html><body><p>This is admin Page.</p></body></html>');
        res.end();
    }
    else res.end('Invalid Request!');
});
server.listen(8000);
console.log('Node.js web server at port 8000 is running..')



7) create a form, based on roll number provided, the student details should be fetched(using ExpressJS)
<!DOCTYPE html>
<html>
<head>
    <title>Student Details</title>
</head>
<body>
    <h2 style="text-align:center;">Student Details</h2>
    <div style="margin-left: 30px;">
        <input type="text" id="roll" placeholder="20bd1a0501" style="width:250px;">
        <br><br>
        <button onclick="fetchStudent()">Fetch</button>
    </div>
    <br><br>
    <table id="result" border="1" style="margin-left: 30px; border-collapse: collapse;">
        <!-- Table will load here -->
    </table>
<script>
function fetchStudent() {
    let roll = document.getElementById("roll").value.trim().toLowerCase();
    // sample JSON (replace with AJAX if needed)
    let students = {
        "20bd1a0501": {
            "slno": 1,
            "name": "john",
            "roll": "20bd1a0501",
            "course": "b.tech",
            "branch": "cse"
        }
    };
    let s = students[roll];
    if (!s) {
        document.getElementById("result").innerHTML = "<tr><td>No student found.</td></tr>";
        return;
    }
    // build output table
    document.getElementById("result").innerHTML =
        `<tr>
            <th>Sl.No</th>
            <th>Name</th>
            <th>Roll No</th>
            <th>Course</th>
            <th>Branch</th>
        </tr>
        <tr>
            <td>${s.slno}</td>
            <td>${s.name}</td>
            <td>${s.roll}</td>
            <td>${s.course}</td>
            <td>${s.branch}</td>
        </tr>`;
}
</script>
</body>
</html>


8)
(index.html)
<!DOCTYPE html>
<html>
<body>
<h2>Student Database Client</h2>
<h3>Register Student</h3>
<form onsubmit="register(event)">
  Name: <input id="r_name"><br>
  ID: <input id="r_id"><br>
  Course: <input id="r_course"><br>
  Branch: <input id="r_branch"><br><br>
  <button type="submit">Register</button>
</form>
<h3>Fetch Student</h3>
<form onsubmit="fetchData(event)">
  Roll No: <input id="f_id"><br><br>
  <button type="submit">Fetch</button>
</form>
<h3>Update Student</h3>
<form onsubmit="updateData(event)">
  Existing Name: <input id="u_old_name"><br>
  New Name: <input id="u_name"><br>
  New ID: <input id="u_id"><br>
  New Course: <input id="u_course"><br>
  New Branch: <input id="u_branch"><br><br>
  <button type="submit">Update</button>
</form>
<h3>Delete Student</h3>
<form onsubmit="deleteData(event)">
  Roll No: <input id="d_id"><br><br>
  <button type="submit">Delete</button>
</form>
<div id="output"></div>
<script>
async function register(e){
  e.preventDefault();
  let body = {
    name: r_name.value,
    id: r_id.value,
    course: r_course.value,
    branch: r_branch.value
  };
  let res = await fetch("http://localhost:3000/register", {
    method: "POST",
    headers: {"Content-Type":"application/json"},
    body: JSON.stringify(body)
  });
  output.innerHTML = await res.text();
}
async function fetchData(e){
  e.preventDefault();
  let res = await fetch("http://localhost:3000/fetch", {
    method: "POST",
    headers: {"Content-Type":"application/json"},
    body: JSON.stringify({id: f_id.value})
  });
  output.innerHTML = <pre>+ ${JSON.stringify(await res.json())}+<pre>;
}
async function updateData(e){
  e.preventDefault();
  let body = {
    oldName: u_old_name.value,
    name: u_name.value,
    id: u_id.value,
    course: u_course.value,
    branch: u_branch.value
  };
  let res = await fetch("http://localhost:3000/update", {
    method: "PUT",
    headers: {"Content-Type":"application/json"},
    body: JSON.stringify(body)
  });
  output.innerHTML = await res.text();
}
async function deleteData(e){
  e.preventDefault();
  let res = await fetch("http://localhost:3000/delete", {
    method: "DELETE",
    headers: {"Content-Type":"application/json"},
    body: JSON.stringify({id: d_id.value})
  });
  output.innerHTML = await res.text();
}
</script>
</body>
</html>

(app.js)
const express = require("express");
const mongoose = require("mongoose");
const cors = require("cors");
const app = express();
app.use(express.json());
app.use(cors());
// Connect MongoDB
mongoose
  .connect("mongodb://localhost:27017/wtexternal")
  .then(() => console.log("MongoDB Connected"))
  .catch((err) => console.log(err));
// Schema
const studentSchema = new mongoose.Schema({
  name: String,
  id: String,
  course: String,
  branch: String,
});
const Student = mongoose.model("students", studentSchema);
// Register Student
app.post("/register", async (req, res) => {
  try {
    await Student.create(req.body);
    res.send("Student Registered Successfully");
  } catch (e) {
    res.send("Failed to register student");
  }
}); //C
// Fetch Student
app.post("/fetch", async (req, res) => {
  let stu = await Student.find({ id: req.body.id });
  res.json(stu);
});
// Update Student
app.put("/update", async (req, res) => {
  const { oldName, name, id, course, branch } = req.body;
  try {
    let result = await Student.findOneAndUpdate(
      { name: oldName },
      {name, id, course, branch },
        { new: true }
    );
    res.send("Student Updated Successfully");
  } catch (e) {
    res.send("Update Failed");
  }
});
// Delete Student
app.delete("/delete", async (req, res) => {
  try {
    await Student.deleteOne({ id: req.body.id });
    res.send("Student Deleted Successfully");
  } catch (e) {
    res.send("Delete Failed");
  }
});
// Start server
app.listen(3000, () => console.log("Server running on port 3000"));
