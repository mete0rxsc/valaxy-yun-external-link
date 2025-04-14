# valaxy-yun-external-link  

`valaxy-yun-external-link` is a Vue component for security verification, designed to validate whether user-input URLs are safe. It is compatible with Valaxy's native Yun theme.  

### Features  

- **Security Verification**: Intercepts user clicks on article hyperlinks via Vue components and redirects them to an in-site notification page.  
- **Customizable Configuration**: Allows users to customize settings such as CSS styles for the redirect page, page title, and description.  
- **Base64 Encryption**: Uses Base64 encryption for URL parameters to protect user privacy.  
- **Console Logging**: Outputs debug information in the console for developer convenience (e.g., "Opening external link: " when a link is clicked).  
- **Compatibility**: Works seamlessly with Twikoo comments to prevent malicious links.  
- **Smart Detection**: Automatically detects internal site URLs. If a user clicks an internal link, it directly opens the page without redirection.  

### Usage  
1. Clone the `valaxy-yun-external-link` repository to your local machine.  
2. Copy all files (except `README.md` and `node_modules`) to the root directory of your Valaxy Yun theme.  
3. Restart `pnpm valaxy` in the terminal to load the new component.  
4. Check if the component works by visiting your site.  


> [!TIP]  
> If the component fails to load or causes errors on the homepage, delete the files in the `components` and `layouts` folders.  
> If issues persist, place the `node_modules` folder in the root directory of your Valaxy theme.  


### Folder Structure  

```  
/--  
  |-- components/                            # Vue components folder  
    |-- LinkInterceptor.vue                  # Vue component file  
  |-- layouts/                               # Vue layouts folder  
    |-- post.vue                             # Vue layout file for article pages  
  |-- node_modules/                          # Node modules folder (use only if errors occur)  
  |-- README.md                              # Documentation  
  |-- public                                 # Public resources folder  
    |-- external-link.html                   # Redirect page HTML file  
```  

### Customization Suggestions  
1. To modify the background color of the redirect page, edit:  
   - The `background-color` value in the `<body>` tag's `style` attribute in `public/external-link.html`.  
   - The corresponding values in lines 171-197 of `public/external-link.html`.  

   Relevant code:  
   ```html  
   // Set background color, text color, and background lines based on mode  
   if (mode === 'normal') {  
     document.body.style.backgroundColor = '#FFFFFF'; // White background  
     document.body.style.color = '#000'; // Black text  

     // Adjust button styles  
     const buttons = document.querySelectorAll('.btn');  
     buttons.forEach(button => {  
       button.style.backgroundColor = '#333'; // Dark background  
       button.style.color = '#FFF'; // Light text  
     });  

     // Set background lines to gray  
     document.body.style.backgroundImage = `  
       linear-gradient(rgba(128, 128, 128, 0.1) 1px, transparent 1px),  
       linear-gradient(90deg, rgba(128, 128, 128, 0.1) 1px, transparent 1px)  
     `;  
   } else {  
     document.body.style.backgroundColor = '#222'; // Default dark mode background  
     document.body.style.color = '#FFF'; // White text  

     // Restore default background lines  
     document.body.style.backgroundImage = `  
       linear-gradient(rgba(255, 255, 255, 0.1) 1px, transparent 1px),  
       linear-gradient(90deg, rgba(255, 255, 255, 0.1) 1px, transparent 1px)  
     `;  
   }  
   ```  

2. To modify the title and description of the redirect page, edit lines 83-95 in `public/external-link.html`.  

   Relevant code:  
   ```html  
   <body>  
     <div class="main-container">  
       <div class="button-group">  
         <button id="direct-link" class="btn">Proceed</button>  
         <a href="/" class="btn">Return Home</a>  
       </div>  
       
       <div class="content-box">  
         <h2>You are leaving for an external site</h2>  
         <p>You are about to visit: <strong id="external-url"></strong></p>  
         <p>Redirecting in <span id="countdown">5</span> seconds...</p>  
       </div>  
     </div>  
   ```  


> [!WARNING]  
> Avoid modifying the Vue code unless necessary, as it may cause the component to malfunction or trigger repeated tab-opening bugs.  


### Issue Reporting  
For any issues, please submit an issue or contact the author via email: <p><a :href="`mailto:Mete0r_xsc@hotmail.com`">Contact Me</a></p>.  
Alternatively, visit the website for support: [https://www.xscnas.top](https://www.xscnas.top).