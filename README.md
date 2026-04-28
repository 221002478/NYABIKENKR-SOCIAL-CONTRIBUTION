
<html>
<head>
    <title>NYABIKENKE SOCIAL CONTRIBUTION </title>
<h3>NSC - Nyabikenke Social Contribution</h3>
<p>MURAKAZA NEZA KURUBUGA AHO GUTANGA UMUSANZU ARINSHINGANO ZACU TWESE.</p>
</head>
<body>

<!-- ===================== LOGIN PAGE ===================== -->
<div id="loginPage">
    <h2>Login</h2>

    <input type="text" id="loginUsername" placeholder="Username"><br><br>
    <input type="password" id="loginPassword" placeholder="Password"><br><br>

    <button onclick="login()">Login</button>

    <p>
        <a href="#" onclick="showPage('registerPage')">Create Account</a> |
        <a href="#" onclick="showPage('forgotPage')">Forgot Password?</a>
    </p>
</div>

<!-- ===================== REGISTER PAGE ===================== -->
<div id="registerPage" style="display:none;">
    <h2>Create Account</h2>

    <input type="text" id="regUsername" placeholder="Username"><br><br>
    <input type="password" id="regPassword" placeholder="Password"><br><br>

    <button onclick="register()">Register</button>

    <p><a href="#" onclick="showPage('loginPage')">Back to Login</a></p>
</div>

<!-- ===================== FORGOT PASSWORD ===================== -->
<div id="forgotPage" style="display:none;">
    <h2>Reset Password</h2>

    <input type="text" id="forgotUsername" placeholder="Username"><br><br>
    <input type="password" id="newPassword" placeholder="New Password"><br><br>

    <button onclick="resetPassword()">Reset Password</button>

    <p><a href="#" onclick="showPage('loginPage')">Back to Login</a></p>
</div>

<!-- ===================== DASHBOARD ===================== -->
<div id="dashboardPage" style="display:none;">
    <h2>Dashboard</h2>

    <p id="welcome"></p>

    <button onclick="logout()">Logout</button>
</div>

<script>
// ===================== PAGE NAVIGATION =====================
// Function to switch between pages
function showPage(pageId) {
    document.getElementById("loginPage").style.display = "none";
    document.getElementById("registerPage").style.display = "none";
    document.getElementById("forgotPage").style.display = "none";
    document.getElementById("dashboardPage").style.display = "none";

    document.getElementById(pageId).style.display = "block";
}

// ===================== REGISTER FUNCTION =====================
function register() {
    let username = document.getElementById("regUsername").value;
    let password = document.getElementById("regPassword").value;

    let users = JSON.parse(localStorage.getItem("users")) || [];

    // Check if user already exists
    let exists = users.find(u => u.username === username);

    if (exists) {
        alert("User already exists");
        return;
    }

    // Add new user
    users.push({ username, password });

    localStorage.setItem("users", JSON.stringify(users));

    alert("Account created successfully!");

    showPage("loginPage");
}

// ===================== LOGIN FUNCTION =====================
function login() {
    let username = document.getElementById("loginUsername").value;
    let password = document.getElementById("loginPassword").value;

    let users = JSON.parse(localStorage.getItem("users")) || [];

    let user = users.find(u => u.username === username && u.password === password);

    if (user) {
        // Save current user session
        localStorage.setItem("currentUser", username);

        loadDashboard();
    } else {
        alert("Invalid username or password");
    }
}

// ===================== RESET PASSWORD =====================
function resetPassword() {
    let username = document.getElementById("forgotUsername").value;
    let newPassword = document.getElementById("newPassword").value;

    let users = JSON.parse(localStorage.getItem("users")) || [];

    let user = users.find(u => u.username === username);

    if (!user) {
        alert("User not found");
        return;
    }

    // Update password
    user.password = newPassword;

    localStorage.setItem("users", JSON.stringify(users));

    alert("Password reset successful!");

    showPage("loginPage");
}

// ===================== LOAD DASHBOARD =====================
function loadDashboard() {
    let currentUser = localStorage.getItem("currentUser");

    if (!currentUser) {
        showPage("loginPage");
        return;
    }

    document.getElementById("welcome").textContent = "Welcome, " + currentUser;

    showPage("dashboardPage");
}

<!DOCTYPE html>
<html>
<head>
    <title>NSC Dashboard</title>

    <style>
        /* ===== BASIC LAYOUT ===== */
        body {
            font-family: Arial;
            margin: 0;
            display: flex;
        }

        /* ===== SIDEBAR MENU ===== */
        .sidebar {
            width: 220px;
            background: #1e3a8a;
            color: white;
            height: 100vh;
            padding: 15px;
        }

        .sidebar h2 {
            text-align: center;
        }

        .menu button {
            width: 100%;
            padding: 10px;
            margin-top: 10px;
            cursor: pointer;
        }

        /* ===== MAIN CONTENT ===== */
        .main {
            flex: 1;
            padding: 20px;
        }

        .card {
            padding: 15px;
            margin-top: 10px;
            background: #f3f4f6;
        }

    </style>
</head>

<body>

<!-- ================= SIDEBAR ================= -->
<div class="sidebar">
    <h2>NSC</h2>

    <p id="user"></p>

    <div class="menu">
        <button onclick="showSection('home')">Dashboard</button>
        <button onclick="showSection('finance')">Finance Tracker</button>
        <button onclick="showSection('accounts')">Accounts</button>
        <button onclick="showSection('reports')">Monthly Reports</button>
        <button onclick="logout()">Logout</button>
    </div>
</div>

<!-- ================= MAIN CONTENT ================= -->
<div class="main">

    <!-- HOME -->
    <div id="home">
        <h2>Welcome to NSC Dashboard</h2>
        <div class="card">
            <p>Track your contributions, savings, and financial activities.</p>
        </div>
    </div>

    <!-- FINANCE -->
    <div id="finance" style="display:none;">
        <h2>Finance Tracker</h2>

        <input type="text" id="desc" placeholder="Description"><br><br>
        <input type="number" id="amount" placeholder="Amount"><br><br>

        <button onclick="addFinance()">Add</button>

        <ul id="financeList"></ul>
    </div>

    <!-- ACCOUNTS -->
    <div id="accounts" style="display:none;">
        <h2>Accounts</h2>

        <div class="card">Cash</div>
        <div class="card">Bank</div>
        <div class="card">Mobile Money</div>
    </div>

    <!-- REPORTS -->
    <div id="reports" style="display:none;">
        <h2>Monthly Reports</h2>

        <div class="card">
            <p>Coming soon: monthly summaries and analytics</p>
        </div>
    </div>

</div>

<script>

// ================= CHECK LOGIN =================
let currentUser = localStorage.getItem("NSC_currentUser");

if (!currentUser) {
    window.location.href = "index.html";
}

document.getElementById("user").innerText = "User: " + currentUser;

// ================= NAVIGATION =================
function showSection(section) {
    document.getElementById("home").style.display = "none";
    document.getElementById("finance").style.display = "none";
    document.getElementById("accounts").style.display = "none";
    document.getElementById("reports").style.display = "none";

    document.getElementById(section).style.display = "block";
}

// ================= FINANCE (SIMPLE STORAGE) =================
let data = JSON.parse(localStorage.getItem("NSC_finance")) || [];

function addFinance() {
    let desc = document.getElementById("desc").value;
    let amount = document.getElementById("amount").value;

    data.push({
        user: currentUser,
        desc: desc,
        amount: amount
    });

    localStorage.setItem("NSC_finance", JSON.stringify(data));

    displayFinance();
}

function displayFinance() {
    let list = document.getElementById("financeList");
    list.innerHTML = "";

    data.filter(d => d.user === currentUser).forEach(item => {
        let li = document.createElement("li");
        li.innerText = item.desc + " : " + item.amount;
        list.appendChild(li);
    });
}

// ================= LOGOUT =================
function logout() {
    localStorage.removeItem("NSC_currentUser");
    window.location.href = "index.html";
}

// load finance on start
displayFinance();

</script>

</body>
</html>

// ===================== AUTO LOGIN =====================
// When page loads, check if user is already logged in
window.onload = function() {
    loadDashboard();
};
</script>

</body>
</html>
