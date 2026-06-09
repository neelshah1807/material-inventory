<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Material Register</title>

<style>
body{
    font-family:Arial, sans-serif;
    margin:20px;
}

h2{
    text-align:center;
}

.form-group{
    margin-bottom:10px;
}

label{
    display:inline-block;
    width:150px;
    font-weight:bold;
}

input,select{
    width:300px;
    padding:5px;
}

button{
    padding:6px 12px;
    margin-left:5px;
    cursor:pointer;
}

table{
    width:100%;
    border-collapse:collapse;
    margin-top:20px;
}

th,td{
    border:1px solid #000;
    padding:5px;
    text-align:center;
}

th{
    background:#d9d9d9;
}
</style>
</head>

<body>

<h2>MATERIAL REGISTER</h2>

<div class="form-group">
<label>Date</label>
<input type="date" id="date">
</div>

<div class="form-group">
<label>Challan No</label>
<input type="text" id="challan">
</div>

<div class="form-group">
<label>Lorry No</label>
<input type="text" id="lorry">
</div>

<div class="form-group">
<label>Material</label>
<select id="material"></select>
<button type="button" onclick="addMaterial()">Add Material</button>
<button type="button" onclick="removeMaterial()">Remove Material</button>
</div>

<div class="form-group">
<label>Party Name</label>
<select id="party"></select>
<button type="button" onclick="addParty()">Add Party</button>
<button type="button" onclick="removeParty()">Remove Party</button>
</div>

<div class="form-group">
<label>Bill No</label>
<input type="text" id="bill">
</div>

<div class="form-group">
<label>Remark</label>
<input type="text" id="remark">
</div>

<div class="form-group">
<label>Measurement</label>
<input type="text" id="measurement">
</div>

<div class="form-group">
<label>Qty</label>
<input type="number" id="qty">
</div>

<div class="form-group">
<label>Other Remarks</label>
<input type="text" id="other">
</div>

<button onclick="addEntry()">Save Entry</button>
<button onclick="uploadLastEntryToDatabase()" style="background-color: #4CAF50; color: white; font-weight: bold; border: 1px solid #4CAF50;">Upload to Database</button>
<button onclick="exportTableToExcel()">Export to Excel</button>

<table id="register">
<thead>
<tr>
<th>SR NO</th>
<th>DATE</th>
<th>CHALLAN NO</th>
<th>LORRY NO</th>
<th>MATERIAL</th>
<th>PARTY NAME</th>
<th>BILL NO</th>
<th>REMARK</th>
<th>MEASUREMENT</th>
<th>QTY</th>
<th>OTHER REMARKS</th>
</tr>
</thead>
<tbody>
</tbody>
</table>

<script>

// 🔴 PASTE YOUR RE-DEPLOYED WEB APP URL HERE:
const WEB_APP_URL = "https://script.google.com/macros/s/AKfycbzihJ-Km6XgNKBIz7J3tunOc7Bg11MnHmUC4gwo917tXQRNPCbvDHmPvA1I1e-BteYS/exec";

// Global object variable holding the last generated entry data package
let lastSavedEntryData = null;

// Party Master
let parties = JSON.parse(localStorage.getItem("parties")) || [
"Select Party"
];

// Material Master
let materials = JSON.parse(localStorage.getItem("materials")) || [
"Select Material"
];

// Load Dropdowns
function loadDropdowns(){

let party=document.getElementById("party");
let material=document.getElementById("material");

party.innerHTML="";
material.innerHTML="";

parties.forEach(function(p){

let opt=document.createElement("option");
opt.text=p;
opt.value=p;
party.add(opt);

});

materials.forEach(function(m){

let opt=document.createElement("option");
opt.text=m;
opt.value=m;
material.add(opt);

});

}

// Add Party
function addParty(){

let name=prompt("Enter Party Name");

if(name){

name=name.trim();

if(!parties.includes(name)){

parties.push(name);

localStorage.setItem(
"parties",
JSON.stringify(parties)
);

loadDropdowns();

}else{

alert("Party already exists");

}
}
}

// Remove Party
function removeParty(){

let selected=document.getElementById("party").value;

if(selected==="Select Party"){
alert("Select Party First");
return;
}

if(confirm("Delete "+selected+" ?")){

parties=parties.filter(
p=>p!==selected
);

localStorage.setItem(
"parties",
JSON.stringify(parties)
);

loadDropdowns();

}
}

// Add Material
function addMaterial(){

let name=prompt("Enter Material Name");

if(name){

name=name.trim();

if(!materials.includes(name)){

materials.push(name);

localStorage.setItem(
"materials",
JSON.stringify(materials)
);

loadDropdowns();

}
