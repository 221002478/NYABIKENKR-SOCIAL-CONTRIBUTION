
<html>
<head>
    <title>NSC - Nyabikenke Social Contribution</title>

    <style>
        /* ===================== BASIC DESIGN ===================== */
        body {
            font-family: Arial;
            margin: 0;
        }

        /* ===================== LOGIN / FORMS ===================== */
        .container {
            width: 300px;
            margin: 80px auto;
        }

        input, button {
            width: 100%;
            padding: 10px;
            margin-top: 10px;
        }

        button {
            cursor: pointer;
        }

        /* ===================== DASHBOARD LAYOUT ===================== */
        .app {
            display: flex;
        }

        /* Sidebar menu */
        .sidebar {
            width: 220px;
            background: #1e3a8a;
            color: white;
            height: 100vh;
            padding: 15px;
        }

        .sidebar button {
            width: 100%;
            margin-top: 10px;
            padding: 10px;
        }

        /* Main content */
        .main {
            flex: 1;
            padding: 20px;
        }

        .card {
            background: #f3f4f6;
            padding: 10px;
            margin-top: 10px;
        }
    </style>
</head>

<body>

<!-- ========================================================= -->
<!-- ===================== LOGIN PAGE ======================== -->
<!-- ========================================================= -->
<div id="loginPage" class="container">

    <h2>NSC Login</h2>

    <input type="text" id="loginUser" placeholder="Username">
    <input type="password" id="loginPass" placeholder="Password">

    <button onclick="login()">Login</button>

    <p>
        <a href="#" onclick="showPage('registerPage')">Create Account</a> |
        <a href="#" onclick="showPage('forgotPage')">Forgot Password</a>
    </p>

</div>

<!-- ========================================================= -->
<!-- ===================== REGISTER PAGE ===================== -->
<!-- ========================================================= -->
<div id="registerPage" class="container" style="display:none;">

    <h2>Create Account</h2>

    <input type="text" id="regUser" placeholder="Username">
    <input type="password" id="regPass" placeholder="Password">

    <button onclick="register()">Register</button>

    <p><a href="#" onclick="showPage('loginPage')">Back to Login</a></p>

</div>

<!-- ========================================================= -->
<!-- ===================== FORGOT PASSWORD =================== -->
<!-- ========================================================= -->
<div id="forgotPage" class="container" style="display:none;">

    <h2>Reset Password</h2>

    <input type="text" id="fpUser" placeholder="Username">
    <input type="password" id="fpPass" placeholder="New Password">

    <button onclick="resetPassword()">Reset</button>

    <p><a href="#" onclick="showPage('loginPage')">Back</a></p>

</div>

<!-- ========================================================= -->
<!-- ===================== DASHBOARD ========================= -->
<!-- ========================================================= -->
<div id="dashboard" style="display:none;" class="app">

    <!-- Sidebar -->
    <div class="sidebar">
        <h2>NSC</h2>

        <p id="userInfo"></p>

        <button onclick="showSection('home')">Dashboard</button>
        <button onclick="showSection('finance')">Finance</button>
        <button onclick="showSection('accounts')">Accounts</button>
        <button onclick="showSection('reports')">Reports</button>
        <button onclick="logout()">Logout</button>
    </div>

    <!-- Main content -->
    <div class="main">

        <!-- ================= HOME ================= -->
        <div id="homeSection">
            <h2>Welcome to NSC System</h2>
            <div class="card">
                Community Social Contribution & Finance Tracking System
            </div>
        </div>

        <!-- ================= FINANCE ================= -->
        <div id="financeSection" style="display:none;">
            <h2>Finance Tracker</h2>

            <input type="text" id="desc" placeholder="Description">
            <input type="number" id="amount" placeholder="Amount">

            <button onclick="addFinance()">Add</button>

            <ul id="financeList"></ul>
        </div>

        <!-- ================= ACCOUNTS ================= -->
        <div id="accountsSection" style="display:none;">
            <h2>Accounts</h2>

            <div class="card">Cash Account</div>
            <div class="card">Bank Account</div>
            <div class="card">Mobile Money</div>
        </div>

        <!-- ================= REPORTS ================= -->
        <div id="reportsSection" style="display:none;">
            <h2>Monthly Reports</h2>

            <div class="card">
                Reports and analytics coming soon...
            </div>
        </div>

    </div>
</div>

<!-- ========================================================= -->
<!-- ===================== JAVASCRIPT ======================== -->
<!-- ========================================================= -->
<script>

    /* ===================== PAGE NAVIGATION ===================== */
    function showPage(page) {
        document.getElementById("loginPage").style.display = "none";
        document.getElementById("registerPage").style.display = "none";
        document.getElementById("forgotPage").style.display = "none";
        document.getElementById("dashboard").style.display = "none";

        document.getElementById(page).style.display = "block";
    }

    /* ===================== DASHBOARD SECTIONS ===================== */
    function showSection(section) {
        document.getElementById("homeSection").style.display = "none";
        document.getElementById("financeSection").style.display = "none";
        document.getElementById("accountsSection").style.display = "none";
        document.getElementById("reportsSection").style.display = "none";

        document.getElementById(section + "Section").style.display = "block";
    }

    /* ===================== REGISTER ===================== */
    function register() {
        let u = document.getElementById("regUser").value;
        let p = document.getElementById("regPass").value;

        let users = JSON.parse(localStorage.getItem("NSC_users")) || [];

        users.push({ username: u, password: p });

        localStorage.setItem("NSC_users", JSON.stringify(users));

        alert("Account created!");
        showPage("loginPage");
    }

    /* ===================== LOGIN ===================== */
    function login() {
        let u = document.getElementById("loginUser").value;
        let p = document.getElementById("loginPass").value;

        let users = JSON.parse(localStorage.getItem("NSC_users")) || [];

        let user = users.find(x => x.username === u && x.password === p);

        if (user) {
            localStorage.setItem("NSC_currentUser", u);
            loadDashboard();
        } else {
            alert("Invalid login");
        }
    }

    /* ===================== RESET PASSWORD ===================== */
    function resetPassword() {
        let u = document.getElementById("fpUser").value;
        let p = document.getElementById("fpPass").value;

        let users = JSON.parse(localStorage.getItem("NSC_users")) || [];

        let user = users.find(x => x.username === u);

        if (!user) {
            alert("User not found");
            return;
        }

        user.password = p;

        localStorage.setItem("NSC_users", JSON.stringify(users));

        alert("Password updated");
        showPage("loginPage");
    }

    /* ===================== LOAD DASHBOARD ===================== */
    function loadDashboard() {
        let currentUser = localStorage.getItem("NSC_currentUser");

        if (!currentUser) {
            showPage("loginPage");
            return;
        }

        document.getElementById("userInfo").innerText = "User: " + currentUser;

        showPage("dashboard");
    }

    /* ===================== FINANCE MODULE ===================== */
    let financeData = JSON.parse(localStorage.getItem("NSC_finance")) || [];

    function addFinance() {
        let desc = document.getElementById("desc").value;
        let amount = document.getElementById("amount").value;
        let user = localStorage.getItem("NSC_currentUser");

        financeData.push({ user, desc, amount });

        localStorage.setItem("NSC_finance", JSON.stringify(financeData));

        displayFinance();
    }

    function displayFinance() {
        let list = document.getElementById("financeList");
        list.innerHTML = "";

        let user = localStorage.getItem("NSC_currentUser");

        financeData
        .filter(x => x.user === user)
        .forEach(item => {
            let li = document.createElement("li");
            li.innerText = item.desc + " : " + item.amount;
            list.appendChild(li);
        });
    }

    /* ===================== LOGOUT ===================== */
    function logout() {
        localStorage.removeItem("NSC_currentUser");
        showPage("loginPage");
    }

    /* ===================== AUTO START ===================== */
    window.onload = function() {
        loadDashboard();
        displayFinance();
    };

</script>

</body>
</html>
