<!DOCTYPE html>
<html>
<head>
    <title>NSC - Nyabikenke Social Contribution</title>

    <style>
        body { font-family: Arial; margin:0; }

        .container { padding:20px; }

        input { padding:8px; margin:5px; width:200px; }

        button { padding:8px 12px; margin:5px; cursor:pointer; }

        .hidden { display:none; }

        /* Sidebar */
        .sidebar {
            width:200px;
            background:#1e3a8a;
            color:white;
            height:100vh;
            float:left;
            padding:10px;
        }

        .main {
            margin-left:210px;
            padding:20px;
        }

        .card {
            background:#f3f4f6;
            padding:10px;
            margin:10px 0;
        }
    </style>
</head>

<body>

<!-- ================= LOGIN / REGISTER ================= -->
<div id="auth" class="container">

    <h2>NSC System</h2>

    <h3>Login</h3>
    <input id="loginUser" placeholder="Username"><br>
    <input id="loginPass" type="password" placeholder="Password"><br>
    <button onclick="login()">Login</button> <h3>Create Account</h3>
    <input id="regUser" placeholder="Username"><br>
    <input id="regPass" type="password" placeholder="Password"><br>
    <button onclick="register()">Register</button>
</div>

<!-- ================= DASHBOARD ================= -->
<div id="dashboard" class="hidden">

    <!-- Sidebar -->
    <div class="sidebar">
        <h3>NSC</h3>
        <p id="currentUser"></p>

        <button onclick="show('home')">Home</button>
        <button onclick="show('finance')">Finance</button>
        <button onclick="show('payments')">Payments</button>
        <button onclick="logout()">Logout</button>
    </div>

    <!-- Main -->
    <div class="main">

        <!-- HOME -->
        <div id="home">
            <h2>Welcome</h2>
            <div class="card">Manage your monthly contributions</div>
        </div>

        <!-- FINANCE -->
        <div id="finance" class="hidden">
            <h2>Finance Tracker</h2>

            <input id="desc" placeholder="Description">
            <input id="amount" type="number" placeholder="Amount">
            <button onclick="addFinance()">Add</button>
 <h3>Create Account</h3>
    <input id="regUser" placeholder="Username"><br>
    <input id="regPass" type="password" placeholder="Password"><br>
    <button onclick="register()">Register</button>

            <ul id="financeList"></ul>
        </div>

        <!-- PAYMENTS -->
        <div id="payments" class="hidden">
            <h2>Monthly Contributions</h2>
            <div id="months"></div>
        </div>

    </div>
</div>

<script>

// ================= USERS STORAGE =================
let users = JSON.parse(localStorage.getItem("users")) || [];

// ================= REGISTER =================
function register(){
    let u = regUser.value;
    let p = regPass.value;

    users.push({username:u, password:p});
    localStorage.setItem("users", JSON.stringify(users));

    alert("Account created!");
}

// ================= LOGIN =================
function login(){
    let u = loginUser.value;
    let p = loginPass.value;

    let found = users.find(x => x.username===u && x.password===p);

    if(found){
        localStorage.setItem("currentUser", u);
        loadDashboard();
    } else {
        alert("Invalid login");
    }
}

// ================= LOAD DASHBOARD =================
function loadDashboard(){
    auth.style.display = "none";
    dashboard.style.display = "block";

    let user = localStorage.getItem("currentUser");
    currentUser.innerText = user;

    loadFinance();
    loadPayments();
}

// ================= LOGOUT =================
function logout(){
    localStorage.removeItem("currentUser");
    location.reload();
}

// ================= NAVIGATION =================
function show(section){
    home.classList.add("hidden");
    finance.classList.add("hidden");
    payments.classList.add("hidden");

    document.getElementById(section).classList.remove("hidden");
}

// ================= FINANCE =================
let financeData = JSON.parse(localStorage.getItem("finance")) || [];

function addFinance(){
    let user = localStorage.getItem("currentUser");

    financeData.push({
        user:user,
        desc:desc.value,
        amount:amount.value
    });

    localStorage.setItem("finance", JSON.stringify(financeData));
    loadFinance();
}

function loadFinance(){
    let user = localStorage.getItem("currentUser");
    financeList.innerHTML = "";

    financeData.filter(x=>x.user===user).forEach(item=>{
        let li = document.createElement("li");
        li.innerText = item.desc + " : " + item.amount;
        financeList.appendChild(li);
    });
}

// ================= PAYMENTS =================
const monthsList = [
"January","February","March","April","May","June",
"July","August","September","October","November","December"
];

let payments = JSON.parse(localStorage.getItem("payments")) || [];

function loadPayments(){
    let user = localStorage.getItem("currentUser");
    months.innerHTML = "";

    monthsList.forEach(m=>{
        let paid = payments.find(p=>p.user===user && p.month===m);

        let div = document.createElement("div");
        div.className = "card";

        div.innerHTML = `
            <b>${m}</b><br>
            Status: <span style="color:${paid?'green':'red'}">
            ${paid?'PAID':'PENDING'}
            </span><br>
            ${!paid ? `<button onclick="pay('${m}')">Pay</button>`:''}
        `;

        months.appendChild(div);
    });
}

function pay(month){
    alert("Dial *182*8*1*355613# to pay");

    let user = localStorage.getItem("currentUser");

    payments.push({
        user:user,
        month:month
    });

    localStorage.setItem("payments", JSON.stringify(payments));
    loadPayments();
}

// ================= AUTO LOGIN =================
if(localStorage.getItem("currentUser")){
    loadDashboard();
}

</script>

</body>
</html>
