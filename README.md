
HTTP Posted File Helper V.1.0.2

Nuget Link (https://www.nuget.org/packages/HttpPostedFileHelper/)

This is a light weight library that helps in the posting of files to IIS Webserver by providing a helper class <code>FileHElper.cs</code> which contains overloaded methods <code>ProcessFile()</code> & <code>ProcessFileAsync()</code>which reduces the boilerplate in posting files.

**Installing..**<br>
<code>  PM> Install-Package HttpPostedFileHelper </code>

**Usage**<br>

     //Reference the Library
      using HttpPostedFileHelper;

**Processing Single Files (Basic Usage)**
    
    [HttpPost]  
    [ValidateAntiForgeryToken] 
     public ActionResult UploadFile(HttpPostedFileBase file)
          {
             //Instanciate the Filehelper class to create a Filehelper Object
             FileHelper filehelper = new FileHelper();
             filehelper.ProcessFile(file, "/MyTargetLocation");
               return view();
           }

     
  **Processing Multiple Files**<br>
   This makes provision for multiple files being uploaded to the server with an overridden method
   for Processing an <code>IEnumerable</code> of files (HttpPostedFileBase Collection)
                  
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult UploadFile(Model model, IEnumerable<HttpPostedFileBase> file)
        {
            FileHelper filehelper = new FileHelper();
            // ProcessFile returns count of files processed           
            int postedfiles = filehelper.ProcessFile(file, "/MyTargetLocation");
            if (postedfiles > 0)
            {
                //files were written successfully
            }           
            return View("Home");
        }

    
   **Asynchronous File Processing**<br>
       Processing files can be done Asynchronously, this allows large files to be processed in the background thread freeing the main          thread
             
           [HttpPost]
           [ValidateAntiForgeryToken]
           public async Task<ActionResult> UploadFile(Model model, IEnumerable<HttpPostedFileBase> file)
           {
              FileHelper filehelper = new FileHelper();          
              await filehelper.ProcessFileAsync(file, "/MyTargetLocation");
              //you can do some other work while awaiting          
               return View("Home");
            }
          
          
   **Reject File Extensions During Upload**<br>
       You can specify the file types to be rejected during an upload by supplying a <code>string</code> of the file extensions         
               
             [HttpPost]
             [ValidateAntiForgeryToken]
             public async Task<ActionResult> UploadFile(Model model, IEnumerable<HttpPostedFileBase> file)
              {
                FileHelper filehelper = new FileHelper();
                string reject = ".jpeg,.png,.svg";
                int postedfiles = await filehelper.ProcessFileAsync(file, "/MyTargetLocation",reject);
                //you can do some other work while awaiting   
                if (postedfiles > 0)
                 {
                    //files were written successfully
                  }   
                 return View("Home");
              }
  
