
<!DOCTYPE html>
<html>
<head>
    <title>NYABIKENKE SOCIAL CONTRIBUTION </title>
<h3>NSC - Nyabikenke Social Contribution</h3>
<p>Track your monthly contributions, savings, and financial records easily.</p>
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

// ===================== LOGOUT =====================
function logout() {
    localStorage.removeItem("currentUser");
    showPage("loginPage");
}

// ===================== AUTO LOGIN =====================
// When page loads, check if user is already logged in
window.onload = function() {
    loadDashboard();
};
</script>

</body>
</html>
