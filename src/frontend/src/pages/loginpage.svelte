<script>
    import { createEventDispatcher } from 'svelte';
    import {loginValidation} from "../dataservices";

    import {  navigate } from "svelte-routing";
  
    let username = '';
    let password = '';
   // let role = 'pupil';
  
    //const dispatch = createEventDispatcher();

    // async function sleep(ms) {
    //   return new Promise(resolve => setTimeout(resolve, ms));
   // }
  
    async function handleSubmit(event) {
      event.preventDefault();
      console.log("dispatched")
      // // Dispatch an event with the login details
      // dispatch('login', { username, password });

      const res = await loginValidation(username, password)
      //await sleep(5000).then(async () => { console.log("sleep ended") });
      console.log("res", res);
      localStorage.setItem('user', JSON.stringify(res))
      const role = res?.role;
      if (res?.login){
        if (role  === 'admin') {
          //navigate('/');
          window.location.href = '/'
        } else if (role  === 'user') {
          navigate('/users');
        }
        else{
          console.log("login failed")
          alert("Login Failed - Internal Error")
          console.log(role)
        }
      } else{
        alert("Login Failed - Incorrect Username or Password")
      }
    }
</script>
  
  <main>
    <h1>Login Page</h1>
    <form on:submit={handleSubmit}>
      <div>
        <label for="username">Username:</label>
        <input type="text" id="username" bind:value={username} required>
      </div>
      <div>
        <label for="password">Password:</label>
        <input type="password" id="password" bind:value={password} required>
      </div>
      <!-- <div>
        <label>Role:</label>
        <label>
          <input type="radio" name="role" value="admin" bind:group={role}>
          Admin
        </label>
        <label>
          <input type="radio" name="role" value="pupil" bind:group={role}>
          Pupil
        </label>
      </div> -->
      <button type= "submit"  >Submit</button>
    </form>
  </main>
  
  <style>
    form {
      display: flex;
      flex-direction: column;
      max-width: 300px;
      margin: 0 auto;
    }
  
    div {
      margin-bottom: 15px;
    }
  
    label {
      margin-right: 10px;
    }
  
    input[type="text"],
    input[type="password"] {
      width: 100%;
      padding: 8px;
      box-sizing: border-box;
    }
  
    button {
      padding: 10px;
      background-color: #007bff;
      color: white;
      border: none;
      cursor: pointer;
    }
  
    button:hover {
      background-color: #0056b3;
    }
  </style>
  