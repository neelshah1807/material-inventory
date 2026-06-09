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

// 🔴 PASTE YOUR GOOGLE APPS SCRIPT WEB APP URL BETWEEN THE QUOTES BELOW:
const WEB_APP_URL = "https://script.google.com/macros/s/AKfycbzihJ-Km6XgNKBIz7J3tunOc7Bg11MnHmUC4gwo917tXQRNPCbvDHmPvA1I1e-BteYS/exec";

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

}else{

alert("Material already exists");

}
}
}

// Remove Material
function removeMaterial(){

let selected=document.getElementById("material").value;

if(selected==="Select Material"){
alert("Select Material First");
return;
}

if(confirm("Delete "+selected+" ?")){

materials=materials.filter(
m=>m!==selected
);

localStorage.setItem(
"materials",
JSON.stringify(materials)
);

loadDropdowns();

}
}

// Save Entry (STORES LOCALLY AND SENDS TO MASTER GOOGLE SHEET)
function addEntry(){

let tbody=document.querySelector("#register tbody");

let sr=tbody.rows.length+1;

// Collect data values
let entryData = {
    date: document.getElementById("date").value,
    challan: document.getElementById("challan").value,
    lorry: document.getElementById("lorry").value,
    material: document.getElementById("material").value,
    party: document.getElementById("party").value,
    bill: document.getElementById("bill").value,
    remark: document.getElementById("remark").value,
    measurement: document.getElementById("measurement").value,
    qty: document.getElementById("qty").value,
    other: document.getElementById("other").value
};

let row=tbody.insertRow();

row.insertCell(0).innerHTML=sr;
row.insertCell(1).innerHTML=entryData.date;
row.insertCell(2).innerHTML=entryData.challan;
row.insertCell(3).innerHTML=entryData.lorry;
row.insertCell(4).innerHTML=entryData.material;
row.insertCell(5).innerHTML=entryData.party;
row.insertCell(6).innerHTML=entryData.bill;
row.insertCell(7).innerHTML=entryData.remark;
row.insertCell(8).innerHTML=entryData.measurement;
row.insertCell(9).innerHTML=entryData.qty;
row.insertCell(10).innerHTML=entryData.other;

// Send data to master Google Sheet seamlessly via text/plain blob to bypass CORS
if (WEB_APP_URL && WEB_APP_URL !== "https://script.google.com/macros/s/AKfycbxAfuhGixCrgQJOKPiwd5JyIw2IKS6pH-rBOCqODpF6JOMPkMznEgsrhSjcyl6gDS87/exec") {
    fetch(WEB_APP_URL, {
        method: "POST",
        mode: "no-cors", 
        headers: {
            "Content-Type": "text/plain" 
        },
        body: JSON.stringify(entryData)
    })
    .then(() => console.log("Data packet routed to Master Sheet."))
    .catch(err => console.error("Error sending data to Master Sheet: ", err));
}

// Clear Form
document.getElementById("date").value="";
document.getElementById("challan").value="";
document.getElementById("lorry").value="";
document.getElementById("bill").value="";
document.getElementById("remark").value="";
document.getElementById("measurement").value="";
document.getElementById("qty").value="";
document.getElementById("other").value="";

}

// Export Excel
function exportTableToExcel(){

let table=document.getElementById("register");

let html=table.outerHTML;

let url='data:application/vnd.ms-excel,'+
encodeURIComponent(html);

let link=document.createElement("a");

link.href=url;

link.download="Material_Register.xls";

link.click();

}

// Load Only Party & Material Masters
window.onload=function(){

loadDropdowns();

};

</script>

</body>
</html>
