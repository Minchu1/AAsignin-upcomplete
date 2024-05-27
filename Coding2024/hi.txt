// Import the functions you need from the SDKs you need
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
import { getAuth, createUserWithEmailAndPassword } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-auth.js";
import { getDatabase, ref, set } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-database.js";

// Your web app's Firebase configuration
const firebaseConfig = {
  apiKey: "AIzaSyAe3Ds40992d55G9_H0zqDff0nbi9zkS4I",
  authDomain: "aviaa-e4a59.firebaseapp.com",
  projectId: "aviaa-e4a59",
  storageBucket: "aviaa-e4a59.appspot.com",
  messagingSenderId: "41825275937",
  appId: "1:41825275937:web:e1ce96aecaa5dd8fff9360"
};

// Initialize Firebase
const app = initializeApp(firebaseConfig);

// Initialize Firebase services
const auth = getAuth(app);
const database = getDatabase(app);

// Register function
function register() {
  const username = document.getElementById('username').value;
  const email = document.getElementById('email').value;
  const password = document.getElementById('password').value;

  if (!validateEmail(email) || !validatePassword(password)) {
    alert('Invalid email or password');
    return;
  }

  if (!validateField(username)) {
    alert('Invalid username');
    return;
  }

  createUserWithEmailAndPassword(auth, email, password)
    .then((userCredential) => {
      const user = userCredential.user;
      const databaseRef = ref(database, 'users/' + user.uid);

      const userData = {
        email: email,
        username: username,
        last_login: Date.now()
      };

      return set(databaseRef, userData);
    })
    .then(() => {
      alert('User created successfully');
    })
    .catch((error) => {
      alert('Error: ' + error.message);
    });
}

// Validate email
function validateEmail(email) {
  const expression = /^[^@]+@\w+(\.\w+)+\w$/;
  return expression.test(email);
}

// Validate password
function validatePassword(password) {
  return password.length >= 6;
}

// Validate field
function validateField(field) {
  return field != null && field.length > 0;
}

// Add event listener to submit button
document.getElementById('submit').addEventListener("click", function(event) {
  event.preventDefault();
  register();
});
