Features of HTTP Posted File Helper V.1.0.1

Below are the features of the library and code spinets to show how it can be used

**Processing Single Files (Basic Usage)**<br />
    
    [HttpPost]   
     public ActionResult UploadFile(HttpPostedFileBase file)
          {
            FileHelper helper = new FileHelper();
             filehelper.ProcessFile(file, "{Name of Existing or New Directory}");
               return view();
        }

     
  **Processing Multiple Files**
   This makes provision for multiple files being uploaded to the server with an overridden method
   for Processing an <code>IEnumerable</code> of files (HttpPostedFileBase Collection)
    
                 
        [HttpPost]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> UploadFile(Model model, IEnumerable<HttpPostedFileBase> file)
        {
            FileHelper filehelper = new FileHelper();
            // ProcessFileAsync returns count of files processed           
            int postedfiles = await filehelper.ProcessFileAsync(file, "~/PostedFiles", ".pdf,.jpeg");
            //you can do some other work while awaiting
            if (postedfiles > 0)
            {
                //files were written successfully
            }           
            return View("Home");
        }

    
   **Asynchronous File Processing**
       Processing files can be done Asynchronously, this allows large files to be processed in the background thread freeing the main          thread
             
           [HttpPost]
           [ValidateAntiForgeryToken]
            public async Task<ActionResult> UploadFile(Model model, IEnumerable<HttpPostedFileBase> file)
           {
            FileHelper filehelper = new FileHelper();          
            int postedfiles = await filehelper.ProcessFileAsync(file, "~/PostedFiles", ".pdf,.jpeg");
            //you can do some other work while awaiting          
            return View("Home");
          }
  
