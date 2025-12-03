1(i) Load from json (index.html)

Load JSON
<script> async function load() { let data = await (await fetch("s1.json")).json(); let arr = data.student; let keys = Object.keys(arr[0]); let table = "" + keys.map(k => "").join("") + ""; arr.forEach(obj => { table += "" + keys.map(k => "").join("") + ""; }); table += "
" + k + "
" + obj[k] + "
"; document.getElementById("out").innerHTML = table; } </script>
(students.json) { "student": [ { "name": "Bhavana", "age": 20, "college": "KMIT", "year": 3, "sem": 1 }, { "name": "Ram", "age": 21, "college": "JNTU", "year": 4, "sem": 2 }, { "name": "John", "age": 26, "college": "KMEC", "year": 1, "sem": 1 }, { "name": "Reena", "age": 19, "college": "NGIT", "year": 3, "sem": 1 } ] }

1(j) display student GPA

Enter Student Details
Name:

Roll No:

Subject1 Marks:

Subject2 Marks:

Subject3 Marks:

Submit
<script> function show(event) { event.preventDefault(); // Prevent page reload // Access form const form = document.getElementById("studentForm"); // Get values from form fields let name = form.name.value; let roll = form.roll.value; let m1 = Number(form.m1.value); let m2 = Number(form.m2.value); let m3 = Number(form.m3.value); // Calculations let total = m1 + m2 + m3; let gpa = (total / 30).toFixed(2); // Assuming 100 marks → scale to 10 // Output table let table = `
Student Marks Sheet
Name	${name}
Roll No	${roll}
Subject 1	${m1}
Subject 2	${m2}
Subject 3	${m3}
GPA	${gpa}
`; document.getElementById("data").innerHTML = table; } </script>
2)AJAX Weather details (ajax.html)

Select Location
--Select-- Hyderabad Mumbai Delhi
<script> function loadWeather() { let city = document.getElementById("loc").value; if (!city) return; let xhr = new XMLHttpRequest(); xhr.open("GET", "weather.json", true); // LOCAL JSON FILE xhr.onload = function () { if (xhr.status == 200 || xhr.status == 0) { // accept 0 for local file:// loads let data = JSON.parse(xhr.responseText); console.log(xhr) console.log(data) let info = data[city]; // city object let out = `
Weather Report
City	${info.name}
Temperature	${info.temp}
Humidity	${info.humidity}
Condition	${info.condition}
`; document.getElementById("out").innerHTML = out; } }; xhr.send(); } </script>
(weather.json) { "hyd": { "name": "Hyderabad", "temp": "32°C", "humidity": "55%", "condition": "Sunny" }, "mumbai": { "name": "Mumbai", "temp": "29°C", "humidity": "70%", "condition": "Cloudy" }, "delhi": { "name": "Delhi", "temp": "34°C", "humidity": "48%", "condition": "Hot" } }

Node Js program that accepts port from the user(server.js) var http = require('http'); var server = http.createServer(function (req, res){ if (req.url == '/'){ //check the URL of the current request res.writeHead(200, { 'Content-Type': 'text/html' }); //set response content res.write('

This is home Page.

'); res.end(); } else if (req.url == "/student") { res.writeHead(200, { 'Content-Type': 'text/html' }); res.write('
This is student Page.

'); res.end(); } else if (req.url == "/admin") { res.writeHead(200, { 'Content-Type': 'text/html' }); res.write('
This is admin Page.

'); res.end(); } else res.end('Invalid Request!'); }); server.listen(8000); console.log('Node.js web server at port 8000 is running..')
create a form, based on roll number provided, the student details should be fetched(using ExpressJS)

<title>Student Details</title>
Student Details


Fetch


<script> function fetchStudent() { let roll = document.getElementById("roll").value.trim().toLowerCase(); // sample JSON (replace with AJAX if needed) let students = { "20bd1a0501": { "slno": 1, "name": "john", "roll": "20bd1a0501", "course": "b.tech", "branch": "cse" } }; let s = students[roll]; if (!s) { document.getElementById("result").innerHTML = "No student found."; return; } // build output table document.getElementById("result").innerHTML = ` Sl.No Name Roll No Course Branch ${s.slno} ${s.name} ${s.roll} ${s.course} ${s.branch} `; } </script>
(index.html)

Student Database Client
Register Student
Name:
ID:
Course:
Branch:

Register
Fetch Student
Roll No:

Fetch
Update Student
Existing Name:
New Name:
New ID:
New Course:
New Branch:

Update
Delete Student
Roll No:

Delete
<script> async function register(e){ e.preventDefault(); let body = { name: r_name.value, id: r_id.value, course: r_course.value, branch: r_branch.value }; let res = await fetch("http://localhost:3000/register", { method: "POST", headers: {"Content-Type":"application/json"}, body: JSON.stringify(body) }); output.innerHTML = await res.text(); } async function fetchData(e){ e.preventDefault(); let res = await fetch("http://localhost:3000/fetch", { method: "POST", headers: {"Content-Type":"application/json"}, body: JSON.stringify({id: f_id.value}) }); output.innerHTML = `
+ ${JSON.stringify(await res.json())}+
`;
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
