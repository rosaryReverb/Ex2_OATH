<script>
     import { onMount } from 'svelte';
     import { jwtDecode } from "jwt-decode";
   
     let userInformation = null;

     function convertToLocaleString(unixTime) {
    return new Date(unixTime * 1000).toLocaleString();
  }

   
     onMount(() => {
       globalThis.handleCredentialResponse = async function (responseFromGoogle) {
         console.log(responseFromGoogle);
   
         const decodedToken = jwtDecode(responseFromGoogle.credential);
   
         userInformation = {
           email: decodedToken.email,
           name: decodedToken.name,
           iat: convertToLocaleString(decodedToken.iat),
           nbf: convertToLocaleString(decodedToken.nbf),
           exp: convertToLocaleString(decodedToken.exp),
           iss: decodedToken.iss,
         };
       };
     });
   </script>
   
   <svelte:head>
     <script src="https://accounts.google.com/gsi/client" async defer></script>
   </svelte:head>
   
   <div id="g_id_onload"
        data-client_id="825129551956-j701gekcnva2h1eff7qr7q63s3rt1ma1.apps.googleusercontent.com"
        data-callback="handleCredentialResponse">
   </div>
   <div class="g_id_signin"
        data-type="standard"
        data-size="small"
        data-theme="outline"
        data-text="sign_in_with"
        data-shape="rectangular"
        data-logo_alignment="left">
   </div>
   
   {#if userInformation}
     <div>
       <h2>User information:</h2>
       <p>Email: {userInformation.email}</p>
       <p>Name: {userInformation.name}</p>
       <p>Issued at time (iat): {userInformation.iat}</p>
       <p>Not before (nbf): {userInformation.nbf}</p>
       <p>Expires (exp): {userInformation.exp}</p>
       <p>Issuer (iss): {userInformation.iss}</p>
     </div>
   {/if}
   