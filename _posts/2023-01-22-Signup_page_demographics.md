---
toc: true
comments: true
layout: post
title: Signup Page demographics
description: Team teach about SASS Javascript login and signup page
courses: { compsci: {week: 20} }
type: hacks
---



<html lang="en">

<head>
<script>
    //import { uri, options } from '{{site.baseurl}}/assets/js/api/config.js';

    function signUp_user() {
        const enteredName = document.getElementById("name").value;
        const enteredUid = document.getElementById("uid").value;
        const enteredPassword = document.getElementById("password").value;
        const enteredDOB = document.getElementById("DOB").value;
        console.log("Name = " + enteredName)
        console.log("Uid = " + enteredUid)
        console.log("Password = " + enteredPassword)
        console.log("DOB = " + enteredDOB)
        const signupHeaders = new Headers();
      signupHeaders.set('111', '222');
      
      signupHeaders.set("Accept", "*/*");
      signupHeaders.set("Accept-Language", "en-US,en;q=0.9");
      signupHeaders.set("Content-Type", "application/json");

        signUp_api(enteredName, enteredUid, enteredPassword, enteredDOB)
        
      }
    

    function signUp_api(name, uid, pw, DOB){
      let signupHeaders = new Headers();
      signupHeaders.append('111', '222');
      
      signupHeaders.append("Accept", "*/*");
      signupHeaders.append("Accept-Language", "en-US,en;q=0.9");
      signupHeaders.append("Content-Type", "application/json");
      

      var raw = JSON.stringify({
          "name" : name,
          "uid": uid,
          "password": pw,
          "DOB": DOB
        });

      var requestOptions = {
          method: 'POST',
          headers: signupHeaders,
          body: raw,
          redirect: 'follow'
        };

      fetch("http://localhost:8084/api/users/", requestOptions)
          .then(response => {
            if (response.ok) {
                console.log("Successfully Signed Up");
                alert("Account has been created. You will be directed to login page shortly.");
                window.location.href = "https://jbaza12.github.io/JaredsBlog/"
              } else {
                console.error("Sign Up Failed");
                // You can handle failed login attempts here
                const errorMessageDiv = document.getElementById('errorMessage');
                errorMessageDiv.innerHTML = '<label style="color: red;">User Sign Up Failed</label>';
              }
          })
          .then(result => { 
            console.log(result);
            
            })
          .catch(error => console.log('error', error));
          

      
      //return response
    }


  </script>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Login Page</title>
  <link rel="stylesheet" href="styles.css"> <!-- Include the compiled CSS file -->
</head>

<body>
    <div class="container">
    <form action="javascript:signUp_user()">
    <p><label for="Name">First Name:</label>
     <input type="text" id="name" placeholder="Your First Name" />
    </p>
    <p><label for="uid">User ID:</label> 
    <input type="text" id="uid" placeholder="User ID" />
    </p>
    <p><label for="password">Password:</label>
    <input type="password" id="password" placeholder="Password" />
    </p>
    <p><label for="DOB">Date Of Birth:</label>
    <input type="text" id="DOB" placeholder="Date of Birth (YYYY-MM-DD)" />
    </p>
    <button class="button-spacing">Submit</button>
    </form>
  </div>
   

   
</body>

</html>
